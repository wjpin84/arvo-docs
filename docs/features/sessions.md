# Sessions

A session is a finding's rule, running against a venue: one finding, one
executor, one thread in the engine. The rule is rebuilt as a shadow engine
warmed on the library up to today; from then on each completed bar is
pushed through it, entries go to the risk gate and exits go straight to the
venue.

Start one from the **Sessions** tab (choose a finding, choose an executor)
or the command line. The executors are `alpaca-paper`, `alpaca-live` and
`robinhood-<last four>`. Paper is drawn apart from real money everywhere it
appears.

## What a session does every poll

- fetches new bars (or takes them from the stream) and pushes each one the
  moment it is over — a daily bar's signal fires overnight and fills at the
  open, the fill the backtest assumed;
- settles: collects fills and books them on the gate;
- **audits** its book against the venue's, and freezes on a disagreement.

## States

| State | Meaning |
|---|---|
| `starting` | opening the finding, warming the shadow |
| `running` | polling or streaming, taking entries |
| `frozen` | entries wait for a person; exits still go. Either the book disagrees with the venue (reconcile, then resume) or the feed has gone dark (lifts itself when the feed returns) |
| `halted` | the gate stopped the account: the drawdown halt, or the kill switch. Stays until released |
| `stopped` | asked to stop; positions left as they are |
| `failed` | the thread ended with an error; the reason is on the row |

## Reconciliation as an incident

A position the venue reports and the gate does not — a fill this process
never heard, a hand-placed order, a broker-side liquidation — means the rule
can no longer size against a book it trusts. The session **freezes** and
records the discrepancy. **Reconcile** makes the gate's book the venue's
(the venue holds the money); **Resume** takes entries again, and is refused
until a reconcile has happened. Both are events in the record. Nothing
resumes on its own.

## The kill switch

**Halt** arms the gate and then flattens everything the session holds, in
that order, so a proposer racing in is refused rather than opening into the
exit. What the venue would not exit is named in the record and on the
status; the halt stands either way. From a session's row, from the
Operations view, from the tray (**Halt trading**, every session) or from
the command line. [How to halt →](../how-to/halt.md)

## The record

Everything a session does is one JSON line per event in
`sessions/<finding>@<executor>.jsonl`, and every event names what caused
it. [The audit chain →](journal.md) · [Record reference →](../reference/session-record.md)
