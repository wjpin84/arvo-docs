# The API and the packages

The engine serves a local gRPC API, defined in
[arvo-engine-api](https://github.com/wjpin84/arvo-engine-api): protos in
`protos/`, and bindings generated from them and published under one version.

| Package | What |
|---|---|
| `arvo-api` (crates.io) | the messages, with serde; no transport, builds for WebAssembly |
| `arvo-client` (crates.io) | the service stubs, engine discovery (`engine.json`, `control.json`), the token helper |
| `arvo-client` (PyPI), module `arvo` | a research client and the generated stubs for every service |

## Two tiers

| Token | File | Reaches |
|---|---|---|
| research | `engine.json` | the `Research` service: strategies, instruments, findings, studies, walk-forwards, bars, reported evidence. What an agent or a script gets |
| control | `control.json` | everything else: `ResearchFiles`, `Market` (fetching, sources, the watchlist, quotes), `Accounts`, `Portfolio`, `Platform` (plugins, jobs, events), `Sessions`, `Scripts`. What a person's front end gets |

A refusal is a gRPC status, never a message with an error field. Field
numbers are the contract; a removed field's number is reserved. Versioning
follows semver from an existing client's point of view — see
[VERSIONING.md](https://github.com/wjpin84/arvo-engine-api/blob/main/VERSIONING.md)
and the changelog.
