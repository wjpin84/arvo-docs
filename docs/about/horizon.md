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

**Before the open**: overnight and pre-market moves on everything held or
watched, the day's calendar (earnings, releases, expiries), what the rules
will do at the open and what the gate would size them at, and the state of
the plane — accounts, feeds, frozen or halted sessions, stale findings.

**After the close**: every signal and what the gate did with it, every fill
against its decision price (the slippage the venue measured against the
cost the finding assumed), the journal on every trade so the review can say
which condition fired in which regime, refusals by reason, and the day's
P&L against the drawdown halt.

Both are scheduled jobs writing a report into the project, a view in the
window, and a research tool so the agent can be asked *why did today go the
way it did*.

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

## Also on the horizon

Volatility regimes and regime-conditioned rules; Bayesian and genetic
parameter search; DuckDB and Parquet as the data plane; news and event
intelligence; fundamentals and economic data; trades and order-book data;
synthetic scenarios; execution algorithms; factor exposure, VaR and CVaR; a
GPU compute plane; a window built on GPUI.
