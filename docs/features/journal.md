# The audit chain and the trade journal

*Why did Arvo own 137 shares at 14:37:22?* needs one chain: bar → signal →
gate decision → order → acknowledgement → fill → position. Arvo records it
twice, in the same words.

## In a session's record

Each event names its cause: a `signal` names the bar it was decided on
(`id` is `<bar>#<n>`), `submitted`, `refused` and `exit` name the signal, a
`filled` names the order and the position it left, a `reconciled` names the
whole book after. So the record is a chain rather than a log, and one
reader walks it:

```
arvo-engine session explain <finding>@<executor> 2026-09-21T14:00:00Z

AAPL.AIEX: 297 at 2026-09-21T14:00:00+00:00
  fill 1 of 1
    bar       2026-09-21T13:45:00 close 335.73
    signal    2026-09-21 13:45:00#0  buy 297 @ 335.73  rule: close above the opening range = 0.101  regime: ranging
    gate      accepted; order fe043155-… acknowledged 2026-09-21T13:55:11
    fill      buy 297 @ 335.89 at 2026-09-21T13:55:12  (decided @ 335.73 → position 297)
```

A position the session adopted through a reconcile says so instead of
inventing a bar behind it. [How to explain a position →](../how-to/explain.md)

## Losses, grouped

A single loss is a ledger row. What says *how* a rule is failing is losses
grouped by the condition that fired, the regime at entry and the exit
reason, with the finding's out-of-sample figure for the same group beside
each. "Stop exits in ranging regimes" is a diagnosis; a list of red rows is
not. The Trades tab and the after-close review both show the grouping.

## The operator's behaviour

The record also holds what the person did: positions adopted because
something traded by hand, halts and resumes, a session stopped after a
losing day, a ruleset edited while a session ran on it. The after-close
review counts these beside the day's P&L, because intervening after losses
is the most common way a Supported rule underperforms its backtest.

## On every trade

Backtest and live alike, each trade carries a **journal**: the condition
that fired in the rule's words, the value it was judged on (a spread, a
distance in ATRs, a deviation), the regime the instrument was in at entry
(trending up, trending down, ranging) and the quantity the rule asked for
before the gate sized it. The **Trades** tab of a finding shows them; the
CSV export writes them; an agent asked *why did this trade fail* has
something to reason from.
