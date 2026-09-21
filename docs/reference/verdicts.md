# Verdicts and advice

## Verdicts

| Verdict | Meaning |
|---|---|
| <span class="arvo-verdict-supported">Supported</span> | beat the benchmark by the required margin, within the risk ceiling, on enough trades to be worth reading |
| <span class="arvo-verdict-notsupported">NotSupported</span> | ran cleanly and did not clear the bar |
| <span class="arvo-verdict-inconclusive">Inconclusive</span> | the run cannot answer the question either way — the expected outcome, and not a failure |

The criteria: at least **30** trades, an excess return over the benchmark
above **0**, a drawdown under **30%**. A finding also carries
`read_this_first`, one sentence saying what the verdict means for that
run.

## Advice

Each item: a **finding** in words, its **evidence**, an **action**, and a
severity — `blocking` (the numbers are not a result yet), `warning` (read
them with this in mind), `info`. Examples the platform produces:

- *Too few round trips to tell skill from luck* — widen the window or loosen the entry; do not read the return until it does.
- *The run ended holding a position* — treat the tail of the curve as provisional.
- *The point estimate says nothing about its own error* — lengthen the window; do not compare this Sharpe against another until it is.
- *The selection did not survive deflation* — the best of this many tries is what luck produces.
- *The dividend gap is most of the margin* — the split-adjusted series cannot see distributions the benchmark paid.

## A session's verdict

The same vocabulary, applied while a rule trades, by comparing the
session's ledger with the finding's out-of-sample expectation:

| Verdict | Reason named when it applies |
|---|---|
| **Holding** | |
| **Diverging** | `expectancy` (live expectancy per trade outside the out-of-sample interval, after enough trades), `drawdown` (beyond the out-of-sample max by a factor, before the account's halt), `frequency` (firing far less or more often than in the window), `regime` (entries in a regime the finding was not Supported in), `execution` (slippage above the cost model assumed) |
| **Inconclusive** | too few live trades yet |

A verdict never touches the gate. It feeds the warning tier and the
promotion gate: a live executor refuses a finding whose paper session was
Diverging.

## Staleness

`stale_reason` on a finding: `data`, `ruleset`, or both. The finding stays;
it no longer describes the present.
