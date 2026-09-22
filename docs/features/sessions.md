# Sessions

A session is a finding's rule, running against a venue: one finding, one
executor, one thread in the engine. The rule is rebuilt as a shadow engine
warmed on the library up to today; from then on each completed bar is
pushed through it, entries go to the risk gate and exits go straight to the
venue.

A session started after the open catches up on the bars it missed the same
way: the rule is warmed on them and their signals are on the record, but an
entry from a bar that closed before the session started is refused
(`catch-up: …`), never sent. Yesterday's signal at today's market is not the
trade the finding measured. Exits still go, as under a freeze.

Start one from the **Sessions** tab (choose a finding, choose an executor)
or the command line. The executors are `alpaca-paper`, `alpaca-live` and
`robinhood-<last four>`. Paper is drawn apart from real money everywhere it
appears.

## What a session does every poll

- fetches new bars (or takes them from the stream) and pushes each one the
  moment it is over — a daily bar's signal fires overnight and fills at the
  open, the fill the backtest assumed;
- settles: collects fills and books them on the gate;
- **audits** its book against the venue's, and freezes on a disagreement.

## States

| State | Meaning |
|---|---|
| `starting` | opening the finding, warming the shadow |
| `running` | polling or streaming, taking entries |
| `frozen` | entries wait for a person; exits still go. Either the book disagrees with the venue (reconcile, then resume) or the feed has gone dark (lifts itself when the feed returns) |
| `halted` | the gate stopped the account: the drawdown halt, or the kill switch. Stays until released |
| `stopped` | asked to stop; positions left as they are |
| `failed` | the thread ended with an error; the reason is on the row |

## Reconciliation as an incident

A position the venue reports and the gate does not — a fill this process
never heard, a hand-placed order, a broker-side liquidation — means the rule
can no longer size against a book it trusts. The session **freezes** and
records the discrepancy. **Reconcile** makes the gate's book the venue's
(the venue holds the money); **Resume** takes entries again, and is refused
until a reconcile has happened. Both are events in the record. Nothing
resumes on its own.

## The promotion gate

A real-money executor takes only a finding that earned it. On start, a
session on `alpaca-live` or a Robinhood account is refused unless:

- the finding's verdict is **Supported**;
- the finding has run on `alpaca-paper` for at least **five days**, by the
  paper session's record;
- that paper session was not **Diverging** from the finding when last
  judged.

The refusal names the gate and every reason at once, so they can be fixed
together. Paper needs no promotion. The gate is on the start itself, for
people and agents alike: nothing else creates a session.

The Sessions tab asks the gate as soon as a finding and an executor are
chosen and shows the road under the form: the finding's verdict, the days
on paper and the paper session's verdict, and for a real-money executor
either the reasons it refuses or that it allows. The button reads
**Promote** for real money and stays disabled while the gate refuses. The
same question is on the API as `CheckPromotion`, so an agent given the
control tier can ask before it tries.

## The kill switch

**Halt** arms the gate and then flattens everything the session holds, in
that order, so a proposer racing in is refused rather than opening into the
exit. What the venue would not exit is named in the record and on the
status; the halt stands either way. From a session's row, from the
Operations view, from the tray (**Halt trading**, every session) or from
the command line. [How to halt →](../how-to/halt.md)

## What the fills cost

A paper session exists to measure what a backtest cannot: what a fill
actually costs against the price the decision was made at. Once anything
has filled, the session carries its divergence: mean and worst adverse
slippage in basis points across its fills, with the slippage the finding's
cost model assumed beside it, the mean signal-to-fill latency, and how many
approved orders never filled (never averaged in: a backtest assumes every
order fills). It is under the selected session in the Sessions tab and on
`session list`, and the verdict's `execution` reason reads the same figure.

## When a rule stops working

A finding says whether a rule worked on its out-of-sample window. A session
answers the question that comes next, in the same vocabulary:

| Verdict | Meaning |
|---|---|
| **Holding** | the live trades sit inside what the out-of-sample distribution would produce |
| **Diverging** | expectancy, drawdown or firing rate has left that range; the reason is named |
| **Inconclusive** | too few live trades to say, which is where every session starts |

The expectation comes from the finding: its out-of-sample trade
distribution, its max drawdown, how often the rule fired, and which regimes
its trades landed in. The session's own ledger, with the same journal on
every trade, is compared against it as the trades accumulate. A rule that
fires a quarter as often as it did is a regime change before it is a loss;
entries landing in a regime the finding was never Supported in are a reason
before the money says so.

A verdict is not a risk limit. Diverging changes nothing at the gate. It is
recorded, raised as an alert, shown on the session and in Operations, and
it is the case for demoting the session, decided by a person or the agent.
The drawdown halt and the daily loss limit protect the account; the verdict
judges the rule.

The verdict is on the session's row in the Sessions tab and in Operations,
in `session list` on the command line, and on the record as a `verdict`
event each time it changes. The expectation the session was judged against
is the record's `expectation` event, written at the start. A session on a
walk-forward or a reported finding has no out-of-sample ledger to expect
anything from, and stays Inconclusive; the record says so.

The thresholds, so a verdict can be read: the expectancy comparison waits
for ten closed live trades and fires when the live mean is more than two
standard errors below the finding's; the drawdown reason fires at one and a
half times the finding's out-of-sample maximum, at any trade count; the
frequency reason once four entries were due and fewer than a quarter, or
more than four times, as many came; the regime reason when most entries,
and at least three, landed in a regime the finding never traded in; the
execution reason after five fills, when slippage is past twice what the
cost model assumed.

!!! note "Since engine 0.4.0"
    A session started by an older engine shows an empty verdict.

## The record

Everything a session does is one JSON line per event in
`sessions/<finding>@<executor>.jsonl`, and every event names what caused
it. [The audit chain →](journal.md) · [Record reference →](../reference/session-record.md)
