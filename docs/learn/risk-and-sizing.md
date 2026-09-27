---
title: Risk and sizing
description: Level 5 — how much to buy, why drawdown arithmetic is brutal, and the gate that sizes every entry in Arvo.
---

# Risk and sizing

*Level 5. Assumes [entries and exits](entries-and-exits.md).*

## The idea

You have been choosing *what* to trade. This level is about *how much*, and it
matters more.

Two traders run the identical rule on the identical instrument over the identical
year. One finishes up 15%, the other is wiped out. The only difference is
position size. There is no entry signal good enough to survive bad sizing, and
there are mediocre signals that make money under good sizing.

The question to answer before every trade is not "how much can I make?" but
**"what does this cost me if I am wrong?"**

## Why it matters

**Drawdown arithmetic is not symmetric.** A loss needs a larger gain to undo it,
and the gap widens fast:

| Fall from peak | Gain needed to recover |
|---|---|
| 10% | 11% |
| 20% | 25% |
| 33% | 50% |
| 50% | 100% |
| 75% | 300% |
| 90% | 900% |

At 50% down you need to double to get back to where you started. This is why
Arvo's default verdict criteria include a **drawdown under 30%** — past that, the
recovery required stops being a trading problem and becomes a fantasy.

**Losing streaks are normal, not evidence of failure.** A rule that wins 40% of
the time will hit six losses in a row about once every two hundred trades. Not if
something goes wrong — as ordinary arithmetic. Your sizing has to be survivable
through a streak you should *expect*, and most beginners size for the average case
and are destroyed by the ordinary one.

## What it looks like

The standard formula, and it is the whole of position sizing:

```
position size = (capital × risk per trade) ÷ (stop distance per unit)
```

Worked, with $50,000, risking 1% per trade, on a stock at $100 whose ATR-based
stop sits $4 below:

```
risk budget     = $50,000 × 0.01  = $500
stop distance   = $4 per share
position size   = $500 ÷ $4       = 125 shares  ($12,500 of stock)
```

Note what happened: a **wider stop buys fewer shares**. The risk stays $500
either way. This is the mechanism that makes a $600 stock and a $20 stock
comparable, and it is why stop distance is measured in ATRs rather than dollars or
percent.

Note also what is *not* in the formula: your confidence, how good the setup looks,
or how the last trade went.

## What people get wrong

**Sizing by capital instead of by risk.** "I put 10% of my account in each
position" sounds disciplined and is not — 10% of your account in a quiet utility
and 10% in a volatile small cap are completely different risks.

**Adding to a loser.** Averaging down increases position size exactly as the
evidence against the position accumulates.

**Ignoring correlation.** Five positions in five different semiconductor
companies is one position in semiconductors, sized five times. The diversification
is nominal, and the loss arrives all at once.

**Trading bigger after a loss to get it back.** Arithmetic does not care about
your intentions, and this is how a survivable drawdown becomes a terminal one.

## See it in Arvo

Arvo does not let sizing be an afterthought: **every entry is a proposal**, and it
passes one gate that sizes it, reduces it, or refuses it with a named reason. The
same gate runs in the backtest and live, so a stored verdict describes a system
that actually exists. Set in `risk.json`, or Arvo's defaults when there is none:

| Field | What it limits |
|---|---|
| `stop_atr_multiple` | stop distance as a multiple of ATR. Not set runs with no stop |
| `risk_per_trade` | fraction of starting capital a stop-out may cost. Needs a stop |
| `max_position_fraction` | fraction of capital one position may occupy. Above 1 is leverage |
| `max_drawdown` | fall from peak that stops the rule for the run. **Permanent** |
| `max_concurrent_positions` | the most positions held at once |
| `max_daily_loss` | realised loss in one day that refuses new entries |
| `correlation_cap` | above this pairwise correlation, names are one bet. **Refuses when unknown** |
| `sector_cap` | the most positions one sector may hold. Refuses an unlabelled name |
| `day_trading` | `pattern_day_trader`: three round trips per five days under $25k |

Three behaviours worth knowing, because they are unusual:

- **A refusal is a fact, not a log line.** `NoStop`, `TooSmall`, `NotWholeLot`,
  `DailyLossLimit`, `TooManyPositions`, `Correlated`, `Halted` — each named on the
  record and counted. "Why didn't it take that trade?" is answerable.
- **The warning band at four fifths.** At 80% of any limit the gate says so
  *before* it acts, writes a `warning` event and raises an alert. This is the one
  moment a person can act before the gate acts for them.
- **The drawdown halt is permanent for the run and lifts for nobody.** The kill
  switch is manual and lifts only when a person releases it. A session that starts
  against an account already holding positions adopts them and comes up **halted**
  until you look — a limit that a restart lifts is not a limit.

Full field reference: [the risk model](../reference/risk-model.md). Why it is one
gate rather than two: [the risk gate](../features/risk.md).

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — position sizing worked through from account to share count.
- *(to be chosen)* — the drawdown table above, and why recovery is harder than the
  fall.

## Next

[Strategy families](families/index.md) — the five shapes Arvo can test, one
lesson each.
