# Research

A **study** is the unit of research: one rule, one instrument, a search over
the rule's parameters, and a verdict. It runs in the engine, on bars from
the project's library, and is stored as a **finding** you can reopen,
replay, compare and export.

## What a study does

1. **Warms up and selects in sample.** The rule runs over every point of
   the search grid on the in-sample window.
2. **Tests out of sample.** The selected configuration runs on the
   held-out window — the only numbers reported as a result.
3. **Deflates.** The best in-sample result is judged against what the best
   of *that many* random trials would have shown. A grid of forty tries
   that beats a grid of forty coin flips by nothing is nothing.
4. **Evaluates.** Against the criteria — at least 30 trades, a positive
   excess return over the benchmark, a drawdown under 30% — and against
   a benchmark curve of buying and holding the instrument.
5. **Asks whether the costs made it.** A result that would be Supported is
   run again under the **conservative cost tier**: half again the
   commission, twice the slippage and never under five basis points, twice
   the fees, twice an option's spread. If it is not Supported there, the
   finding is refused, with the reason, and the advice says so first: the
   edge was the cost assumption's, not the rule's. A verdict is never
   upgraded by this, only refused.
6. **Records.** The finding, with its verdict, its advice, both curves,
   the trade ledger with what the rule saw on every trade, the dataset's
   content hash, the ruleset's hash and the engine's commit — so a finding
   knows when it has gone **stale** because its data or its ruleset changed.

## Kinds of study

| Kind | Question |
|---|---|
| **Study** | Does this rule, over this search, beat holding the instrument out of sample? |
| **Walk-forward** | Does the selection hold up when the window rolls forward, re-selecting each step? |
| **Panel** | The same rule over many instruments at once, one parameter set: does it hold across names? |
| **Book** | Many instruments, one account, one gate: what does the portfolio do, and which members were silent? |
| **Reported** | Evidence a script computed elsewhere, submitted with its curve and ledger; Arvo computes the verdict, never the submitter |

## Verdicts

<span class="arvo-verdict-supported">Supported</span> — beat the benchmark
by the margin, within the risk ceiling, on enough trades to be worth
reading. <span class="arvo-verdict-notsupported">NotSupported</span> — ran
cleanly and did not clear the bar. <span class="arvo-verdict-inconclusive">Inconclusive</span>
— the run cannot answer either way: too few trades, too short a window, a
Sharpe whose standard error swallows it. The expected outcome, and not a
failure. [Verdicts and advice →](../reference/verdicts.md)

Every finding carries **advice**: named findings with a severity
(`blocking`, `warning`, `info`), the evidence, and what to do about it —
widen the window, loosen the entry, treat the tail as provisional. Read the
advice before the return.

## Where findings live

`evidence/` in the project folder, one JSON per finding and an index. The
**History** list in the Research view shows them with their staleness; the
**Runs** tab shows the ones scripts and agents produced, grouped by author,
with the size of the search each was held to.

## Provenance

A finding records the ruleset's content hash, the engine's commit and the
dataset's version. When the ruleset changes, every finding made with the old
one is marked stale and says so; the same when the data is refetched and
differs.
