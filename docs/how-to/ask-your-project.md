# Ask your project a question

A project is a folder of files. The window and the agent's tools answer the
questions Arvo was built to answer. For any other question, open the folder
with [DuckDB](https://duckdb.org/) and ask in SQL.

The engine links no database. It writes one file, `.arvo/views.sql`, that names
each of the project's stores as a view. DuckDB reads the files where they are.

## Open it

Install DuckDB once, then from the project folder:

```sh
duckdb -init .arvo/views.sql
```

The paths in the file are relative, so run it from the project folder.

The engine writes the file when it starts and when a store changes form. To
write it without starting the engine, or to see it without writing anything:

```sh
arvo-engine ~/arvo views
arvo-engine ~/arvo views --print
```

## What is there

A view exists when its store does. A new project with no findings has no
`findings` view.

| View | What it holds |
|---|---|
| `bars` | Every bar in the library: `instrument`, `interval`, `time`, and the prices. `time` is the bar's open, in UTC |
| `dividends` | Cash dividends by `ex_date` |
| `fetches` | One row per fetch: when, which source, what was asked, the hash before and after, and `change`, which says whether the fetch re-adjusted or revised the bars already held |
| `option_quotes` | Every recorded option chain. `chain` is the underlying and the feed, such as `SPY.indicative` |
| `findings` | One row per finding: `verdict`, `reasons`, `author`, `trials`, and the whole `record` as JSON |
| `curves` | Every equity curve, one row per point. Join to `findings` on `artifact` |
| `sessions` | Every line of every session's record |
| `audit` | What an agent or a script asked the engine to do |

`option_quotes` reads Parquet for finished days and CSV for today. A query
written against the view does not change when a day is compacted.

## Read, do not write

The engine is the only writer of a project's files. DuckDB can write files too,
so do not point a `COPY ... TO` at the project folder. Write your own results
somewhere else.

An agent that reaches Arvo through its tool list cannot run SQL. These views
are for you, for a script, and for an agent that has a shell.

## Five questions worth asking

### Every refusal, by the reason given

```sql
SELECT reason, count(*) AS findings
FROM (SELECT unnest(reasons) AS reason FROM findings WHERE verdict <> 'Supported')
GROUP BY reason
ORDER BY findings DESC;
```

### How much each author has searched

A finding counts the configurations it tried. Added up by author, that is the
size of the search behind everything that author has claimed.

```sql
SELECT author, count(*) AS findings, sum(trials) AS trials,
       count(*) FILTER (WHERE verdict = 'Supported') AS supported
FROM findings
GROUP BY author
ORDER BY trials DESC;
```

### Which members of a universe are behind

```sql
SELECT member AS instrument, min(time)::DATE AS first_bar, max(time)::DATE AS last_bar, count(time) AS bars
FROM (SELECT unnest(instruments) AS member FROM read_json('universes/sp100.json'))
LEFT JOIN bars ON bars.instrument = member AND bars."interval" = '1day'
GROUP BY member
ORDER BY last_bar NULLS FIRST, member;
```

A member with no bars at all sorts first.

### What an option costs to cross

The half-spread by premium, from every chain the engine has recorded. A quote
that had not changed by the next snapshot is counted once.

```sql
SELECT CASE WHEN mid < 0.10 THEN '1 under $0.10' WHEN mid < 1 THEN '2 $0.10 to $1'
            WHEN mid < 3 THEN '3 $1 to $3' WHEN mid < 10 THEN '4 $3 to $10' ELSE '5 $10 and over' END AS premium,
       count(*) AS quotes,
       round(quantile_cont(half_spread, 0.5), 3) AS p50,
       round(quantile_cont(half_spread, 0.9), 3) AS p90
FROM (SELECT DISTINCT symbol, quote_at, (ask + bid) / 2 AS mid, (ask - bid) / 2 AS half_spread
      FROM option_quotes
      WHERE chain = 'SPY.indicative' AND ask > 0 AND ask >= bid)
GROUP BY premium
ORDER BY premium;
```

The engine prints the same table, beside what the cost model charges:

```sh
arvo-engine ~/arvo option-quotes spreads SPY
```

### What an agent asked for

```sql
SELECT agent, tool, count(*) AS calls, count(*) FILTER (WHERE NOT ok) AS refused, max(time)::DATE AS last_call
FROM audit
GROUP BY agent, tool
ORDER BY calls DESC;
```

## A finding's curve

A finding's curves are kept beside its record, in a file named by a hash of
what they hold. `findings.artifact` is that hash, and `curves.series` says
which curve of the finding a row belongs to.

```sql
SELECT f.id, f.verdict, count(*) AS points, round(last(c.equity ORDER BY c.time), 2) AS final_equity
FROM findings f JOIN curves c ON c.artifact = f.artifact
WHERE f.kind = 'study' AND c.series LIKE '%/strategy_curve'
GROUP BY f.id, f.verdict
ORDER BY f.id DESC;
```

A finding recorded before curves were kept apart carries them inside `record`
and has no rows in `curves`. Rewrite the store once, with the engine stopped,
to move them:

```sh
arvo-engine ~/arvo evidence rewrite
```

It checks each finding reads back as the finding it was before replacing its
file, and it leaves alone any finding it cannot read.

## Anything not in a column

`findings.record` is the finding's whole record as JSON. Reach into it with
DuckDB's JSON functions:

```sql
SELECT id, json_extract(record, '$.out_of_sample_evidence.evaluation.strategy.sharpe') AS sharpe
FROM findings
WHERE kind = 'study'
ORDER BY recorded_at DESC
LIMIT 10;
```
