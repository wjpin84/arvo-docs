# Alpaca

The vendor that does the most: bars on two feeds, option quotes, holdings,
and the one paper venue.

**Sources** — `alpaca-iex` (free plan, venue `AIEX`) and `alpaca-sip` (paid
plan, `ASIP`), each also as a total-return source (`AIEXTR`, `ASIPTR`).
Daily and intraday bars, corporate actions for the dividend gap. Intraday
bars are trimmed to the regular session.

**Executors** — `alpaca-paper` and `alpaca-live`. Market orders only, by
design: a limit price is a second risk decision the gate has no opinion
about. The session records what each fill cost against the price the
decision was made at; that difference is the point of paper trading.

**Streaming** — one-minute bars over `wss://stream.data.alpaca.markets`,
on the feed the source fetches from, aggregated to the rule's interval.

**Keys** — one pair per sub-account (paper, live), typed once in Accounts.
Bars use whichever pair exists, paper preferred.

!!! warning "Crypto"
    Arvo does not trade crypto. A crypto position that appears in the paper
    account (placed by hand, say) is adopted by a running session's
    reconciliation and can be seen in the Operations view, but the executor
    cannot exit it: Alpaca wants a different time-in-force for crypto orders,
    and the session says so in its record rather than pretending.
