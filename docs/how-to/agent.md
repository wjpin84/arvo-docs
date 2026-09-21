# Ask the agent

1. View → Agent, or ++ctrl+alt+i++. The first launch fetches the Claude
   Code adapter with `npx`; after that it starts in a second. The header
   names the agent and the project it works in.
2. Type in words. *Write a ruleset named fast_cross for sma_cross with fast
   5 and 10 and slow 20 and 30, then run a study on MSFT.RH and tell me the
   verdict.*
3. Answer the permission prompts. Each Arvo tool asks once per session
   (*Yes, and don't ask again for Run Study*).
4. Read the cards: the ruleset it wrote is a link into the editor; the
   finding is a link into Runs and appears there under *agent chat*.

The agent can also run commands in the project (*run the tests*), behind
the same prompts, with the output under the card. It cannot fetch data,
touch a credential or trade: those calls do not exist in what it is given.

To use a different agent, set `agent.command` to any Agent Client Protocol
agent's command line. **Restart** in the panel header starts it again.
