# Start a paper session

Paper trading in Arvo is a broker's paper account — Alpaca's — with a real
API, real fills and latency and simulated money. It is not a rehearsal; it
is the measurement a backtest cannot make: what a fill actually costs
against the price the decision was made at.

1. Connect Alpaca with **paper** keys (Accounts).
2. Have a finding on an instrument the executor trades — for Alpaca, an
   Alpaca-venue instrument (`AAPL.AIEX`).
3. **Sessions** tab → choose the finding → executor `alpaca-paper` → Start.
   Or: `arvo-engine session start <finding> alpaca-paper`.
4. Watch the row: state, signals, submitted, refused, fills, last bar. The
   record below it shows every event as it happens.

What to expect:

- A daily rule hears "nothing new" most polls; its signal fires overnight
  and fills at the open.
- A 5-minute rule on Alpaca streams: each bar arrives a moment after it
  closes and the rule acts on it at once.
- The first bars after a start are the catch-up from the library to now.
- If the paper account already holds positions when the session starts,
  it adopts them and comes up **halted** until you release it — a session
  should not start believing it is flat when it is not.

Stopping leaves positions as they are. Halting flattens them.
