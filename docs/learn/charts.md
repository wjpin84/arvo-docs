---
title: Charts
description: Level 2 — how a bar is built, what a timeframe chooses for you, and the four things a chart cannot show.
---

# Charts

*Level 2. Assumes [markets](markets.md).*

## The idea

A chart is not a recording of what happened. It is a **summary**, and every
summary throws something away. Learning which things were thrown away is most of
what separates reading a chart from being fooled by one.

One bar covers a period of time and keeps four numbers from it:

| | |
|---|---|
| **Open** | The first trade of the period |
| **High** | The highest trade |
| **Low** | The lowest trade |
| **Close** | The last trade |

A candle draws the same four numbers with the open-to-close range as a body and
the rest as wicks. Candles and bars carry identical information, so the choice
between them is cosmetic — though the candle's shape is a genuinely faster way to
read what happened inside the period, which is the next section.

**Volume** is how many units changed hands in the period. It is the only figure
on a standard chart that is not a price.

## Why it matters

**A bar destroys the order of events inside it.** A daily bar tells you the high
and the low, and *nothing* about which came first. This is not a detail — it is
the reason honest backtesting is hard. If a bar's low would have stopped you out
and its high would have hit your target, no amount of staring at the bar decides
which happened, and a backtest that quietly picks the profitable one is lying to
you.

**The timeframe is a decision, not a setting.** A five-minute chart and a daily
chart of the same instrument are different data, not different zoom levels. A
pattern that appears on one is often absent on the other, and switching
timeframes until a rule looks good is a search — one that counts, and that Arvo
holds you to.

## What it looks like

### A candle, in parts

Four numbers, drawn. The body spans open to close, the wicks reach out to the
high and the low, and the colour says which way the period closed.

<figure class="arvo-figure">
<svg viewBox="0 0 460 250" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>The parts of a candle</title>
  <desc>An up candle and a down candle, with the open, high, low and close labelled on each.</desc>
  <g stroke="currentColor" stroke-dasharray="3 3" opacity="0.45" stroke-width="1">
    <line x1="116" y1="25" x2="138" y2="25"/>
    <line x1="116" y1="75" x2="123" y2="75"/>
    <line x1="116" y1="175" x2="123" y2="175"/>
    <line x1="116" y1="210" x2="138" y2="210"/>
    <line x1="354" y1="25" x2="332" y2="25"/>
    <line x1="354" y1="65" x2="347" y2="65"/>
    <line x1="354" y1="165" x2="347" y2="165"/>
    <line x1="354" y1="210" x2="332" y2="210"/>
    <line x1="175" y1="50" x2="142" y2="50"/>
    <line x1="175" y1="125" x2="157" y2="125"/>
    <line x1="175" y1="195" x2="142" y2="195"/>
  </g>
  <g stroke="#3a9d5d" stroke-width="2">
    <line x1="140" y1="25" x2="140" y2="75"/>
    <line x1="140" y1="175" x2="140" y2="210"/>
  </g>
  <rect x="123" y="75" width="34" height="100" fill="#3a9d5d"/>
  <g stroke="#d64545" stroke-width="2">
    <line x1="330" y1="25" x2="330" y2="65"/>
    <line x1="330" y1="165" x2="330" y2="210"/>
  </g>
  <rect x="313" y="65" width="34" height="100" fill="#d64545"/>
  <g fill="currentColor" font-size="13" font-family="inherit">
    <g text-anchor="end">
      <text x="112" y="29">High</text>
      <text x="112" y="79">Close</text>
      <text x="112" y="179">Open</text>
      <text x="112" y="214">Low</text>
    </g>
    <g text-anchor="start">
      <text x="358" y="29">High</text>
      <text x="358" y="69">Open</text>
      <text x="358" y="169">Close</text>
      <text x="358" y="214">Low</text>
      <text x="178" y="54" opacity="0.7">upper wick</text>
      <text x="178" y="129" opacity="0.7">body</text>
      <text x="178" y="199" opacity="0.7">lower wick</text>
    </g>
    <g text-anchor="middle" font-size="12" opacity="0.75">
      <text x="140" y="238">Closed higher</text>
      <text x="330" y="238">Closed lower</text>
    </g>
  </g>
</svg>
<figcaption>Open and close swap places depending on direction. Everything else is the same drawing.</figcaption>
</figure>

Wicks are also called shadows, and the two words mean the same thing.

### The shapes, and what each one tells you

Every shape below is the same four numbers in a different arrangement. Read them
as a **description of what happened during that period** — that is what the shape
reliably tells you, and it is all it tells you.

<figure class="arvo-figure arvo-figure--wide">
<svg viewBox="0 0 720 215" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>Six candle shapes</title>
  <desc>Six candles showing a long body, a small body with long wicks, a long upper wick, a long lower wick, a body with no wicks, and a candle whose open equals its close.</desc>
  <g stroke="#3a9d5d" stroke-width="2">
    <line x1="60" y1="25" x2="60" y2="35"/>
    <line x1="60" y1="160" x2="60" y2="170"/>
    <line x1="180" y1="20" x2="180" y2="90"/>
    <line x1="180" y1="105" x2="180" y2="175"/>
    <line x1="420" y1="25" x2="420" y2="35"/>
    <line x1="420" y1="62" x2="420" y2="175"/>
  </g>
  <g stroke="#d64545" stroke-width="2">
    <line x1="300" y1="20" x2="300" y2="135"/>
    <line x1="300" y1="160" x2="300" y2="170"/>
  </g>
  <g stroke="currentColor" stroke-width="2">
    <line x1="660" y1="30" x2="660" y2="98"/>
    <line x1="660" y1="103" x2="660" y2="170"/>
  </g>
  <rect x="45" y="35" width="30" height="125" fill="#3a9d5d"/>
  <rect x="165" y="90" width="30" height="15" fill="#3a9d5d"/>
  <rect x="285" y="135" width="30" height="25" fill="#d64545"/>
  <rect x="405" y="35" width="30" height="27" fill="#3a9d5d"/>
  <rect x="525" y="45" width="30" height="110" fill="#3a9d5d"/>
  <rect x="645" y="98" width="30" height="5" fill="currentColor"/>
  <g fill="currentColor" font-size="13" text-anchor="middle" font-family="inherit" opacity="0.8">
    <text x="60" y="200">1</text>
    <text x="180" y="200">2</text>
    <text x="300" y="200">3</text>
    <text x="420" y="200">4</text>
    <text x="540" y="200">5</text>
    <text x="660" y="200">6</text>
  </g>
</svg>
</figure>

| | Shape | What happened |
|---|---|---|
| 1 | Long body, short wicks | It opened near one end of the range and closed near the other. Price moved one way and mostly stayed there |
| 2 | Small body, long wicks both sides | It travelled a long way in both directions and finished near where it began. Plenty of activity, no resolution |
| 3 | Long upper wick, small body below | It pushed well above where it ended and gave all of it back within the period |
| 4 | Long lower wick, small body above | The reverse: it fell well below where it ended and recovered before the close |
| 5 | Body with no wicks | It never traded outside its open-to-close range. One direction, start to finish |
| 6 | Open and close equal | It ended exactly where it started, whatever it did in between |

!!! warning "This is where candle reading stops and pattern trading begins"
    Each row above describes the period that just finished. None of them predicts
    the next one, and the moment a shape is given a name and a forecast —
    *hammer*, *engulfing*, *morning star* — it has become a claim that needs
    testing rather than an observation.

    Those claims are exactly the category [level 6
    excludes](families/index.md#what-is-not-here): patterns read by eye. The
    honest route is not to trust them or to dismiss them, but to write one as a
    [rule in data](../features/rulesets.md#rules-as-data) and put it through the
    same deflation and cost tiers as everything else. Nobody here has done that
    yet, and either answer would be worth having.

### One thing that does not travel between markets

Candle *anatomy* is universal — four numbers, drawn the same way in every market
on earth. Two things about candles are **not**, and it matters if you read
material written for a different market than the one you trade:

- **Gaps need a market that closes.** US equities close overnight and gap
  routinely, so a candle's open often sits far from the previous close. Forex runs
  24 hours, five days a week, and gaps essentially only over the weekend. Any
  shape or pattern defined by a gap is common in one market and nearly absent in
  the other.
- **In a 24-hour market the open and close are arbitrary.** They fall wherever the
  day is rolled, so two venues can draw different candles from identical prices. A
  US equity's open and close are real auction events with real volume behind them.

### Three intervals, three questions

Same instrument, three intervals, three different questions:

| Interval | What a bar is | What you can ask |
|---|---|---|
| 1 minute | a minute of trading | Where the liquidity is, how a fill behaves |
| 5 minutes | a slice of a session | What happened around the open, VWAP, an intraday range |
| 1 day | a whole session | Whether a move persisted for weeks |

Longer bars mean fewer bars. Two years of daily data is about five hundred bars
and, for a typical rule, a few dozen trades — which as you will see in [level
8](reading-a-verdict.md) is usually not enough to conclude anything at all.

## What a chart cannot show you

Four things, and each one has cost somebody money.

**1. Distributions.** Most historical price series are *split-adjusted* — past
prices are rescaled so a 4-for-1 split does not appear as a 75% crash. Useful,
but adjusted series handle dividends inconsistently, and a dividend paid is
return you received that the price line never shows. On a dividend payer over two
years, that missing return can be larger than the margin a strategy appears to
have won by.

**2. What you could actually have traded.** The chart shows the last price. It
does not show the spread at that moment, or whether there was any size available.
A backtest filling at the close of a thin stock is assuming a counterparty that
may not have existed.

**3. The names that are not there.** Charts exist for companies that still exist.
A list of today's index members contains only the ones that survived — the
failures were removed, and their charts are not in front of you. Testing a rule
on today's winners and concluding the rule works is called **survivorship bias**,
and it is invisible precisely because the evidence was deleted.

**4. Everything else that was happening.** One instrument's chart hides whether
the whole market rose that month. A rule that made 20% in a year the index made
25% lost money in the only sense that matters, and the single chart will not tell
you.

!!! warning "Arvo can defend you against two of these, not four"
    The dividend gap is measured and reported beside every result, and the cost
    model refuses to assume free trading. **Survivorship bias it cannot see** —
    a universe of today's index members passes every gate honestly, because no
    search was performed to select them. This is why a
    [universe](../features/universes.md) in Arvo refuses to exist without a
    written reason for its membership. That reason is the only defence available,
    and it is yours to write.

## See it in Arvo

- A rule is **defined at an interval** and a ruleset cannot move it. A one-minute
  `sma_cross` is refused with the reason, rather than run to produce a curve
  measuring something nobody asked for.
- Bars are fetched to files and **pinned by content hash**. A study never reads a
  vendor live, so the same study over the same library gives the same answer next
  year — and if a refetch changes the data, every finding made from it is marked
  **stale** with the reason.
- The **dividend gap** sits beside every result: the return the split-adjusted
  series cannot see, measured rather than folded in.
- Every study is scored **against holding the instrument**, so point 4 above is
  answered by default. Beating the benchmark is the bar, not making money.

## Watch

<!-- TODO: curate. Channel name and year beside each link. -->

- *(to be chosen)* — how a candle is built, bar by bar, as a session runs.
- *(to be chosen)* — survivorship bias, with a worked example of a fund index.

## Next

[Market types](market-types.md) — trending, ranging, volatile, and why no single
rule suits all three.
