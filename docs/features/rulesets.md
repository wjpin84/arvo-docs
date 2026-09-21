# Rules and rulesets

A **rule** is a strategy Arvo implements — the code that decides entries
and exits. A **ruleset** is a document that runs a rule over a search of
parameters you choose. You write rulesets; you do not write rules (an
extension cannot contribute one either). The word *strategy* appears only in
code, protocols and findings; the window says rule and ruleset.

## The rules Arvo ships

| Rule | Interval | Premise |
|---|---|---|
| `sma_cross` — Moving-average crossover | 1 day | The control. Not a good idea, a rule nobody disputes |
| `volatility_breakout` | 1 day | A thrust measured in ATRs, so it means the same on any price |
| `momentum_breakout` | 1 day | Buy a new channel high, leave on a trailing channel low |
| `cross_sectional_momentum` | 1 day | Hold the few that rose most, and nothing else — ranks the set rather than judging each name on its own |
| `opening_range` | 5 minutes | The session's first bars set a range; a close above it enters, with a target in range widths |
| `vwap_reversion` | 5 minutes | Buy a close more than a deviation below VWAP; leave at VWAP |
| `put_spread` | 1 day | Sell a put near a delta, buy one a width below; sized by the cash the worst case needs |
| `zero_dte_breakout`, `zero_dte_put_spread` | 5 minutes | Same-day options on the instrument's chain |

A rule is defined at an interval; a ruleset cannot move it. A one-minute
`sma_cross` is refused with the reason.

## A ruleset

A JSON file in the project's `rulesets/` folder:

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

`axes` is the search: every combination is one trial, and the deflation
counts them. `fixed` pins a parameter. A ruleset that searches nothing is
refused — one axis with two values is the smallest search.

The **Rulesets** tab lists the project's, with each one's problem when it
has one (a rule Arvo does not have, an interval the rule is not defined
at, a name that is one of Arvo's own). Edit the file in the editor and the
list re-reads it; the Agent panel and the MCP server can write one for
you. [How to write a ruleset →](../how-to/write-a-ruleset.md)
