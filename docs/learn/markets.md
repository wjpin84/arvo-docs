---
title: Markets
description: Level 1 — what you are buying, what an order is, and who is on the other side of it.
---

# Markets

*Level 1. Assumes nothing.*

## The idea

A share is a fraction of a business. When you buy one, someone sold you one —
and that is the part most explanations skip, because everything that goes wrong
later starts there.

There is no "the price". There are two prices at all times: the highest anyone
is currently willing to pay, and the lowest anyone is currently willing to
accept. You buy at the second and sell at the first.

| Term | Means |
|---|---|
| **Bid** | The best price someone will buy from you at |
| **Ask** (or offer) | The best price someone will sell to you at |
| **Spread** | The gap between them. You cross it on the way in *and* on the way out |
| **Last** | What the most recent trade happened at. History, not an offer |

The number on a chart is the last. It is not what you can get.

## Why it matters

**The spread is a fee nobody sends you a bill for.** On a liquid large company
it is a cent on a hundred dollars and you can nearly ignore it. On a thin stock,
or on an option, it can be a percent or more per round trip. A rule that trades
twice a week pays it a hundred times a year.

This is the single most common reason a strategy that looks profitable on a
chart loses money in an account. Not fraud, not bad luck — the spread and the
commission, collected every time, on a margin too thin to carry them.

**Someone on the other side disagrees with you.** Every trade has a buyer and a
seller, both of whom looked at the same public information. You are not
extracting money from "the market" as an abstraction. You are betting that the
person opposite you is wrong, or that they had a different reason for trading —
they needed cash, they were hedging, they were rebalancing a fund. The second
case is where honest profits mostly come from, which is worth knowing before you
assume you have to outsmart anyone.

## What it looks like

Two order types carry almost everything:

| Order | Says | Costs you |
|---|---|---|
| **Market** | "Fill me now, at whatever is available" | Certainty of execution, at an uncertain price |
| **Limit** | "Fill me at this price or better, or don't" | Certainty of price, with no guarantee of a fill |

There is no order type that gives you both, and every apparent exception is one
of these two with conditions attached.

When you send a market order to buy, you take the ask. If your order is larger
than what is offered at the ask, the rest fills at the next price up, and the
next. That difference — between the price you decided at and the price you
actually got — is **slippage**, and it is a real cost that no chart shows.

## What people get wrong

**Assuming the fill matches the screen.** It rarely does, and it is worst exactly
when you most want to trade: at the open, on news, on a breakout. Strength in a
price and a wide spread arrive together.

**Ignoring the round trip.** Beginners compute a trade's profit from entry to
exit price and forget they paid to get in and paid to get out. Two costs, every
trade.

**Believing volume means you can get out.** Volume is what traded, not what is
available to you now. A stock that traded a million shares today can still have
two hundred on the bid.

## See it in Arvo

Arvo keeps the distinction you just learned, in the vocabulary:

- An **instrument** is `SYMBOL.VENUE` — `AAPL.RH`, `MSFT.YF`. The venue names
  where the bars came from, *not* where an order goes.
- An **executor** is where orders actually go: `alpaca-paper`, `alpaca-live`,
  `robinhood-<last four>`. Bars and orders are separate on purpose.
- Every study runs under a stated **cost model** — slippage, commission, a
  per-fill fee, an option's spread. You cannot run a study in Arvo that pretends
  trading is free.
- A paper session **measures** what your fills actually cost against the price
  the decision was made at, and reports it beside what the backtest assumed.
  That comparison is the whole reason paper trading exists here.

Read [connect an account](../getting-started/accounts.md) to get real bars, and
[the glossary](../glossary.md) for instrument, venue, source and executor in one
line each.

## Watch

<!-- TODO: curate. Channel name and year beside each link so a dead link is
     visibly dead; re-check when this page is next edited. -->

- *(to be chosen)* — an order book filling in real time. This is the one idea on
  this page that a video teaches better than prose.
- *(to be chosen)* — market versus limit orders, with the failure case of each.

## Next

[Charts](charts.md) — how price gets drawn, and the four things a chart cannot
show you.
