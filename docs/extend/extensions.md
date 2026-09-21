# Extensions

An extension is a folder with a manifest, `arvo-extension.json`, and
whatever else it ships. Arvo installs one from a GitHub repository or from a
folder dropped into its extensions directory (Extensions view → *Extensions
directory*), and lists what it contributes.

```json
{
  "id": "midnight",
  "name": "Midnight",
  "version": "0.1.0",
  "description": "A dark theme and a tighter crossover.",
  "contributes": {
    "themes": [
      { "id": "deep", "label": "Midnight Deep", "base": "dark",
        "colors": { "--color-bg": "#0b0d12" } }
    ],
    "strategies": [
      { "name": "fast-cross", "label": "Faster crossover",
        "premise": "The same rule Arvo ships, over a tighter grid.",
        "interval": { "step": 1, "unit": "day" },
        "kind": "grid", "rule": "sma_cross",
        "axes": { "fast": [3, 5], "slow": [20, 40] } }
    ],
    "providers": [
      { "id": "acme", "services": ["arvo.source.v1.Source"],
        "build": "cargo build --release", "run": "target/release/acme-source" }
    ]
  }
}
```

Two ways to contribute, and a line between them:

- **Declarative** — data Arvo reads and renders with its own components:
  themes, and strategies as documents. No code runs anywhere.
- **A provider** — logic runs as its own process, speaking gRPC over
  loopback, started and stopped by Arvo. The process boundary is the
  sandbox. [Writing a provider →](providers.md)

There is no third way: no third-party code runs inside Arvo's window, in
any form. A provider is built on install from its recipe (`build`, `run`,
split on whitespace, no shell), or downloaded from an `assets` entry for
the platform with a required, checked `sha256` — it is an executable Arvo
will run with your privileges, and the manifest says so before you confirm.

The contract lives in [arvo-extension-api](https://github.com/wjpin84/arvo-extension-api):
`manifest.schema.json`, the protos, and `PROTOCOL.md`. It is public, so a
provider can be built without any of Arvo in front of you.
