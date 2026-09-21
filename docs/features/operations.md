# Operations

One screen answering *is anything wrong right now*. View → Operations.

At the top, the verdict: **All clear**, or *N things need attention* and
the list, in the order someone should care:

- a session frozen (with Reconcile and Resume right there), halted, failed,
  or running with an error;
- the price stream interrupted;
- a session near one of the gate's limits, with the figure;
- a session Diverging from its finding, with the reason;
- the risk model refused;
- findings gone stale.

Account and plugin problems come from the same list the status-bar badge
counts, so the screen cannot disagree with the badge. It is current state,
not a tally of events: a problem fixed stops counting, and one that was
there when the window opened counts without an event.

Under it: **Sessions** (state, rule, executor, last bar, stop and halt),
**Venues** (each vendor with its sub-accounts, each plugin's reachability,
the price stream's last word) and **Gate** (the risk model in force, sessions
halted, sessions frozen with an unreconciled book, stale findings). It
refreshes every five seconds while shown.
