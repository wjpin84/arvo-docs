# Yahoo Finance

**Source** — daily bars and dividends (venues `YF` and `YFTR`), no
credential. It runs as a **provider process**, `arvo-plugin-yahoo`, started
and supervised by the engine: the first real plugin, built against the
published [provider protocol](../extend/providers.md), which is why it
exists as one.

**The price stream** — the watchlist's live prices come from Yahoo's
streaming endpoint, which needs no key. Ticks are display-only: a tick never
becomes a bar, and nothing in the library is written from a socket.

**No executor** — nothing to sign in to, nothing to order.
