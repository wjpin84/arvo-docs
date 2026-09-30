# Skills for Claude Code

[`arvo-skills`](https://github.com/wjpin84/arvo-skills) is a Claude Code
plugin: seven skills and two agents that teach an agent how Arvo works, in
Arvo's own words and numbers. With the MCP server an agent has the *tools*;
with the plugin it has the *discipline* — read the verdict before the
number, count every run as a search, describe a market rather than
recommend one, and say when an idea cannot be written as a rule.

Every threshold in the skills was read from the engine's source and then
tested against a running engine. Nothing in them is general trading lore.

## Install

```
/plugin marketplace add wjpin84/arvo-skills
/plugin install arvo@arvo-skills
```

The skills expect [the MCP server](claude.md) under the name `arvo`, and
they expect it named for the author, so the engine deflates the agent's
whole loop as one search:

```sh
claude mcp add arvo -- ~/.local/arvo/arvo-mcp-server --agent claude
```

Restart Claude Code. The skills appear as `arvo:arvo-research` and so on;
the agents as `arvo:arvo-study` and `arvo:arvo-market`.

## The skills

A skill loads when the request matches its description, or by name
(`/arvo:arvo-rules`). Each is under 140 lines, tables over prose, and ends
with what not to do.

| Skill | Loads when you ask to… |
|---|---|
| `arvo-research` | run or read a study, walk-forward or panel; rank or compare findings; judge whether a result is real; re-run until something passes |
| `arvo-rules` | write a rule, ruleset or universe; translate Pine; understand why `write_rule` refused something |
| `arvo-indicators` | turn a trading idea into a rule; explain a condition; find out why a rule never fires or fires every bar; learn which compiled rule already covers an idea |
| `arvo-market-structure` | describe what an instrument is doing — trend, range, volatility, regime, session, gaps — and turn an observation into a testable premise |
| `arvo-market-data` | explain a result that looks wrong; find missing or bad data; compare two vendors; understand staleness or an instrument id |
| `arvo-risk-and-costs` | understand sizing, stops, the risk model, a refusal, cost tiers, options stress, a live session's verdict, or promotion |
| `arvo-platform` | change code in any Arvo repository: where a change goes, branches, tests, releases, launching |

What they hold to, across all seven:

- **Verdict before number.** A finding is read in the order the engine
  writes it: `read_this_first`, the verdict, the advice, then numbers —
  out-of-sample ones only, each with the cost tier it is under.
- **Every run is a search.** Findings are deflated against everything the
  author has run. The skills never suggest "try another parameter"; a
  second run needs a stated hypothesis.
- **Describe, never recommend.** The market skills return a description
  and one premise a rule could test. Nothing says buy or sell.
- **Say what cannot be expressed.** A breakout against yesterday's channel,
  a Bollinger band, a chart pattern: named as untestable, with the compiled
  rule if one covers it, rather than approximated.

## The agents

An agent runs in its own context with its own tool list. One exists only
where a job produces output the main conversation should not read, or
needs a tool set it should not have.

| Agent | Job | Tools |
|---|---|---|
| `arvo-study` | tests one stated hypothesis, once, and reports the verdict and advice — not the finding's JSON | the `arvo` research tools |
| `arvo-market` | reads an instrument from the library and a live feed, returns a short description and one premise | the `arvo` read tools and a broker's read-only tools — no order, watchlist or scan-writing tool exists in its context |

The second agent exists for the tool restriction more than the context.
When a broker MCP that can place orders is present in the session, a
market-reading task runs where those tools are absent, not where a prompt
says not to use them — the same argument the engine makes for the MCP
server, applied one layer up.

Ask in words: *what is QQQ doing?* runs `arvo-market`; *test whether a
20/50 crossover holds on the S&P 100* runs `arvo-study`. Each study the
agent runs is saved under the author the MCP server was named for.

## Keeping it current

The installed plugin is a copy pinned to a version, not a link to the
repository. After a new version is pushed:

```
/plugin marketplace update arvo-skills
/plugin update arvo@arvo-skills
```

then restart. To work on the skills themselves, load the working tree
directly and skip the cycle:

```sh
claude --plugin-dir ~/arvo-skills
```

## What it is not

- Not a library of trading knowledge. Crypto, DeFi, ICT and chart-pattern
  skills live in other collections; Arvo cannot test them, so the skills
  say so instead of carrying them.
- Not an agent that trades. Starting a session is a person's decision,
  made with the control token; promotion to real money needs five paper
  days whoever asks. [The Agent panel →](../features/agent.md)

## Writing a skill

One folder under `skills/`, a `SKILL.md` whose `name` matches the folder
and whose `description` says *when* to use it. A number or a name goes in
only with the engine file it was read from, so that when the engine
changes the skill is known to be wrong. An agent lists its tools
explicitly, and any broker tool must be a read. `main` takes pull requests.
