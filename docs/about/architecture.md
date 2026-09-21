# Architecture

Four repositories and one for decisions:

| | |
|---|---|
| [arvo-engine](https://github.com/wjpin84/arvo-engine) | the daemon, the MCP server and every crate that decides anything |
| [arvo-desktop](https://github.com/wjpin84/arvo-desktop) | the window and its editor; links no engine crate, talks to the engine over the API |
| [arvo-engine-api](https://github.com/wjpin84/arvo-engine-api) | the API: protos and the Rust and Python bindings generated from them |
| [arvo-extension-api](https://github.com/wjpin84/arvo-extension-api) | the provider contract a plugin implements |
| [arvo-adrs](https://github.com/wjpin84/arvo-adrs) | decisions, research notes and plans, across all of them |

## The research loop

```
Hypothesis → Experiment → Simulation → Evaluation → Evidence → Research memory → Agent
```

NautilusTrader is the financial runtime underneath, linked in-process:
orders, positions, accounts, execution, backtesting. Arvo owns what is
unique to Arvo — out-of-sample selection, deflation, `Inconclusive` as a
verdict, reconciliation, replayable findings, advice — and exactly one crate
is permitted to name a Nautilus type. The differentiator is evaluation, not
execution.

## Standing rules

- **The window knows nothing about how a study is run.** It renders what
  the engine says. The build enforces it: the desktop depends on the two
  contract crates and nothing else of Arvo's.
- **The engine keeps running when the window closes.** Schedules run and
  sessions trade with nothing open; the window, the Python package, the MCP
  server and the command line are all clients of one engine per user.
- **The AI never receives broker credentials.** An agent reaches Arvo
  through a tool list that cannot trade.
- **One risk policy, backtest and live.**
- **Bars are fetched to files and pinned by content hash.** A study never
  reads a vendor live; a tick never becomes a bar.
- **A provider is a process.** Arvo starts it, hands it a token per spawn
  and a grant per call, and asks nothing of a service it did not claim.
  No third-party code runs in the window.
- **Anything that makes a wrong answer look right outranks anything that
  adds capability.**

## Inside the engine

`crates/` is the platform (data, research, risk, execution, the plugin
host, the one Nautilus crate), `integrations/` talk to vendors (Alpaca,
Robinhood, Yahoo), `app/` is the daemon and the MCP server. Traits live
beside their implementer or their primary consumer, never in a shared
types crate, and a provider trait is earned by removing a dependency or a
panic — never by anticipating an implementation nobody asked for.
