# Scripts and schedules

A project script is Python, run from the editor (Run → Run Script) or on a
schedule, using the `arvo` package to run research through the engine. Its
findings are saved under the author `script:<name>` and shown in the
**Runs** tab, grouped by script, with the size of the search each script's
findings are deflated against.

The **Jobs** panel shows the engine's own jobs (checking findings for
staleness, recording option quotes, probing plugins) and lets you schedule a
script: every *n* minutes, or a cron line in local time. Schedules belong to
the engine, so they run with no window open.

The interpreter comes from `python.defaultInterpreterPath` or the project's
environment; the status bar names it.
