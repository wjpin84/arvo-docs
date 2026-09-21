# Research from Python

```sh
pip install arvo-client        # the package is arvo-client; the module is arvo
```

```python
import arvo

engine = arvo.connect()                      # reads engine.json from the app data directory
found = engine.run_study("AAPL.RH", "sma_cross", author="script:first-look")
print(found.verdict)                         # read these three before any number
print(found.read_this_first)
for item in found.advice:
    print(item.severity, item.finding, "->", item.action)
print(found.detail["out_of_sample"])

prices = engine.bars("AAPL.RH")              # a pandas DataFrame, read-only
```

`author` is required and it matters: a run is saved as that author's
finding and deflated against everything the author has run, so a script
trying configurations until one passes does not make it pass.

The client holds the **research token** only. It can list strategies,
instruments and findings, open a finding, run a study or a walk-forward,
read bars, and **report** evidence it computed elsewhere (a curve and a
ledger; Arvo computes the verdict). It cannot fetch, sign in or trade; the
generated stubs under `arvo.<domain>.v1` can reach the control tier with the
token from `control.json`, which is how a front end you write yourself
would.

A script run from the editor, or on a schedule, is the same thing with
`script:<name>` as its author and its findings in the Runs tab.
[The API and the packages →](../reference/api.md)
