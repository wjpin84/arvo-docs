# The risk model

`risk.json` in the project, or Arvo's defaults when there is none. One
model, in the backtest and in every session. A field not set is *not set*,
and the Risk tab says so rather than showing a default as if you chose it.

| Field | What |
|---|---|
| `stop_atr_multiple` | Stop distance as a multiple of ATR. Not set runs with no stop |
| `atr_period` | Bars the ATR is averaged over |
| `risk_per_trade` | Fraction of starting capital a stop-out may cost. Needs a stop |
| `max_position_fraction` | Fraction of starting capital one position may occupy. Above 1 is leverage |
| `max_drawdown` | Fall from the account's peak that stops the rule for the run. Permanent |
| `max_concurrent_positions` | The most positions held at once across a book |
| `max_daily_loss` | Realised loss in one day, as a fraction of starting capital, that refuses new entries |
| `correlation_cap` | Above this pairwise correlation, names are one bet; how many such bets may be held. Refuses when unknown |
| `sector_cap` | The most positions one sector may hold, with each name's sector. Refuses an unlabelled name |
| `day_trading` | `unconstrained`, or `pattern_day_trader`: three round trips per five days under $25k |

Costs — slippage, commission, a per-fill fee, an option spread — are on the
experiment, recorded with every finding, and the paper session's measured
slippage is reported against what the backtest assumed.
