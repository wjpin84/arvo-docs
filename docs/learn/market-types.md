---
title: Market types
description: Level 3 — trending, ranging, and why the regime a result was earned in matters more than the result.
---

# Market types

*Level 3. Assumes [charts](charts.md).*

## The idea

Markets do two things, and only one at a time: they **go somewhere**, or they
**wander**. Almost every strategy you will meet is a bet on which of those is
currently happening.

Arvo labels three:

| Regime | Means |
|---|---|
| **Trending up** | Net movement is large relative to total movement, upward |
| **Trending down** | The same, downward |
| **Ranging** | Went nowhere in particular — *not* the same as quiet; a range can be violent |

The measure is an **efficiency ratio**: net movement divided by total movement
over the last twenty bars. A path straight up scores 1.0. A path that thrashes
around and ends where it started scores 0.0. Above 0.35, Arvo calls it a trend.

## Why it matters

A rule that works in trends, judged over a window that was half trend and half
chop, gets one number describing neither. Strongly positive in the first half and
strongly negative in the second nets to about zero — and `NotSupported` is then a
perfectly true statement about an average **the market never actually produced**.

This is why the regime is on every trade in Arvo rather than in a footnote. Two
questions look the same and are not:

- *Did this rule make money?* — one number, often meaningless.
- *Where did this rule make its money, and where did it give it back?* — two
  numbers that tell you what you own.

The second question is the difference between having an edge and having a bet on
conditions continuing. Both can be worth taking. Only one of them you can size
honestly.

## What it looks like

| | Trending | Ranging |
|---|---|---|
| Pullbacks | stop short of the last low | run all the way through it |
| New highs | tend to be followed by more | tend to be the top |
| Which family wins | trend following, breakout | mean reversion |
| Which family bleeds | mean reversion — sells every new high in a market that keeps making them | trend following — buys every false start |

Read that table in both columns and you have the central problem of strategy
selection: **the two main families are each other's failure mode.** A trend
follower and a mean reverter given the same instrument and the same year will
disagree on every single trade, and which one was right is a fact about the
regime, not about the rules.

## What people get wrong

**Believing you can see the regime in advance.** You cannot, and this is not a
skill issue. The twenty-bar efficiency ratio needs twenty bars — by the time it
says "trending", the trend is at least a month old and may be over. Every regime
label, including Arvo's, is a statement about the past.

**Filtering a backtest on a regime label.** This is the most flattering form of
cheating available. If the label was computed over the whole window, then
"only trade in trends" uses information from the future, and the resulting curve
is fiction. Arvo is explicit about this: the labels are legitimate for
*describing* a result and illegitimate for *taking* one.

!!! warning "Descriptive, not causal — and the data layer enforces it"
    Arvo's regime labels are computed after the fact, over the benchmark's own
    equity curve. Honest use: *"this rule made its money in the trending third
    and gave it back in the range, so the pooled verdict averages two different
    answers."* Not honest, and not something Arvo licenses: *"so trade it in
    trends."* Acting on that needs to know the regime **before** the bar, which
    is a real-time detector and a much harder thing. A rule may filter on a
    signal series only when that series is marked **causal** — computed from data
    available at that bar — and the distinction is enforced in the data layer,
    not left to your discipline.

**Assuming three buckets is too few.** Arvo deliberately does not split out high
and low volatility: five buckets halve the observations in each, and the question
the labels exist to answer does not need them. Fewer buckets, more evidence per
bucket.

## See it in Arvo

- The **regime at entry** is journalled on every trade, alongside the rule that
  fired and its value. The Trades tab shows it per round trip.
- A finding's result can be **split by regime**, which is how you find out that a
  verdict is an average of two opposite answers.
- A live session carries a `regime` divergence reason: when most of its entries —
  at least three — land in a regime the finding never traded in, the session is
  judged **Diverging** before the money says so.
- The label comes from the **benchmark curve**, so for a [panel or a
  book](../concepts/trading.md#panel) it is the book's regime, not any single
  member's. A member that ranged while the book trended is not visible there, and
  that is a stated limit rather than a hidden one.

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — the same instrument through a trend and then a range, with
  the same rule applied to both.
- *(to be chosen)* — why regime detection in real time is hard.

## Next

[Entries and exits](entries-and-exits.md) — the signal is the easy half.
