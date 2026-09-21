# Glossary

**Rule** — a strategy Arvo implements: the code deciding entries and exits. You do not write rules.

**Ruleset** — a document that runs a rule over a search of parameters you choose. You write rulesets. (*Strategy* is the word in code, protocols and stored findings; the window says rule and ruleset.)

**Study** — one rule, one instrument, one search, one verdict. *Walk-forward*, *panel* and *book* are studies of other shapes.

**Finding** — a stored study: verdict, advice, curves, ledger, provenance. Research memory is the set of them.

**Verdict** — `Supported`, `NotSupported` or `Inconclusive`. Computed by Arvo, never supplied.

**Holding / Diverging** — a session's verdict: whether the live trades still look like the finding's out-of-sample trades. Inconclusive until there are enough of them. Judges the rule, not the account; the gate's halts do that.

**Advice** — what a finding says to read before its numbers: a finding in words, evidence, an action, a severity.

**Cost tiers** — realistic (the experiment's stated model), conservative (half again the commission, twice the slippage with a five-point floor, twice the fees) and optimistic (half the commission, nothing else). A Supported result must survive the conservative tier.

**Deflation** — judging the best of a search against what the best of that many random tries would show.

**Out of sample** — the held-out window the selected configuration is scored on; the only numbers reported as a result.

**Benchmark** — holding the instrument. The bar every study is measured against.

**Dividend gap** — the return the split-adjusted series cannot see; measured beside the result, not folded in.

**Stale** — a finding whose data or ruleset changed since it was made.

**Instrument** — `SYMBOL.VENUE`; the venue names where the bars came from. Also the facts the gate sizes against: lot, tick, hours, multiplier.

**Venue** — the namespace bars are filed under (`RH`, `AIEX`, `YF`, `SIM`…). Not where an order goes.

**Source** — a provider of bars (and dividends, quotes, chains). **Executor** — a provider that takes orders.

**Session** — a finding's rule running against an executor. Paper or real money.

**Shadow** — the backtest engine kept alive and fed one bar at a time inside a session, so the live rule is the backtested rule.

**Gate** — the risk policy that sizes or refuses every entry, in a backtest and live. It never refuses an exit.

**Halt** — the gate refusing all entries: the drawdown halt (permanent) or the kill switch (manual, released by a person).

**Freeze** — a session refusing entries while it waits for a person: a book that disagrees with the venue, or a feed gone dark.

**Reconcile** — making the gate's book the venue's. **Resume** — taking entries again, after a reconcile.

**Promotion gate** — what a finding must have before a real-money executor takes it: a Supported verdict, five days on paper, and no Diverging verdict on that paper session.

**Kill switch** — arm the gate, then flatten everything held. *Halt trading* in the tray, `halt` on a session.

**Record** — a session's JSONL file, one event per line, each naming its cause.

**Journal** — on every trade: the rule that fired, its value, the regime at entry, the quantity asked for.

**Regime** — trending up, trending down or ranging, labelled over recent closes.

**Provider** — an extension's process, serving the published protocol. **Extension** — a folder with a manifest contributing themes, strategies as documents, or providers.

**Engine** — `arvo-engine`, the daemon: research, the library, credentials, sessions, the API. **Window** — the desktop, one client of it.

**Research token / control token** — the two keys to the API: what an agent or script gets, and what a person's front end gets.
