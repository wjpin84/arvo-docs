# Settings keys

`settings.json` in the app data directory (user), `.arvo/settings.json` in
the project (project, on top). VS Code's names where VS Code has one.

| Key | What |
|---|---|
| `workbench.colorTheme` | the theme, shipped or from an extension |
| `editor.fontSize`, `editor.tabSize`, `editor.minimap.enabled` | the editor |
| `files.autoSaveDelay` | hot-exit backups |
| `python.defaultInterpreterPath` | the interpreter scripts run with; the project's environment otherwise |
| `agent.command` | the Agent Client Protocol agent the Agent panel launches, as one command line. Default: `npx --yes @agentclientprotocol/claude-agent-acp` |
