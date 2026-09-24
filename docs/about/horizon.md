# Where this is going

The **Horizon** project on GitHub is the parking lot: everything Arvo
intends and has not started, kept as issues so it is argued about once.
This page is the shape of it. Nothing here is a promise or a date.

## The end goal: automated trading

Arvo exists so that trading can be automated — a rule that survived
research runs against a venue with no person at each order — and so that
every order it places can be explained back to a bar and a rule. The pieces
are in place: sessions trade rules on their own, one gate sizes every entry,
the record is a chain, and the kill switch is everywhere.

What is not yet in place is the loop around it. "AI trading system" means
two different things, and Arvo has chosen one:

| | The agent sets up the rules that trade | The agent makes the trade |
|---|---|---|
| Who decides an order | the rule, sized by the gate | the agent, in the moment |
| What explains it | a bar, a signal, a finding, a ruleset | a transcript |
| What research says about it | a verdict, deflated against the search | nothing |
| Status | **the path**: today's shape, extended with promotion, session control for the agent, and the daily reviews | not built; would need its own decision, and would still go through the gate |

The steps toward the first: a promotion gate as the only way a finding
becomes a session, for people and agents alike; a control-tier tool list an
agent can be given deliberately (start, stop, reconcile, resume, halt,
promote), separate from the research tools it gets by default; and the two
daily analyses below, as what an operator — human or agent — reads to decide
what to change.

## The two daily analyses

**After the close** is built: [the review](../features/review.md), written
fifteen minutes after the regular close.

**Before the open**: overnight and pre-market moves on everything held or
watched, the day's calendar (earnings, releases, expiries), what the rules
will do at the open and what the gate would size them at, and the state of
the plane — accounts, feeds, frozen or halted sessions, stale findings.

Both are scheduled jobs writing a report into the project, and a research
tool so the agent can be asked what happened. The review after the close
exists; the briefing before the open waits on calendar data.

## All markets

Arvo trades US equities and their options. The end state is forex, stocks,
futures, crypto, commodities, indices, bonds and ETFs — on the working
assumption that **each market uses different strategies**, so each is its
own instrument model (tick, lot, multiplier, hours, margin; expiry and roll
for futures, the quote currency for forex, 24/7 and fractional lots for
crypto), its own sources and executor, its own session calendar, and its
own rules with a control rule the way the crossover is the control for
stocks. ETFs are nearest, being stocks to every provider Arvo has; index
futures are the first non-equity market on the list.

## More providers

Interactive Brokers (most markets, one executor), Polygon or Databento
(data at scale), Tradier or Schwab (more equity and option executors),
OANDA (forex), Coinbase, Kraken or Binance (crypto), and an economic and
bond data vendor. The [provider protocol](../extend/providers.md) is
published, so each is an extension, not a change to the engine. The order
is decided by which market someone wants to run a rule on.

## Feeding the gates first

Everything above is another gate, view or provider. Since 2026-09-24 the
order of work is to widen what the gates see before adding to them
([ADR 0030](https://github.com/wjpin84/arvo-adrs/blob/main/0030-the-gates-are-fed-before-they-are-widened.md)),
because a chain of deflation, walk-forward, conservative costs, a paper gate
and a live verdict is worth exactly as much as what runs through it, and today
that is eight compiled rules on fifteen names.

In order:

1. **Rules as data** ([#225](https://github.com/wjpin84/arvo-desktop/issues/225)): conditions over an indicator library, stored beside the rulesets, so the agent writes rules and not only parameters. A data rule must reproduce the compiled rule's ledger before the compiled one is retired.
2. **Leaderboard** ([#226](https://github.com/wjpin84/arvo-desktop/issues/226)): findings ranked by deflated out-of-sample expectancy under conservative costs, and only that. Raw return is not a column.
3. **Universes** ([#227](https://github.com/wjpin84/arvo-desktop/issues/227)): a few hundred daily names and a handful intraday, each list a file with the reason it was chosen, kept fetched. A panel is deflated against the universe's size.
4. **Pine import** ([#228](https://github.com/wjpin84/arvo-desktop/issues/228)): the subset of Pine v5 the rule language can say, the rest refused by name, so a community's strategies meet the one thing their own tester cannot do.
5. **Chart** ([#229](https://github.com/wjpin84/arvo-desktop/issues/229)): bars, regimes, signals, fills and refusals drawn from the record. It computes nothing and it is not a gate.
6. **Option chains for more underlyings** ([#230](https://github.com/wjpin84/arvo-desktop/issues/230)): the recorder pointed past SPY, so the option rules become candidates.

## Also on the horizon

Volatility regimes and regime-conditioned rules; Bayesian and genetic
parameter search; DuckDB and Parquet as the data plane; news and event
intelligence; fundamentals and economic data; trades and order-book data;
synthetic scenarios; execution algorithms; factor exposure, VaR and CVaR; a
GPU compute plane; a window built on GPUI.
