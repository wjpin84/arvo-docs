# Arvo in Claude Code and other agents

`arvo-mcp-server` is Arvo as an MCP server over stdio: an agent reads
research memory and runs studies, and cannot fetch, share or reach a
broker — no tool names a credential, a source or an order, and the token it
holds reaches only the research service.

```sh
claude mcp add arvo -- <path-to>/arvo-mcp-server
```

It finds the engine through `engine.json` in the app data directory and,
when none is running, starts one (from `ARVO_ENGINE` or `arvo-engine`
beside itself) that keeps running after the agent's session ends. Pass
`--agent <name>` to say who is running; the findings are saved under that
author and deflated against everything that author has run, and the
engine appends every run to `agent-audit.jsonl` in the project.

| Tool | What it does |
|---|---|
| `list_strategies` | the rules Arvo ships and the rulesets the project and its extensions offer |
| `list_rulesets` / `write_ruleset` | the project's rulesets; write one (refused with the reason when it cannot run) |
| `list_instruments` | what the library holds |
| `list_findings` / `open_finding` | research memory, and one finding in full |
| `run_study` / `run_walk_forward` | run, and answer with the finding |

The Agent panel in the window is the same server attached to an agent over
the Agent Client Protocol; from outside the window, this is how Claude Code,
Codex or any MCP client reaches Arvo.
