# Install

Arvo is two programs that ship together: the **window** (a desktop
application) and the **engine** (a daemon the window starts and talks to).
An installed Arvo starts its own engine; you never run it by hand unless
you want to.

!!! note "Where it runs"
    Windows (x86_64), macOS (Apple silicon) and Linux (x86_64). The engine
    is released for all three; the window is developed and tested on Windows
    first.

## From a release

Engine releases are on the [arvo-engine releases page](https://github.com/wjpin84/arvo-engine/releases):
one archive per platform, `arvo-engine-<version>-<triple>.zip`, holding
`arvo-engine` and `arvo-mcp-server`, with a `.sha256` beside it.

A window installer bundles the engine it was built against, so installing
the window is enough. Until an installer is published for your platform,
build the window from source (below) against a released engine.

## From source

You need a Rust toolchain (stable), Node.js (for the editor's web build),
[Trunk](https://trunkrs.dev) and the Tauri CLI, and Python 3.10+ with `uv`
or `pip` for the tooling.

=== "Windows (PowerShell)"

    ```powershell
    git clone --recurse-submodules https://github.com/wjpin84/arvo-desktop
    cd arvo-desktop
    python tools/fetch_engine.py          # downloads the engine release pinned in app/arvo-runtime/engine-version
    $env:ARVO_ENGINE = "<the path it prints>"
    cargo tauri dev                       # or: cargo tauri build, for an installer
    ```

=== "macOS / Linux"

    ```sh
    git clone --recurse-submodules https://github.com/wjpin84/arvo-desktop
    cd arvo-desktop
    python tools/fetch_engine.py
    export ARVO_ENGINE="<the path it prints>"
    cargo tauri dev
    ```

`fetch_engine.py` needs the GitHub CLI signed in while the engine repository
is private. To build the engine yourself instead:

```sh
git clone --recurse-submodules https://github.com/wjpin84/arvo-engine
cd arvo-engine
cargo build --release -p arvo-engine -p arvo-mcp-server
```

and point `ARVO_ENGINE` at `target/release/arvo-engine`.

## What gets written where

| | |
|---|---|
| App data directory | `%APPDATA%/com.arvo.desktop` on Windows, `~/Library/Application Support/com.arvo.desktop` on macOS, `~/.local/share/com.arvo.desktop` on Linux. Holds `engine.json` and `control.json` (how a client finds the engine), `settings.json`, `keybindings.json`, the extensions directory and the window's own state |
| Project folder | The folder you open. Holds `data/` (the bar library), `evidence/` (findings), `rulesets/`, `sessions/` (records), `portfolios/`, `snapshots/` and your scripts |
| Keychain | Credentials, in the operating system's keychain — never in a file |

## Licence

Apache-2.0. The engine links NautilusTrader, which is LGPL-3.0-only; see
the `NOTICE` in each repository.
