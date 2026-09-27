---
title: What you can do with Arvo
description: The seven things Arvo is for, in the order you would do them.
---

# What you can do with Arvo

Seven things, in roughly the order you would do them. Each one links to the page
that walks you through it.

## Test whether a trading idea holds up

*"Does this actually work, or did I just find a number I liked?"*

Describe an idea as a rule, choose what to test it on, and run a study. You get
back a **finding**: a verdict, plain-language advice, and the evidence behind
both.

The verdict is one of three — `Supported`, `NotSupported`, or `Inconclusive`.
The third one matters: it means *there isn't enough evidence to say*, which is a
real answer and one most tools cannot give you.

[Run a study and read the verdict →](../how-to/run-a-study.md)

## Search parameters without fooling yourself

*"Which settings work best?"*

Run a rule over a grid of parameter values. Arvo picks a winner on one slice of
history and judges it on a slice it never saw — then checks whether the winner
beats what the *best of that many tries* would score by luck alone.

So a parameter search gives you a result you can weigh, rather than the best of
a hundred guesses wearing a Sharpe ratio.

[Write a ruleset →](../how-to/write-a-ruleset.md)

## Test one idea across many instruments

*"Does this work generally, or just on the one chart I looked at?"*

A panel runs one rule across a whole list of instruments, with **one
configuration for all of them**. Tuning per instrument would be a second search;
holding the configuration fixed means the rule has to work broadly to pass at
all.

It also pools evidence, so a rule that trades rarely can accumulate enough
trades to say something.

[Universes →](../features/universes.md)

## Bring in a strategy you already have

*"I have this in TradingView."*

Point Arvo at a Pine v5 script and it reads what it can express as a rule — and
**tells you by name what it could not**, rather than translating approximately
and leaving you with a rule that quietly does something else.

The result is then a rule like any other, and goes through the same evaluation.

[Import from Pine →](../features/pine.md)

## Paper trade it against a real broker

*"Would this behave the same way with real market conditions?"*

Take a finding and run it on a broker's paper endpoint: real API, real fills,
real latency, simulated money. Arvo compares the price the rule decided at
against the price it actually got, every time.

That difference is the thing a backtest assumes and cannot measure. It is why
five paper days are required before a rule can touch real money.

[Start a paper session →](../how-to/paper-session.md)

## Let it run

*"Can I stop watching it?"*

A session keeps trading with nothing open — no window, no terminal, no agent
attached. It checks its own positions against the broker on every poll, and if
the two disagree it **stops taking new positions and waits for you** rather than
trading on a position it is not sure it holds.

One kill switch stops everything, and it stays stopped until a person clears it.

[Sessions →](../features/sessions.md) · [Halt trading →](../how-to/halt.md)

## Find out why you own something

*"Why did it buy this?"*

Walk backwards from any position: the fill, the order, the risk decision that
sized it, the signal, the rule that fired, and the bar it fired on — with the
market conditions at the time.

Not a log you search through. A record built to answer that question.

[Explain a position →](../how-to/explain.md)

## Work with an AI agent

*"Can Claude help me with this?"*

Attach an agent and it can write rules, run studies, and read every finding. It
**cannot** place an order, reach a broker, or touch a credential — not by
policy, but because the tools that would let it do those things do not exist on
the surface it is given.

Starting a session stays a person's decision.

[Ask the agent →](../how-to/agent.md) · [With Claude Code →](../engine/claude.md)

## What Arvo will not do for you

Worth saying plainly, because it is the other half of the product:

- **It will not tell you what to trade.** No recommendations, no signals, no
  shared list of winning strategies.
- **It will not make a weak idea look good.** Fewer results will pass here than
  elsewhere. That is the point, not a limitation.
- **It will not trade on its own initiative**, and neither will an agent you
  attach to it.

## Next

- Why any of this is unusual: [Why Arvo is different](../concepts/index.md)
- The vocabulary: [Core concepts](../concepts/trading.md)
- Get it running: [Install](../getting-started/install.md), or
  [run it headless](../engine/index.md) with no window at all
