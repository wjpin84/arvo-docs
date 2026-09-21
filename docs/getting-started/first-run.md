# First run

## The window

Arvo opens like an editor: an activity bar on the left (Portfolio,
Research, Files, Search, Accounts, Extensions, Alerts, Settings), tabs in
the middle, a bottom panel (Problems, Output, Terminal, Jobs) and, when you
open it, the Agent panel on the right. Everything is a command: the menus,
the keybindings and the palette (++ctrl+shift+p++) are three views of one
registry, with VS Code's names where VS Code has one.

The **Welcome** tab lists the tabs worth knowing: Rulesets, Risk, Sessions,
Portfolio, Runs, Operations, Agent.

## The project folder

Arvo works in a folder. Choose one on the first run (File → Open Folder);
Arvo remembers it. Everything the engine produces lands there — the bar
library, findings, session records — as files you can read, copy and put
under git. Your own scripts live there too.

## The engine

The window starts an engine for you and shows its state in the status bar.
The engine writes `engine.json` (its address and the research token) and
`control.json` (the control token) to the app data directory, which is how
every other client — a script, an agent, the command line — finds it.

Closing the window does not stop the engine: it minimises to the tray and
keeps serving, so a schedule runs and a session trades with the window
closed. **Quit Arvo** from the tray stops both. **Halt trading** from the
tray is the kill switch.

## Your first study

1. Open **Research** in the activity bar, pick a source (Alpaca, Robinhood or
   Yahoo, whichever you connected) and fetch bars for an instrument.
2. Pick a rule (start with *Moving-average crossover* — it is the control,
   a rule nobody disputes) and run a study.
3. Read the **verdict** and the advice above the numbers. Most first studies
   are `Inconclusive`; that is the platform working. [How to read one →](../how-to/run-a-study.md)

## Data you can try without an account

The library ships with three simulated instruments — `DRIFT.SIM`,
`TREND.SIM` and `NOISE.SIM` — so a study can be run before any vendor is
connected. A rule that "works" on `NOISE.SIM` is the first thing to be
suspicious of.
