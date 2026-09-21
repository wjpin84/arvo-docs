# The session record

`sessions/<finding>@<executor>.jsonl` in the project: one JSON line per
event — `{"at": ..., "event": ..., "detail": ...}` — appended as things
happen, surviving the session and the engine. The desktop's Sessions tab
reads it from disk; `session explain` walks it.

| Event | Detail | Caused by |
|---|---|---|
| `started` | the experiment, how far the library warmed the shadow | |
| `expectation` | what the finding's out-of-sample trades lead the session to expect: `trades`, `expectancy`, `deviation`, `max_drawdown`, `entries_per_bar`, `regimes`, `slippage_bps`; or `none` and why | the finding |
| `instrument` | lot, tick, hours, multiplier the gate sizes against | the source |
| `feed` | `{streaming, source}` | |
| `feed_up`, `feed_down`, `feed_ended` | the stream's state, with the reason | |
| `reconciled` | at start: `adopted`, `cancelled`, `stranded`; mid-session: `corrected` and the whole `positions` book | a start, or a Reconcile |
| `bar` | `at`, `close`, how many signals it produced | |
| `signal` | `id` (`<bar>#<n>`), `bar`, side, quantity, price, `exit`, and the journal: `rule`, `signal`, `regime` | the bar |
| `submitted` | `{signal, order}` | the signal, accepted by the gate |
| `refused` | `{signal, why}` — a gate rejection, or *frozen* | the signal |
| `exit` | `{signal, why, order}` — sent without asking the gate | the signal |
| `ignored` | a sell to open, which no hosted rule does | |
| `filled` | the execution: order, side, quantity, decision and fill prices and instants, proposer, and the `position` it left | the order |
| `frozen` | `{discrepancies}` or `{stale}` | the audit, or the feed |
| `resumed` | why, when the feed came back | a Resume, or the feed |
| `resume_refused`, `reconcile_failed`, `audit_failed`, `fetch_failed`, `settle_failed` | the reason | |
| `halted` | why; for the kill switch also what `flattened` and what `failed` | the gate, or a Halt |
| `warning` | `entered` and `cleared`, the limits by name, and `near`, everything the session is near now in the gate's words: `drawdown 8.1% of a 10.0% limit`. Written when the set changes, not when a figure moves | a bar, or a settle |
| `verdict` | `holding`, `diverging` or `inconclusive`, and for diverging the `reason` (`expectancy`, `drawdown`, `frequency`, `regime`, `execution`) with what was seen and what was expected. Written when it changes | a settle |
| `stopped` | | a Stop |

Times are UTC. Every failure also raises an alert on the engine's event
channel, which the window's alerts list and the Operations view show.
