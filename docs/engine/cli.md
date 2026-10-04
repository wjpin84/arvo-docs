# Command line

The engine's binary *is* the command line. There is no separate `arvo`
executable, by decision: a second binary would be a second place for the
argument parsing, the discovery and the token handling to drift.

```
arvo-engine [<data-dir>]                        serve; the app data directory when none is given
arvo-engine [<data-dir>] shutdown
arvo-engine [<data-dir>] session list
arvo-engine [<data-dir>] session start <finding> <executor>
arvo-engine [<data-dir>] session stop|reconcile|resume <id>
arvo-engine [<data-dir>] session halt <id>|--all [reason...]
arvo-engine [<data-dir>] session explain <id> <time>
arvo-engine [<data-dir>] review [YYYY-MM-DD]
arvo-engine [<data-dir>] rank [--rule R] [--instrument I]
arvo-engine [<data-dir>] universes [refresh]
arvo-engine [<data-dir>] pine <file> [--interval 1day] [--keep]
arvo-engine [<data-dir>] option-quotes spreads <symbol>
arvo-engine [<data-dir>] option-quotes compact
arvo-engine [<data-dir>] views [--print]
arvo-engine [<data-dir>] evidence rewrite
arvo-engine help
```

Every verb talks to the engine serving `<data-dir>` — the one that wrote
`engine.json` there — or to the app data one when you give no directory. Give
the directory whenever you run more than one project, or the verb will reach a
different engine than you meant.

## Reading what you have

### `rank` — the leaderboard

Every comparable finding in one order, so two rules are compared the same way
every time.

```sh
arvo-engine ~/arvo rank
arvo-engine ~/arvo rank --rule sma_cross
arvo-engine ~/arvo rank --instrument MSFT.YF
```

The order is not "highest return". It is: findings that survived **under
conservative costs** first, then those that were `Supported` only at stated
costs, then the rest; ties broken by drawdown, then by trade count. A rule that
looks better on paper and worse once costs are doubled ranks below its
neighbour, deliberately. [Verdicts and advice →](../reference/verdicts.md)

### `universes` — coverage, and filling it

```sh
arvo-engine ~/arvo universes            # what the project asks for, and what is missing
arvo-engine ~/arvo universes refresh    # fetch what is missing or behind
```

!!! note "`refresh` fetches the whole reach, every time"
    Not an incremental top-up. A vendor write **replaces** a series file, so a
    short refetch would truncate a ten-year history to whatever window it asked
    for. Fetching the full reach is slower and cannot silently destroy history.

### `review` — after the close

```sh
arvo-engine ~/arvo review               # today
arvo-engine ~/arvo review 2026-09-25    # a given day
```

Written under `reviews/` and printed. It covers every session's fills against
the price the rule decided at, what the gate refused and why, the stretches a
session was frozen or halted, and what a person did by hand.

### `session explain` — the chain behind a position

```sh
arvo-engine ~/arvo session explain twin_cross@alpaca-paper 2026-09-25T14:35:00
```

For everything held at that instant: bar → signal → gate → order → fill →
position, with the rule that fired, the value it fired on, and the regime. This
is the one verb that needs no running engine — it reads the record on disk.

### `option-quotes` — the recorded chains, read

The engine records option chains because no vendor serves a quote after the
day. `spreads` reads the whole recording for one underlying and prints the
half-spread by premium, beside what the cost model charges:

```sh
arvo-engine ~/arvo option-quotes spreads SPY
```

It changes nothing. The model's constant stays where it is until someone
decides to move it, because moving it re-costs every option study.

`compact` rewrites every finished day of the recording as Parquet, about a
seventeenth of the size. The engine does this by itself each hour; the verb is
for a project whose engine is not running. A day's CSV is removed only after
the Parquet file has been read back and holds the same rows.

```sh
arvo-engine ~/arvo option-quotes compact
```

### `views` — the project in DuckDB

Writes `.arvo/views.sql`, which names each of the project's stores as a DuckDB
view, and says how to open it. `--print` shows the file and writes nothing.
See [Ask your project a question](../how-to/ask-your-project.md).

```sh
arvo-engine ~/arvo views
```

### `evidence rewrite` — an older store, in today's form

A finding is stored as a compact record with its curves in a Parquet file
beside it. A store written before that holds each finding as one indented JSON
file with the curves inside, and is several times larger.

```sh
arvo-engine ~/arvo evidence rewrite
```

Stop the engine first; the verb refuses while one is serving the folder. Each
finding's new form is read back and compared with what was there before its
file is replaced, and a finding the build cannot read is named and left as it
was. Nothing about a finding changes but its format number.

## Running a session

```sh
arvo-engine ~/arvo session list
arvo-engine ~/arvo session start <finding> alpaca-paper
arvo-engine ~/arvo session stop twin_cross@alpaca-paper
```

A session's id is `<finding>@<executor>`, as `session list` prints it, with its
state, its counts, and its verdict against its finding — including the reason
when the live result is diverging from the backtest.

`<executor>` is one of:

| | |
|---|---|
| `alpaca-paper` | Alpaca's paper endpoint. Real API, real fills and latency, simulated money |
| `alpaca-live` | Real money |
| `robinhood-<last4>` | Real money, the account whose number ends in those four digits |

The last four digits are required and are not guessed: `robinhood` alone names
no account and is refused, as is `robinhood-85` — too few digits to identify
one.

!!! warning "Paper is the only thing that measures fill cost"
    A backtest assumes a fill. Alpaca's paper endpoint gives you a real one, at a
    real spread, with real latency — so its divergence from the backtest is a
    *measurement* rather than an assumption. That is why five paper days gate a
    live executor.

### When a session freezes

A session audits its book against the venue every poll. If they disagree it
freezes: it stops taking entries and waits, rather than trading on a position
it is not sure it holds.

```sh
arvo-engine ~/arvo session reconcile twin_cross@alpaca-paper   # square the book against the broker
arvo-engine ~/arvo session resume twin_cross@alpaca-paper      # once you are satisfied
```

[When a session freezes →](../how-to/frozen-session.md)

### `halt` — the kill switch

```sh
arvo-engine ~/arvo session halt --all "spread blew out"
```

This arms the gate, flattens what can be flattened, and **stays halted**. A
restart does not lift it and a new session cannot start under it; a person
clears it deliberately. A limit a restart lifts is not a limit.

!!! danger "A halt reports what it could not do"
    If the venue refuses an exit, the halt says so and names the instrument
    rather than reporting success. Read the output — positions the venue would
    not close are still yours.

## Importing a rule

```sh
arvo-engine ~/arvo pine strategy.pine --interval 1day
arvo-engine ~/arvo pine strategy.pine --keep     # write it under rules/
```

Reads the subset of Pine v5 the rule language can express and **refuses the rest
by name** rather than translating it approximately. Without `--keep` it prints
the translation and writes nothing.

What it refuses, it lists: a strategy using a function Arvo cannot say is
reported as unsupported with that function named, so you know what was dropped
instead of getting a rule that quietly does something else.
[Importing from Pine →](../features/pine.md)

## Stopping the engine

```sh
arvo-engine shutdown
arvo-engine ~/arvo shutdown
```

Asks the engine to stop and waits until it has. On the way out the engine:

1. asks every running session to stop, so each record ends with `stopped`;
2. stops the plugins it started;
3. removes its `engine.json` and exits.

Positions are left as they are. Stopping is not flattening: `session halt` is
the kill switch. A session that does not end within fifteen seconds is named,
and the engine goes anyway.

The verb then says what became of each session:

```
the engine (pid 30312) has stopped
  20260921T134031540-AAPL.AIEX@alpaca-paper: stopped, positions as they were
```

With no engine running it says so and exits 0, so it can sit in front of a
build. Use it before rebuilding the engine: on Windows the binary cannot be
replaced while it runs.

Ending the process with the operating system does none of this. No destructor
runs, the plugins are orphaned, and a session's record simply ends.

A session does not start again by itself when the engine does. Start it with
`session start`.

## Exit codes

| | |
|---|---|
| `0` | did what you asked |
| non-zero | did not, and said why on stderr |

A verb that cannot find an engine says so and names the file it looked in. A
verb refused by the gate prints the gate's reason — a refusal is never silent,
which is the same rule the engine follows internally.

## The MCP server

```sh
arvo-mcp-server [<app-data-dir>] [--agent NAME]
```

Stdout is the protocol; diagnostics go to stderr. See
[With Claude Code](claude.md).
