# Accounts and credentials

Open **Accounts** in the activity bar. Each vendor is a row: what it needs
from you, whether it has it, and what it gives back (`bars`, `quotes`,
`holdings`, `option quotes`).

| Vendor | Credential | Gives | Notes |
|---|---|---|---|
| **Alpaca** | an API key pair, typed once | bars (IEX free, SIP paid), option quotes, holdings, **paper and live trading**, streamed bars | Paper and live are two sub-accounts with two key pairs from one login. Bars come from whichever pair exists, paper preferred |
| **Robinhood** | sign in (OAuth) | bars, quotes, holdings, option quotes | A login holds more than one account; a session names the one it trades by its last four digits |
| **Yahoo Finance** | none | bars, dividends, the live price stream | Runs as a provider process (`arvo-plugin-yahoo`). No key, no sign-in, and therefore no order |

Credentials go into the operating system's keychain and nowhere else. A
provider never receives a whole credential: each call carries a grant for
that call. An agent never receives any: the research API has no call that
touches one.

## Alpaca

Create a key pair in the Alpaca dashboard (paper keys from the paper
dashboard, live keys from the live one) and paste them in Accounts →
Alpaca. For a headless machine the engine also reads `APCA_API_KEY_ID` and
`APCA_API_SECRET_KEY` from the environment, keychain first.

## Robinhood

Accounts → Robinhood → Sign in opens the vendor's login in your browser;
Arvo keeps the session token. An expired session shows as a problem on the
dashboard and in the Operations view, and raises an alert.

## What "connected" means

A vendor is connected when its credential is there and usable. The status
bar counts them (`Accounts 2/2`); the Operations view lists them with their
sub-accounts. A vendor needing nothing is always connected.
