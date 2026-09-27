# Install the engine

The engine is a single binary with no runtime dependencies. Installing it does
not install the window, and it does not need one.

Two executables ship together in every archive:

| | |
|---|---|
| `arvo-engine` | the daemon, and the command line — [there is no separate `arvo` binary](cli.md) |
| `arvo-mcp-server` | the research surface for agents, over stdio |

## System requirements

| Platform | Support | Notes |
|---|---|---|
| Windows 11, Windows 10 22H2 (x86_64) | Supported | Developed and tested here first |
| Linux (x86_64), glibc 2.31+ | Supported | Ubuntu 20.04 and newer; CI runs `ubuntu-latest` |
| macOS 12+ (Apple silicon) | Supported | |
| macOS (Intel) | Partially supported | Built, not routinely tested |
| Linux (aarch64), Windows (arm64) | Not released | Build from source |

Runtime needs are modest: about 200 MB of memory idle, more while a study runs.
A study is CPU-bound and uses every core you have — a parameter grid crossed
with a universe is thousands of independent backtests, and they are run in
parallel. Disk is dominated by the bar library, not the binary: ten years of
daily bars for a hundred instruments is roughly 40 MB; a day of five-minute
option chains for one underlying is larger than that.

## From a release

Archives are on the [releases page](https://github.com/wjpin84/arvo-engine/releases),
one per platform, named `arvo-engine-<version>-<triple>.zip`, each with a
`.sha256` beside it.

=== "Windows (PowerShell)"

    ```powershell
    $ver = "0.7.0"
    gh release download "v$ver" --repo wjpin84/arvo-engine --pattern "*x86_64-pc-windows-msvc.zip*"
    (Get-FileHash "arvo-engine-$ver-x86_64-pc-windows-msvc.zip").Hash -eq `
      (Get-Content "arvo-engine-$ver-x86_64-pc-windows-msvc.zip.sha256").Split()[0].ToUpper()
    Expand-Archive "arvo-engine-$ver-x86_64-pc-windows-msvc.zip" -DestinationPath "$env:LOCALAPPDATA\Arvo"
    ```

=== "macOS"

    ```sh
    ver=0.7.0
    gh release download "v$ver" --repo wjpin84/arvo-engine --pattern "*aarch64-apple-darwin.zip*"
    shasum -a 256 -c "arvo-engine-$ver-aarch64-apple-darwin.zip.sha256"
    unzip "arvo-engine-$ver-aarch64-apple-darwin.zip" -d ~/.local/arvo
    chmod +x ~/.local/arvo/arvo-engine ~/.local/arvo/arvo-mcp-server
    ```

=== "Linux"

    ```sh
    ver=0.7.0
    gh release download "v$ver" --repo wjpin84/arvo-engine --pattern "*x86_64-unknown-linux-gnu.zip*"
    sha256sum -c "arvo-engine-$ver-x86_64-unknown-linux-gnu.zip.sha256"
    unzip "arvo-engine-$ver-x86_64-unknown-linux-gnu.zip" -d ~/.local/arvo
    chmod +x ~/.local/arvo/arvo-engine ~/.local/arvo/arvo-mcp-server
    ```

!!! note "While the repository is private"
    `gh release download` needs the [GitHub CLI](https://cli.github.com) signed
    in to an account with access. Once the repository is public, `curl -L` on
    the asset URL is enough.

Put the directory on your `PATH`, or call the binary by its full path — nothing
in the engine depends on being installed anywhere in particular.

## From source

You need a stable Rust toolchain and nothing else. The submodule holds the
contract crates, so `--recurse-submodules` is not optional.

```sh
git clone --recurse-submodules https://github.com/wjpin84/arvo-engine
cd arvo-engine
cargo build --release -p arvo-engine -p arvo-mcp-server
```

The binaries land in `target/release/`. A release build of the whole workspace
takes several minutes cold and is worth it: a debug engine runs research
roughly **15–100× slower**, which reads as a hang rather than as a slow build.

!!! warning "Use the release build for research"
    A leaderboard that takes half a second against a release engine takes
    8–75 seconds against a debug one. If something feels broken, check which
    build you are pointing at before looking further.

## Verify

```sh
arvo-engine help
```

Then serve a project folder and let it write its handshake:

```sh
mkdir -p ~/arvo && arvo-engine ~/arvo
```

It prints the address it is listening on and writes `engine.json` into that
folder. Leave it running and, from a second terminal:

```sh
arvo-engine ~/arvo session list
```

An empty list is a success: the verb found the engine, authenticated with the
token, and asked it a question. If it cannot find the engine, see
[Configure](configure.md#how-a-client-finds-the-engine).

## Upgrade

Replace the binaries. The project folder and the keychain are untouched, and
findings recorded by an older engine still load — every persisted format reads
its own history, and a field added later reads back as absent rather than
failing.

Stop the running engine first (<kbd>Ctrl</kbd>+<kbd>C</kbd>, or
`session halt --all` if it is trading; see [the kill switch](cli.md#halt-the-kill-switch)).

!!! danger "A live session is not upgraded, it is ended"
    Stopping the engine does not flatten positions — it stops watching them.
    If a session holds anything, halt it deliberately and confirm the account is
    flat at the broker before you replace the binary.

## Uninstall

Delete the two binaries. Then decide about the two things that are not in them:

| | |
|---|---|
| Project folders | Yours. Holds the bar library, findings, rulesets and session records. Deleting them destroys research history that cannot be refetched — [option chains are not available after the fact](../features/universes.md) |
| Keychain entries | Remove under `com.arvo` in Keychain Access (macOS), Credential Manager (Windows) or your secret service (Linux) |

The engine writes nothing outside the project folder you give it and the app
data directory; there is no registry key, no service, and no daemon that
survives a reboot.

## Next

[Configure the engine](configure.md), then [run something](examples.md).
