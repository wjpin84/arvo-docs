# Importing from Pine

TradingView hosts a community's worth of strategies, and its tester has no
deflation, no walk-forward and no conservative-cost re-run. Reading a Pine
script into Arvo is not about hosting someone's strategy. It is about running
on it the one thing its own tester cannot.

A script becomes a [rule as data](rulesets.md#rules-as-data), and from there
it is a finding like any other: deflated against your search, walked forward,
re-run under conservative costs, and made to earn five days on paper before
the promotion gate lets it near money.

## What is translated

- **Indicators**: `ta.sma`, `ta.atr`, `ta.highest`, `ta.lowest`, on any of
  the bar's fields.
- **Conditions**: `ta.crossover`, `ta.crossunder`, `>`, `<`, `and`, `or`,
  `not`, and parentheses.
- **Inputs**: `input.int`, `input.float` and `input` become the rule's
  parameters, with their defaults, so a ruleset can search them.
- **Trading**: `strategy.entry` and `strategy.close` inside an `if`. Long
  only, one instrument, one interval.

The interval is not in the script, because Pine takes it from the chart. You
say which when you read it.

## What is refused, and why that matters

Everything else is **named**, and all of it at once, so you can see what the
script would have to lose:

```
3 construct(s) this cannot translate: request.security: another symbol or
timeframe; a rule here runs on one instrument at one interval; ta.rsi: this
build's indicators are SMA, ATR, MAX and MIN; strategy.short: every rule
here is long only
```

A silently dropped `request.security`, or a dropped short, leaves a rule that
runs, produces a curve, and is not the strategy anybody wrote. That is worse
than a refusal, so there are no silent drops.

Drawing is the one exception. `plot` and its neighbours change nothing about
what a rule trades and appear in nearly every published script, so they are
read, set aside, and **listed** as set aside.

An inclusive comparison (`>=`) is refused rather than quietly made strict,
because the difference is the entry's edge.

## Reading one

On the command line, which needs no engine:

```
arvo-engine [<data-dir>] pine strategy.pine
arvo-engine [<data-dir>] pine strategy.pine --interval 5minute --keep
```

Without `--keep` it prints the rule for you to read. With it, the rule is
written under `rules/` and the picker offers it at once.

The agent has `translate_pine`, which writes nothing: it returns the rule,
and `write_rule` keeps it. Reading a script and keeping it are separate
decisions on purpose, and reading forty scripts to keep one is a search of
forty that your findings are deflated against.

## Provenance

A rule read from a script records where it came from: the language, the
script's title, the author as the script names them, and a hash of the
original text. A finding on that rule can then be traced back to what it was
translated from, which is the only way to tell later whether the translation
was faithful.

## What it is not

Not a Pine interpreter. It reads a script the way a person does, looking for
the trade, and when it cannot be sure it refuses. A script that reads is a
script whose every line it understood. Repainting semantics, Pine's own
drawing and the strategy's account settings are outside it; Arvo's own
[risk gate](risk.md) sizes every trade.
