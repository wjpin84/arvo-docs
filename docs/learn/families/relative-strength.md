---
title: Relative strength
description: Level 6 — ranking instruments against each other instead of judging each on its own, and the survivorship trap underneath it.
---

# Relative strength

*Level 6 — strategy families.*

## The claim

> Out of a group, the few that rose the most keep outrunning the rest.

Every other family in this section judges one instrument on its own history. This
one is different in kind: it **ranks a set** and holds the top few, whatever their
individual charts look like.

The distinction matters more than it sounds. A rule asking "has this gone up?" can
be satisfied by a rising market lifting everything. A rule asking "has this gone up
*more than its peers*?" cannot — in a market where everything rose 20%, the ranking
still separates the field. You are betting on relative performance, and taking a
position on the market itself only incidentally.

## Why anyone believes it

**Cross-sectional momentum is among the most replicated results in finance.**
Ranking a universe by trailing return and holding the top decile has been
documented across decades, countries and asset classes. Whatever else is arguable,
the pattern is not obscure.

**Money chases relative performance.** Funds are measured against peers and
benchmarks, so capital flows toward what is outperforming. The flow is the
mechanism, and it operates on relative rank rather than absolute return.

**Ranking cancels the market.** A shared move affects every member equally and
drops out of the comparison. What is left is the differences between names, which is
a cleaner signal than any single name's path.

!!! note "Say the cost"
    This family's documented edge came with brutal, occasional reversals —
    "momentum crashes" that arrive at turning points and undo years in weeks. The
    long-run record is strong *and* the path is not something most accounts can
    hold through. Both halves are the finding.

## What it looks like

No chart. That is the honest answer, and it is why this is the hardest family to
learn visually: the signal is a **table**, sorted.

| Rank | Instrument | Return over `lookback` | Held? |
|---|---|---|---|
| 1 | one of the set | +31% | yes |
| 2 | another | +24% | yes |
| 3 | another | +18% | no, with `hold_top: 2` |
| … | … | … | no |

`cross_sectional_momentum` takes two numbers:

| Parameter | Means |
|---|---|
| `lookback` | how many bars the ranking return is measured over |
| `hold_top` | how many of the top-ranked names to hold, and nothing else |

Rebalancing follows the ranking: a name that falls out of the top `hold_top` is
sold and the new entrant bought.

## When it fails

**The reversals are the whole risk.** At a market turn, yesterday's leaders become
the worst performers, and a rule holding exactly those names takes the full hit
simultaneously across every position. Your diversification is nominal — the
positions were selected by the same criterion, so they fail together.

**`hold_top` too small is a coin toss.** Holding the top 2 of a set of 5 is a
concentrated bet on two names. Arvo refuses the degenerate cases outright, which
is worth knowing before you try them.

**Turnover is high.** Rankings churn, and every change is a sale and a purchase,
both paying costs. A short `lookback` makes this much worse.

**And the trap that sinks most attempts at this family: survivorship.** If your set
is "today's index members", you have selected on survival, and every name that
failed is absent. The rule will look excellent, and the result is worthless. This
family is more exposed to that trap than any other, because a *set* is exactly the
thing people assemble carelessly.

!!! warning "Arvo cannot detect this for you, and says so"
    Deflation corrects for *searching*. A universe of today's index members
    involved no search, so every gate passes such a result honestly. This is why a
    [universe](../../features/universes.md) in Arvo **refuses to exist without a
    written reason** for its membership, and a file without one is rejected. That
    written reason is the only available defence, and it is yours to write. "The
    S&P 100 as constituted in January 2024, before the test window" is a reason.
    "Large US tech names" is not.

## Test it

This rule cannot be run as an ordinary study. Arvo refuses it:

> *Cross-sectional momentum ranks instruments against each other and needs more
> than one; run it as a book*

And a book of two or three is refused as well:

> *Cross-sectional momentum ranks instruments against each other; 4 is the fewest a
> ranking says anything about, and this has 3*

So: define a [universe](../../features/universes.md) with its stated reason, then
run a **book** — many instruments, one account, one gate. A ruleset over the shipped
defaults:

```json
{
  "name": "leaders",
  "label": "Cross-sectional momentum",
  "premise": "The strongest few of a stated set keep outrunning it, net of the turnover.",
  "interval": { "step": 1, "unit": "day" },
  "kind": {
    "kind": "grid",
    "rule": "cross_sectional_momentum",
    "fixed": { "trade_size": 10.0 },
    "axes": { "lookback": [20.0, 60.0, 120.0], "hold_top": [2.0, 3.0] }
  }
}
```

Six configurations, so the winner is judged as the best of six.

What to read:

1. **Your universe's reason, before any number.** If you cannot defend the
   membership in a sentence that does not reference performance, stop — the result
   cannot mean anything, and no part of Arvo will catch it.
2. **Which members were silent.** A book reports them. A result driven by two of
   twenty names is a concentrated bet wearing a portfolio's clothes.
3. **The regime is the book's, not any member's.** For a book, Arvo labels the
   regime from the benchmark holding every member — so a name that ranged while the
   book trended is not visible. A stated limit, not a hidden one.
4. **The drawdown, hard.** This family's failure is a fast simultaneous one. Check
   it against the [drawdown table](../risk-and-sizing.md).

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — cross-sectional versus time-series momentum, side by side.
- *(to be chosen)* — a momentum crash, with the leaders before and after.

## Next

[Opening range](opening-range.md) — the first minutes of a session.
