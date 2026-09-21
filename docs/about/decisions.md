# Decisions

Every decision with consequences is an architecture decision record in
[arvo-adrs](https://github.com/wjpin84/arvo-adrs): one numbered file,
immutable once accepted, superseded rather than edited. The ones a user of
Arvo meets:

| | |
|---|---|
| [0006](https://github.com/wjpin84/arvo-adrs/blob/main/0006-live-execution-is-a-separate-decision.md) | Live execution is a separate decision, not a configuration flag |
| [0008](https://github.com/wjpin84/arvo-adrs/blob/main/0008-fetch-writes-files.md) | A fetch writes files; a study reads files |
| [0009](https://github.com/wjpin84/arvo-adrs/blob/main/0009-one-risk-policy.md) | One risk policy, in the backtest and in a session |
| [0010](https://github.com/wjpin84/arvo-adrs/blob/main/0010-paper-fills-at-the-next-price.md) | Paper fills at the next price, never the requested one |
| [0011](https://github.com/wjpin84/arvo-adrs/blob/main/0011-dividend-gap-beside-not-folded-in.md) | The dividend gap is measured beside a result, not folded in |
| [0013](https://github.com/wjpin84/arvo-adrs/blob/main/0013-dividends-arrive-as-reinvestment.md) | Total-return venues reinvest distributions at the ex-date |
| [0016](https://github.com/wjpin84/arvo-adrs/blob/main/0016-an-agent-reaches-arvo-through-a-tool-list-that-cannot-trade.md) | An agent reaches Arvo through a tool list that cannot trade |
| [0018](https://github.com/wjpin84/arvo-adrs/blob/main/0018-arvo-keeps-running-when-the-window-closes.md) | Arvo keeps running when the window closes |
| [0019](https://github.com/wjpin84/arvo-adrs/blob/main/0019-the-workbench-is-commands-first.md) | The workbench is commands first |
| [0020](https://github.com/wjpin84/arvo-adrs/blob/main/0020-settings-are-a-file-the-person-can-read.md) | Settings are a file the person can read |
| [0022](https://github.com/wjpin84/arvo-adrs/blob/main/0022-a-plugin-serves-an-earned-trait-and-the-sources-go-first.md) | A plugin serves an earned trait, and the sources go first |
| [0023](https://github.com/wjpin84/arvo-adrs/blob/main/0023-arvo-owns-the-lifecycle-of-what-it-spawns.md) | Arvo owns the lifecycle of what it spawns |
| [0024](https://github.com/wjpin84/arvo-adrs/blob/main/0024-an-extension-contributes-to-points-and-runs-no-code-in-the-window.md) | An extension contributes to points and runs no code in the window |
| [0026](https://github.com/wjpin84/arvo-adrs/blob/main/0026-the-ledger-accepts-evidence-it-did-not-compute.md) | The ledger accepts evidence it did not compute; Arvo computes the verdict |
| [0028](https://github.com/wjpin84/arvo-adrs/blob/main/0028-the-engine-owns-a-venue-session.md) | The engine owns a venue session |
| [0029](https://github.com/wjpin84/arvo-adrs/blob/main/0029-the-engine-hosts-the-plugins.md) | The engine hosts the plugins |
