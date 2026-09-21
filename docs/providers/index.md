# Sources and executors

Two kinds of provider, and the difference matters:

- A **source** serves bars (and sometimes dividends, quotes, option chains).
  Bars are fetched to files in the project's library and pinned by content
  hash; a study never reads a vendor live, so a stored finding can be checked
  against exactly the data it saw.
- An **executor** takes orders. `alpaca-paper` is Alpaca's paper endpoint —
  a real API with simulated money, which is what makes its slippage a
  measurement rather than an assumption. `alpaca-live` and
  `robinhood-<last four>` are real money and say so by name.

## Venues

An instrument is named `SYMBOL.VENUE`, and the venue names **where the bars
came from**, not where an order goes. Two vendors' copies of the same ticker
are two datasets with two hashes, filed apart on purpose.

| Venue | Source | Basis |
|---|---|---|
| `AIEX` | Alpaca, IEX feed (free) | split-adjusted |
| `ASIP` | Alpaca, SIP feed (paid) | split-adjusted |
| `AIEXTR`, `ASIPTR` | the same feeds, total-return | distributions reinvested at the ex-date |
| `RH` | Robinhood | split-adjusted |
| `YF` | Yahoo Finance | split-adjusted |
| `YFTR` | Yahoo Finance, total-return | distributions reinvested |
| `SIM` | shipped simulated series | — |
| `AOPT` | option contracts, by OCC symbol | — |

Split-adjusted prices leave dividends out, which always favours a strategy
that holds; Arvo measures that gap beside the result rather than folding it
in. A study on a total-return venue records that basis.

## Streaming

A session on an intraday rule asks its source for a live feed. Alpaca
streams one-minute bars over a websocket, aggregated to the rule's interval
(five minutes, say) and delivered a moment after each bar closes; every
other source is polled once a minute. A feed that goes dark freezes the
session and lifts the freeze itself when the feed returns.

## Intervals

`1day` everywhere; `5minute` (and other minute steps) where the source
serves it. Intraday bars are kept to the regular session — pre-market and
after-hours prints are dropped, whatever the vendor sends — so an opening
range means the opening range.
