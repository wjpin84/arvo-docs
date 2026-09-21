# Command line

The engine's binary is the command line; there is no separate `arvo`
binary by decision. Every verb below talks to the running engine through
`engine.json` and the control token, except `explain`, which reads a record
on disk and needs no engine.

```
arvo-engine [<data-dir>]                       serve; the app data directory when none is given
arvo-engine help
arvo-engine session list
arvo-engine session start <finding> <executor>
arvo-engine session stop|reconcile|resume <id>
arvo-engine session halt <id>|--all [reason...] the kill switch: arm the gate, flatten, stay halted
arvo-engine session explain <id> <time>        the chain behind every position held then
arvo-engine [<data-dir>] review [YYYY-MM-DD]   the review after the close, written and printed
```

A session's `<id>` is `<finding>@<executor>`, as `session list` prints it,
with its state, its counts, and its verdict against its finding (with the
reason, when diverging).
`<executor>` is `alpaca-paper`, `alpaca-live` or `robinhood-<last four>`.

The MCP server:

```
arvo-mcp-server [<app-data-dir>] [--agent NAME]
```

Stdout is the protocol; diagnostics go to stderr.
