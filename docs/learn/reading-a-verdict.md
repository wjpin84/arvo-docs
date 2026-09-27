---
title: Reading a verdict
description: Level 8 — what Supported, NotSupported and Inconclusive mean, why the third is the common one, and the order to read a finding in.
---

# Reading a verdict

*Level 8. Assumes [why a backtest lies](backtest-lies.md).*

## The idea

Arvo's answer to a study is not a number. It is a **verdict**, a paragraph of
advice, and then the evidence — in that order, deliberately, because a number
always looks like an answer.

| Verdict | Means |
|---|---|
| **Supported** | beat the benchmark by the required margin, within the risk ceiling, on enough trades to be worth reading |
| **NotSupported** | ran cleanly and did not clear the bar |
| **Inconclusive** | the run cannot answer the question either way |

The criteria are fixed and published rather than tuned per run: **at least 30
trades**, an excess return over the benchmark **above 0**, and a drawdown **under
30%**. The benchmark is holding the instrument, which is the bar every study is
measured against — making money is not the test, beating the alternative of doing
nothing is.

## Why Inconclusive is the important one

`Inconclusive` is the most common verdict, and it is not a failure, a limitation or
a soft `NotSupported`. It is a different kind of statement:

- **NotSupported** says *the rule did not clear the bar on this evidence.*
- **Inconclusive** says *there is not enough evidence here to say either way.*

A rule with 18 round trips has not been shown to work *or* to fail. Reporting a
return on it would be reporting noise with a decimal point. Most systems in this
space would give you the ratio anyway; Arvo says it does not know, which is the
honest answer and the one most people are not used to hearing.

**The correct response to Inconclusive is a better test, not a different rule.**
Widen the window, or pool evidence with a [panel](../concepts/trading.md#panel) —
the same configuration across many instruments, which makes the trade-count bar
reachable honestly instead of by lowering it.

!!! tip "If you are getting Inconclusive constantly, nothing is wrong"
    A daily rule on one instrument over two years produces a few dozen trades at
    most. That is the arithmetic of daily bars, not a fault in your rule or in
    Arvo. The intraday families — [mean reversion](families/mean-reversion.md),
    [opening range](families/opening-range.md) — generate enough round trips to get
    real verdicts, which makes them the better place to learn what a verdict feels
    like.

## Advice, and why it comes first

Every finding carries **advice**: named items, each with a finding in words, its
evidence, an action, and a severity.

| Severity | Means |
|---|---|
| `blocking` | the numbers below are **not a result yet** |
| `warning` | read them with this in mind |
| `info` | worth knowing |

Real examples Arvo produces:

- *Too few round trips to tell skill from luck* — 18 trades against a 30 minimum —
  widen the window or loosen the entry; do not read the return until it does.
- *The run ended holding a position* — treat the tail of the curve as provisional.
- *The point estimate says nothing about its own error* — lengthen the window; do
  not compare this Sharpe against another until it is.
- *The selection did not survive deflation* — the best of this many tries is what
  luck produces.
- *The dividend gap is most of the margin* — the split-adjusted series cannot see
  distributions the benchmark paid.
- *Supported only under the stated costs* — do not promote this; widen the edge or
  trade less often until the rule survives fills that cost twice what was assumed.

Each of those is a lesson from an earlier level, fired automatically against your
own result. That is the whole design: the advice is the curriculum, applied.

## The order to read a finding in

Follow this every time. The order exists because reading it in any other order
makes you credulous.

1. **The verdict.** Then `read_this_first`, one sentence on what the verdict means
   for this run.
2. **The advice.** If anything is `blocking`, stop — the numbers below are not a
   result. Fix the test and re-run.
3. **The search.** How many configurations were tried, and whether the selection
   survived deflation against that many. A grid that only wins in sample is the
   most common way a wrong answer looks right.
4. **Out-of-sample numbers only.** The in-sample numbers chose the configuration.
   They are not evidence of anything and are shown so you can see the gap.
5. **Against the benchmark**, with the dividend gap beside it. If the gap is most of
   the margin, the margin is dividends the price series cannot see.
6. **The cost tiers.** Supported at stated costs but not at conservative means the
   edge belonged to the cost assumption.
7. **The trades.** Every round trip with the rule that fired, its value, the regime
   at entry, the exit reason, fees, and what filled against what was asked.

## Cost tiers

Three, and only two of them can affect a verdict:

| Tier | What it is | Can it change a verdict? |
|---|---|---|
| **Realistic** | the experiment's stated model: what a fill is expected to cost at that venue | it *is* the verdict |
| **Conservative** | half again the commission, twice the slippage and never under five basis points, twice the fees, twice an option's spread | **it can only refuse, never upgrade** |
| **Optimistic** | half the commission and nothing else | never — nothing is judged under it |

Optimistic exists purely to answer "how much of this result is costs?" The
leaderboard ranks by what survived conservative costs first, so a rule that looks
better raw and worse at doubled costs ranks *below* its neighbour. Raw return is a
column you read and never the order — a ranking by raw return is the overfitting
machine every screener ships, and Arvo does not ship one.

## A session's verdict is a different question

Once a rule is trading, the same three words are reused for a new question: do the
live trades still look like the finding's out-of-sample trades?

| Verdict | Means |
|---|---|
| **Holding** | the live trades sit inside what the out-of-sample distribution would produce |
| **Diverging** | expectancy, drawdown or firing rate has left that range; the reason is named |
| **Inconclusive** | too few live trades to say, which is where every session starts |

The named reasons, with the thresholds, so a verdict can be read rather than
trusted:

| Reason | Fires when |
|---|---|
| `expectancy` | after ten closed live trades, the live mean is more than two standard errors below the finding's |
| `drawdown` | at one and a half times the finding's out-of-sample maximum, at any trade count |
| `frequency` | four entries were due and fewer than a quarter, or more than four times, as many came |
| `regime` | most entries, and at least three, landed in a regime the finding never traded in |
| `execution` | after five fills, slippage is past twice what the cost model assumed |

**A verdict is not a risk limit.** Diverging changes nothing at the gate — it is
recorded, raised as an alert, and is the *case* for demoting a session, decided by a
person. The drawdown halt and the daily loss limit protect the account; the verdict
judges the rule. Keeping those two jobs separate is deliberate.

## Staleness

A finding carries `stale_reason`: `data`, `ruleset`, or both. The finding stays — it
simply no longer describes the present. Re-run it; the old one remains as history.

A finding records the ruleset's content hash, the engine's commit and the dataset's
version, which is what makes staleness detectable at all rather than something you
have to remember.

## What people get wrong

**Reading the return first.** The whole page is arranged to stop you doing this, and
it is still the most common mistake.

**Treating Supported as permission.** It means one finding cleared one bar on one
window. Real money needs the [promotion gate](paper-to-real.md), and that is level 9.

**Comparing two findings' Sharpe ratios casually.** If the advice says the point
estimate says nothing about its own error, then the comparison is noise against
noise.

**Assuming a verdict is about the account.** It is about the rule. The account has
its own protections and they are unrelated.

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — statistical significance and sample size, in plain terms.
- *(to be chosen)* — why "not enough evidence" differs from "no effect".

## Next

[Paper to real money](paper-to-real.md) — what paper trading measures, what it
cannot, and the gate in front of the money.
