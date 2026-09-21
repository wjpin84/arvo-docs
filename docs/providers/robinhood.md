# Robinhood

**Source** — daily bars, quotes, option chains and quotes (venue `RH`),
behind a sign-in. The live price stream for the watchlist comes from
Yahoo, not from here.

**Executor** — `robinhood-<last four>`: a login holds more than one account,
and a session trades exactly one, named by the last four digits of its
number. Real money; there is no paper account.

**Holdings** — Portfolio syncs each account's positions, which is what the
watchlist starts from and what a book study is sized against.

Sign in from Accounts. An expired session is a problem on the dashboard,
an alert, and a line in the Operations view until you sign in again.
