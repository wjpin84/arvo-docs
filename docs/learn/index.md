---
title: Learn trading
description: Trading from the beginning, where every claim comes with a way to check it.
---

# Learn trading

Most trading education stops where the interesting part starts. It tells you
what a pattern is supposed to do, and leaves you to believe it.

Here every lesson ends with something you can run. Arvo scores the claim on real
bars and tells you whether it held — including when the honest answer is that it
cannot tell. You are not learning a set of strategies. You are learning to test
them, which is the only skill that keeps working after the lesson ends.

No experience assumed. Level 1 starts at what a share is and what happens when
you press buy.

## How a lesson is shaped

Every page, the same six parts, so you can stop at the one you came for:

| | |
|---|---|
| 1 | **The claim** — one sentence, the way a trader would say it |
| 2 | **Why anyone believes it** — the mechanism, not the backtest |
| 3 | **What it looks like** — on a chart, and as the rule's logic |
| 4 | **When it fails** — the market it needs, and the market that kills it |
| 5 | **Test it** — a ruleset to run, or the place in Arvo where you can see it for yourself |
| 6 | **Watch** — video, for what a diagram cannot carry |

The foundation levels, 1 to 5, use the same six beats with the first as *the idea*
rather than a claim, since "what a bid is" is not something to be argued with.

!!! note "If a topic cannot reach step 5, it does not get a page"
    This is a deliberate limit, and it is what keeps this section from becoming a
    textbook nobody finishes. A claim Arvo cannot test is a claim we would be asking
    you to take on faith, and that is the habit this section exists to break. Topics
    that fail the test — chart patterns read by eye, sentiment calls, anything
    needing data Arvo does not keep — are named in the lesson that comes closest,
    and not given a page of their own. [Level 10](what-moves-markets.md) bends this
    rule on purpose and says so at the top.

## The ladder

Ten levels. Each is usable on its own, but the order is the order the ideas depend
on each other.

| Level | You learn | It lands in Arvo as |
|---|---|---|
| 1 | [**Markets**](markets.md) — what you are buying, what an order is, the bid and the ask, who is on the other side | Instruments, executors, the cost model |
| 2 | [**Charts**](charts.md) — bars, candles, timeframes, volume, and the four things a chart cannot show you | Intervals, the dividend gap, pinned data |
| 3 | [**Market types**](market-types.md) — trending, ranging; why one rule cannot suit both | [`regime`](../concepts/trading.md#regime) on every trade |
| 4 | [**Entries and exits**](entries-and-exits.md) — the signal is the easy half; the exit decides the result | A rule's two halves; exits never ask the gate |
| 5 | [**Risk and sizing**](risk-and-sizing.md) — the part that decides whether you survive, before the part that decides whether you profit | [The risk gate](../features/risk.md), `risk.json` |
| 6 | [**Strategy families**](families/index.md) — six shapes, one lesson each | The [rules Arvo ships](../features/rulesets.md#the-rules-arvo-ships) |
| 7 | [**Why a backtest lies**](backtest-lies.md) — overfitting, the cost of searching, look-ahead, survivorship | [Out of sample, deflation, cost tiers](../concepts/trading.md) |
| 8 | [**Reading a verdict**](reading-a-verdict.md) — what `Inconclusive` is telling you, and why it is the common answer | [Verdicts and advice](../reference/verdicts.md) |
| 9 | [**Paper to real money**](paper-to-real.md) — what paper trading measures, and what it cannot | [Divergence, the promotion gate](../features/sessions.md) |
| 10 | [**What moves the market**](what-moves-markets.md) — rates, expectations, flows, microstructure | Why a regime changed under your rule |

Level 6 is the one people arrive for. Levels 7 and 8 are the ones that change how
they trade.

## The six families in level 6

| Family | The claim | Arvo's rule |
|---|---|---|
| [Trend following](families/trend-following.md) | what has gone up keeps going up | `momentum_breakout` |
| [Volatility breakout](families/breakout.md) | a large move is a beginning, not an end | `volatility_breakout` |
| [Mean reversion](families/mean-reversion.md) | price stretched from fair value comes back | `vwap_reversion` |
| [Relative strength](families/relative-strength.md) | the strongest few keep outrunning the rest | `cross_sectional_momentum` |
| [Opening range](families/opening-range.md) | a session's first minutes set its direction | `opening_range` |
| [Selling options](families/selling-options.md) | you are paid to take risk others shed | `put_spread` |

## Not advice

!!! warning "This teaches method, not positions"
    Nothing here is a recommendation to buy or sell anything, and a `Supported`
    verdict is not a prediction. A verdict says a claim survived a specific test on
    specific history — which is the most any test can say. Arvo is built to make
    that limit visible rather than hide it, and this section is written the same
    way.

## Start

- **New to all of this** — [level 1](markets.md), in order.
- **You already trade** — start at [level 7](backtest-lies.md), which is the part
  Arvo does differently, then read the families for the rules it ships.
- **You want the argument first** — [why Arvo is
  different](../concepts/index.md). It is short, and the whole ladder is built on
  it.
