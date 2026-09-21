# Write a ruleset

1. Open the **Rulesets** tab. It lists the project's rulesets and the
   problem with any that cannot run.
2. Create `rulesets/<name>.json` in the project — from the tab, from the
   Files view, or by asking the Agent panel ("write a ruleset named
   fast_cross for sma_cross with fast 5 and 10, slow 20 and 30").
3. Name a rule Arvo ships, pin what you want pinned in `fixed`, and put the
   search in `axes`. Keep the search small and honest: the deflation counts
   every combination.
4. Save. The tab re-reads it; the Research view's rule picker offers it.

```json
{
  "name": "fast_cross",
  "label": "Faster crossover",
  "premise": "Say what you expect and why, in one sentence; it is recorded with every finding.",
  "interval": { "step": 1, "unit": "day" },
  "kind": {
    "kind": "grid",
    "rule": "sma_cross",
    "fixed": { "trade_size": 10.0 },
    "axes": { "fast": [5.0, 10.0], "slow": [20.0, 30.0] }
  }
}
```

!!! tip "A ruleset cannot change a rule's interval"
    `sma_cross` is a daily rule; a ruleset saying `1minute` is refused with
    *sma_cross is defined at 1day, and the document says 1minute*. The
    intraday rules are `opening_range`, `vwap_reversion` and the 0DTE pair.

Refused rulesets stay listed with their reason; nothing is silently
dropped. A ruleset's content hash is recorded on every finding made with it,
and editing it marks those findings stale.
