# Examples

## A ruleset

```json
{
  "name": "fast_cross",
  "label": "Faster crossover",
  "premise": "MSFT swings on a 20-30 day base, faster than the shipped 50-200 grid.",
  "interval": { "step": 1, "unit": "day" },
  "kind": {
    "kind": "grid",
    "rule": "sma_cross",
    "fixed": { "trade_size": 10.0 },
    "axes": { "fast": [5.0, 10.0], "slow": [20.0, 30.0] }
  }
}
```

## A research script

```python
"""Runs the crossover over the watchlist and prints what is Supported."""
import arvo

engine = arvo.connect()
for instrument in ("AAPL.RH", "MSFT.RH", "JNJ.RH"):
    found = engine.run_study(instrument, "fast_cross", author="script:scan")
    blocking = [a for a in found.advice if a.severity == "blocking"]
    print(f"{instrument:10} {found.verdict:14} {'; '.join(a.finding for a in blocking)}")
```

Run it from the editor or schedule it every morning; its findings land in
the Runs tab under `scan`, deflated against every configuration `scan` has
ever tried.

## An agent turn

> **You:** Using the arvo tools only: write a ruleset named panel_cross for
> sma_cross with trade_size 10 and axes fast 5 and 10, slow 30 and 50; then
> run a study on MSFT.RH and tell me the finding id and verdict.
>
> **Agent:** *write_ruleset* → `rulesets/panel_cross.json`, 4 trials ·
> *run_study* → finding `20260920T221202796-MSFT.RH` — **Inconclusive**:
> 18 round trips against a 30-trade minimum, so the OOS numbers are not
> readable as a result. Next, one change: loosen the entry so the rule
> fires often enough to clear 30 trades.

## A session record, walked

```
$ arvo-engine session explain 20260921T134031540-AAPL.AIEX@alpaca-paper 2026-09-21T14:00:00Z
AAPL.AIEX: 297 at 2026-09-21T14:00:00+00:00
  fill 1 of 1
    bar       2026-09-21T13:45:00 close 335.73
    signal    2026-09-21 13:45:00#0  buy 297 @ 335.73  rule: close above the opening range = 0.101  regime: ranging
    gate      accepted; order fe043155-… acknowledged 2026-09-21T13:55:11
    fill      buy 297 @ 335.89 at 2026-09-21T13:55:12  (decided @ 335.73 → position 297)
```

## Reporting evidence from your own engine

```python
finding = engine.report(
    hypothesis="momentum persists on weekly bars",
    experiment=..., curve=[(when, equity), ...], benchmark_curve=[...],
    ledger=[{"opened": ..., "closed": ..., "direction": "long", "quantity": 10,
             "entry": 100.0, "exit": 104.0, "pnl": 40.0, "commission": 1.0,
             "exit_reason": "signal", "rule": "weekly high", "signal": 1.2, "regime": "trending up"}],
    trials=12, author="script:their-engine",
)
```

Arvo computes the verdict from the curve and the ledger with the same
criteria a study uses; there is no argument for a verdict, on purpose.
