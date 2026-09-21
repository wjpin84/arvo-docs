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
  walk-forward — Arvo's MCP tools, behind the **research token**, which has
  no call that fetches data, touches a credential or trades;
- read and write files in the project (a ruleset it writes appears in the
  editor);
- run commands in the project folder, each behind a permission prompt, with
  the output under the tool card.

What it cannot do, by construction: reach an account, an order, or anything
outside the project.

## What the panel shows

The agent's words; each tool call as a card with its state, the files it
touched as links into the editor and the finding it produced as a link into
Runs; permission prompts with the agent's own options (*allow once*, *allow
always*, *reject*). A finding the agent makes appears under **agent chat**
in the Runs tab beside the scripts' groups.
