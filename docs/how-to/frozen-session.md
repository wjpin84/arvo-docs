# When a session freezes

A frozen session has stopped taking entries and is waiting for you. Exits
still go out; bars still flow. The row, the Sessions record and the
Operations view all say why.

## The book disagrees with the venue

The record shows `frozen` with the discrepancies — *BTCUSD.RH: gate 0 venue
0.001* — something traded that the rule did not decide.

1. Look. The Portfolio and the venue's own site say what happened.
2. **Reconcile** (the row, the Operations view, or `arvo-engine session
   reconcile <id>`): the gate's book is made the venue's; the record shows
   `reconciled` with the corrected positions and the book after.
3. **Resume** (`session resume <id>`): entries again. Resume before
   reconcile is refused — resuming against a book the venue disagrees with
   is the state the freeze exists to prevent.

A position adopted this way has no bar or signal behind it, and
`session explain` says so.

## The feed went dark

A streaming session whose socket stops answering freezes with *stale feed*
and thaws by itself when the feed is back; you can also resume it at once,
since nothing about the book is in doubt. The poll underneath keeps catching
bars up either way.
