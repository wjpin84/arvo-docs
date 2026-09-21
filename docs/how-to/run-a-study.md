# Run a study and read the verdict

## Run it

Research view → pick the rule (or your ruleset) and the source → pick an
instrument with bars (fetch first if the library has none) → **Run**. A
study takes seconds on daily data; the Jobs panel shows it running.

## Read it, in this order

1. **The verdict.** `Supported`, `NotSupported` or `Inconclusive`.
   Inconclusive is the common one and the honest one.
2. **The advice.** Each item names a finding, its evidence and an action:
   *Too few round trips to tell skill from luck — 18 trades against a 30
   minimum — widen the window or loosen the entry; do not read the return
   until it does.* A `blocking` item means the numbers below are not a
   result yet.
3. **The search.** How many configurations were tried, and whether the
   selection survived deflation against that many. A grid that only wins in
   sample is the most common way a wrong answer looks right.
4. **Out of sample only.** The in-sample numbers chose the configuration;
   they are not evidence of anything.
5. **Against the benchmark.** Holding the instrument is the bar. The
   dividend gap beside it says how much of the margin is dividends the
   split-adjusted series cannot see.
6. **The trades.** The Trades tab: every round trip with the rule that
   fired, its value, the regime, the exit reason, fees, and what filled
   against what was asked.

## Compare and export

Select two findings in History to compare them side by side. A finding
exports as a report (Markdown with the charts), the trades as CSV, and the
experiment as a file another engine could re-run.

## When it goes stale

A finding whose data was refetched and differs, or whose ruleset changed,
is marked stale with the reason. Re-run it; the old one stays as history.
