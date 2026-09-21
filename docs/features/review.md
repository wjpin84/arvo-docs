# The review after the close

Fifteen minutes after the regular close on a trading day, the engine reads
every session's record for the day and writes one review under the
project's `reviews/` folder, as `YYYY-MM-DD.md` beside a `.json` with the
same content. It is never rewritten, so what you read is what was written;
ask for an earlier day and it is written then.

## What it says

For each session that had anything to say:

- bars, signals, what was submitted, what the gate **refused and why**, and
  how each position was **exited** and why;
- every **fill against its decision price**, the mean and worst slippage,
  the slippage the finding assumed, and signal-to-fill latency;
- every **round trip closed** on the day: opened, closed, size, entry, exit,
  profit, and the condition, regime and exit reason it carried, from the
  journal on every trade;
- **losses grouped** by condition, regime and exit reason, with their count
  and total. "Stop exits in ranging regimes" is how a rule fails; a list of
  red rows is not;
- the day's events: freezes, feed gaps, halts by the gate or by hand, each
  change of the session's **verdict**, each limit the **warning band** was
  entered on;
- what a **person** did: positions adopted from the venue, a halt by hand,
  a resume, a stop. Intervening after losses is the most common way a
  Supported rule underperforms its backtest, so these sit beside the P&L.

Realised profit is over the round trips that closed on the day, across
sessions, with what the finding expected per trade beside it.

## Where to read it

- The file, in the editor: `reviews/2026-09-21.md` under the project.
- The command line: `arvo-engine review` for today, `arvo-engine review
  2026-09-18` for a day, printed and written if it was not.
- The agent: `read_review`, so it can be asked *why did today go the way it
  did*.

A day is a UTC date. The US regular session sits inside one; a market whose
session crosses midnight UTC needs the per-market calendar that supporting
all markets brings.
