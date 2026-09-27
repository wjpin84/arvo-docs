---
title: What moves the market
description: Level 10 — rates, expectations, flows and microstructure; why regimes change, and the one level Arvo cannot test for you.
---

# What moves the market

*Level 10, the last. Assumes [paper to real money](paper-to-real.md).*

## The idea

Every level so far has treated price as a series to be measured. This one asks why
the series does what it does — because a rule is an assumption about behaviour, and
knowing what produces that behaviour tells you when the assumption has stopped
holding.

!!! note "This level bends the rule of this section"
    [Every lesson here ends in a study](index.md), and most of what follows Arvo
    cannot test: it keeps bars, dividends, quotes and chains, not interest rate
    expectations or fund flows. It earns its place anyway, because the testable
    question is not *"does macro predict prices"* — it is *"did my rule's behaviour
    change when its conditions did"*, and Arvo measures exactly that. The limit is
    named rather than hidden, which is the house rule that outranks the other one.

## Four things that actually move price

### 1. The discount rate

Any asset is worth its future cash flows, valued in today's money. The rate used to
do that valuing is built on what interest rates are expected to be. Change the
expected rate and **every** valuation changes at once, in the same direction.

This is the most important mechanism on the page, and it explains something you have
already met. In [relative strength](families/relative-strength.md) you read that
positions selected by the same criterion fail together. Rate-driven moves are why
diversification across names fails exactly when you need it: the names are different,
the discount rate is shared.

Long-duration assets — those whose value is mostly in distant cash flows — move most.
That is why an unprofitable growth company and a government bond can fall on the same
day for the same reason.

### 2. Expectations, not news

Price responds to the **difference** between what happened and what was expected, not
to whether it was good.

A company reporting record profits can fall 8% on the day, because the market had
priced in better records. This is the single most common source of confusion for new
traders: *"the news was good and it went down"*. The news was good and the
expectation was better. The expectation was already in the price.

The practical consequence: by the time a fact is known to you, it is known to
everybody, and the price you can trade at already contains it.

### 3. Flows — people who must trade

The mechanism behind several of the families in [level 6](families/index.md), and the
most underrated item on this page. Some participants transact regardless of price:

| Who | Why they must |
|---|---|
| Index funds | a name enters or leaves the index; they buy or sell it, at whatever it costs |
| Funds meeting redemptions | investors want cash; positions are sold to raise it |
| Option dealers | short options must be hedged as price moves, mechanically |
| Companies buying back stock | an authorised programme buys on a schedule |
| Anyone stopped out | a stop is an order that fires without regard to value |

None of these are opinions about value. They are constraints, and they push price
away from where two-sided trading would have left it. This is what you are paid for
in [mean reversion](families/mean-reversion.md) and in [selling
options](families/selling-options.md) — supplying what a constrained participant
needs, at a moment that suits them and not you.

### 4. Positioning

When a trade becomes crowded, the people in it are the people who would have to sell.
An unwind is then self-reinforcing: selling triggers stops, stops trigger selling.

This is the mechanism behind the momentum crashes in [relative
strength](families/relative-strength.md) and the correlated losses in [selling
options](families/selling-options.md). Both are the same event seen from different
positions — a crowded trade unwinding faster than it accumulated.

## Underneath: microstructure

Below the level of why price moves is the question of how it moves — the mechanics
that decide what your fill actually is.

Market makers quote both sides and profit from the spread, and they widen that spread
when uncertainty rises. That is why costs are worst exactly when your rule most wants
to trade, which you met in [markets](markets.md) and paid for in [opening
range](families/opening-range.md).

You do not need to model any of this. You need to know it exists, because it is where
your assumed fill and your actual fill part company — and Arvo measures that gap as
[divergence](paper-to-real.md) rather than leaving you to wonder.

## Why regimes change

The honest answer to the question level 3 left open. A [regime](market-types.md) is
not a weather pattern that arrives for no reason — it is what the four mechanisms
above look like from the outside:

- A **trend** is usually a repricing that takes time: information diffusing, a flow
  completing, a rate expectation shifting over months.
- A **range** is usually the absence of any of those: no new information, no
  constrained participant, nothing to reprice.
- A **regime change** is one of them starting or finishing.

Which is the useful takeaway of the whole ladder: your rule does not have an edge in
the abstract. It has an edge while a particular mechanism is operating, and the
mechanism has causes you can at least name.

## What Arvo can and cannot see here

**Cannot:** rate expectations, earnings surprises, fund flows, positioning data,
index rebalancing schedules. There is no data layer for any of it, and a rule needing
it has nowhere to read it from. Do not try to encode a macro view in a ruleset — you
will be encoding your guess about a series Arvo cannot verify.

**Can:** the *consequences*, and this is the part to use.

| What you want to know | What Arvo gives you |
|---|---|
| Did the conditions my rule needs go away? | the `regime` divergence reason: most entries, at least three, in a regime the finding never traded in |
| Did my rule stop firing? | the `frequency` reason: fewer than a quarter, or more than four times, as many entries as were due |
| Did my costs change? | the `execution` reason: slippage past twice what the cost model assumed |
| Was my result an average of two different markets? | the regime split on a finding |

A rule that fires a quarter as often as it did **is a regime change before it is a
loss**. That sentence is the whole practical value of this level: the mechanisms are
untestable here, and their footprints are measured, named, and raised as an alert
while there is still time to act.

## What people get wrong

**Trading the news.** By the time you read it, it is in the price. What is not in the
price is the part nobody has worked out yet, and you are unlikely to be first.

**Assuming a reason means a direction.** "Rates are rising, so stocks fall" is a
mechanism, not a trade. The market has the same information and has already acted on
it.

**Explaining a loss with a narrative.** After any drawdown you will find a plausible
macro story. It is indistinguishable from coincidence and it will make you change a
rule for the wrong reason. The divergence reasons exist so you have a measured answer
instead of a story.

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — how interest rates flow into valuations.
- *(to be chosen)* — index rebalancing and forced flows, with a worked example.
- *(to be chosen)* — market makers and why spreads widen.

## You have finished the ladder

Ten levels, and the thread running through all of them: **a claim is only worth as
much as the test behind it.**

Where to go now:

- [Run a study and read the verdict](../how-to/run-a-study.md) — the mechanics, if
  you have been reading rather than running.
- [Write a ruleset](../how-to/write-a-ruleset.md) — your own idea, honestly searched.
- [Rules as data](../features/rulesets.md#rules-as-data) — write the rule itself, in
  indicators and JSON Logic, once the six families stop being enough.
- [Why Arvo is different](../concepts/index.md) — the argument the whole ladder was
  built on. It reads differently now.
