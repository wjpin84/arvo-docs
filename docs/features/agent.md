# The Agent panel

View → Agent (++ctrl+alt+i++) opens a chat to the right of the editors. It
is an **Agent Client Protocol** client — the protocol Zed's agent panel
speaks — not a model call: Arvo launches an agent as a subprocess, hands it
Arvo's research tools, streams its turn into the panel, puts its permission
requests to you, and serves its file reads and writes inside the project
folder.

By default the agent is **Claude Code**, through the protocol's adapter
(`npx @agentclientprotocol/claude-agent-acp`), using your own Claude login.
The `agent.command` setting names any other ACP agent.

## What the agent can do

- read findings, list rules and rulesets, write a ruleset, run a study or a
  walk-forward, look at the bars a study saw and the regime each closed in,
  and read findings against each other — Arvo's MCP tools, behind the
  **research token**, which has no call that fetches data, touches a
  credential or trades;
- read and write files in the project (a ruleset it writes appears in the
  editor);
- run commands in the project folder, each behind a permission prompt, with
  the output under the tool card.

What it cannot do, by construction: reach an account, an order, or anything
outside the project.

## Where the agent fits in automated trading

Arvo's end goal is automated trading: a rule that survived research runs
against a venue with no person at each order. That is what a
[session](sessions.md) is. The agent's role is to **set up the rules that
trade** — author rulesets, run studies, read the advice, and in time
promote a finding to a session and adjust it from the daily reviews. The
rule trades; the agent researches and operates. Every order is still one
the gate sized and the record can explain back to a bar and a rule.

The other meaning — the agent looks at the market and places an order
because it decided to — is deliberately not built. An order like that has
no rule behind it to backtest, no finding to go stale and no search to
deflate against; nothing explains it but a transcript. If it is ever
built, it goes through the gate like any other proposer, and it takes a
decision that supersedes the current one. [Where this is going →](../about/horizon.md)

## What the panel shows

The agent's words; each tool call as a card with its state, the files it
touched as links into the editor and the finding it produced as a link into
Runs; permission prompts with the agent's own options (*allow once*, *allow
always*, *reject*). A finding the agent makes appears under **agent chat**
in the Runs tab beside the scripts' groups.
