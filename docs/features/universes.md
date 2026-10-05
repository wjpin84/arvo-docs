# Universes

A universe is a named list of instruments, chosen for a reason other than
their returns, that the engine keeps fetched. It is a file in the project,
`universes/<name>.json`:

```json
{
  "name": "etf30",
  "reason": "The thirty most-traded US ETFs by volume: liquidity, not returns.",
  "interval": { "step": 1, "unit": "day" },
  "since": "2016-01-01",
  "instruments": ["SPY.YF", "QQQ.YF", "IWM.YF"]
}
```

The **reason** is required and is shown wherever the universe is. A list
chosen by looking at returns is the search the platform exists to deflate,
and the file says out loud how it was chosen: an index's members, a
liquidity floor, a sector. Each instrument is `SYMBOL.VENUE`, on a venue a
source serves; a file that names no reason, an unknown venue or a member
twice is refused with the reason.

## Kept fetched

Every hour the engine's `universes` job fetches each member whose
series is missing or behind the last completed bar, through the source that
serves its venue, back to `since` on first fill. A daily series is due once
the date has rolled over in UTC. An intraday series is due five minutes after
the regular close, so the day's bars are there when its review is read. A member that fails is named
and the rest are still fetched. The same on demand:

```
arvo-engine [<data-dir>] universes            each universe and its members' coverage
arvo-engine [<data-dir>] universes refresh    fetch what is missing or behind
```

## A panel over a universe

`run_panel` takes a universe and, optionally, a rule: one rule, one parameter
set, every member at once, so the question is whether the rule holds across
names rather than on the one name it was tried on. The finding records the
universe, its reason and its size, and carries notes that say what to keep
in mind:

- the universe's size is the search that keeping its best member would be,
  and the panel's deflation is over the grid, not the members;
- membership is as listed today, not point-in-time
  ([#9](https://github.com/wjpin84/arvo-desktop/issues/9));
- members with no series at the interval yet were left out, by name.

A panel an agent or a script runs is saved as that author's finding and
deflated against everything the author has run, as a study is. From Python
`author` is required.

`Research.RunPanel` in the contract; `run_panel` on the agent's research
tier. The whole-library panel (`ViewPanel`) is unchanged.

## The first universes

The project ships with three, written by hand and named for what they are:
the S&P 100 as listed on the day the file was written, the thirty most-traded
US ETFs, and ten liquid names at five minutes for the intraday rules. Keeping
them fetched is also how the library's cost was measured: 160 series are about
50 MB, and loading all hundred members of the S&P 100 takes about half a
second of a panel that runs for minutes. So the bar library stays as CSV files
until something larger needs otherwise
([arvo-engine #32](https://github.com/wjpin84/arvo-engine/issues/32)).
