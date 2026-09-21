# Explain a position

```
arvo-engine session explain <finding>@<executor> <time>
```

`<time>` is RFC 3339 (`2026-09-21T14:00:00Z`) or `YYYY-MM-DD HH:MM:SS` read
as UTC. It reads the session's record on disk — the engine need not be
running, the session need not exist any more — and prints the chain behind
every position held at that time: the bar it was decided on, the signal with
the rule's condition, value and regime, the gate's decision and the order,
the fill and the position it left. A flat session says so and names the
last change to the book; an adopted position says it was adopted.
