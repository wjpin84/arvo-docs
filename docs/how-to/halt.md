# Halt trading

The kill switch arms the gate first and then flattens everything the session
holds — in that order, so anything racing in behind the button is refused
rather than opening into the exit. What the venue refuses to exit is named
in the record and on the session's status; the halt stands either way.

| Where | What it halts |
|---|---|
| A session's **halt** button (Sessions, Operations) | that session |
| Tray → **Halt trading** | every running or frozen session, with the window closed |
| `arvo-engine session halt <id> [reason…]` | that session |
| `arvo-engine session halt --all [reason…]` | every running or frozen session |

A halted session stays up to book the exits' fills and then sits halted;
stop it when you are done. A drawdown halt is not the kill switch and lifts
for nobody.
