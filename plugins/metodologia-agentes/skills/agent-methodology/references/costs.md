# Context, usage and workflow size

The context of a chat and the weekly usage of an account are finite. A long project spends them without anyone noticing, and when they run out, the work is left half done.

## Context

- The results of a workflow or of many agents are not read in full: they are summarized with a scratchpad script that prints only what is needed.
- Agents are asked for short reports (fewer than 25 lines) with a fixed schema.
- Text corrections spread across many files are delegated to an agent with exact instructions.
- Files that have already been read in full are not read again.
- If the context is filling up, the agent says so and closes with a prompt for a new chat, with the memory up to date (`memory.md`).

## Workflow size

- **In tasks that are not programming** (planning, taking inventory, reading documents, building lists of decisions), the agent reads and consolidates by itself. If an independent review is needed, it uses few agents (2 to 4) and verifies only what is delicate. Before launching more than 6, it says how many and for what, and waits for a yes.
- **When it comes to programming**, once the owner has said the word, the agent launches the agents and workflows it needs: independent tests, mutation, lab page. Reviews follow the rule of asking who does them.
- Agents are not launched for topics the owner did not ask for: ask first.

## Weekly usage

- When the account is close to its limit, the agent says so and proposes options: hand the design or the reviews to the other agent, work without subagents, or wait for the reset.
- Before it runs out, everything has its note in the team log and, if the owner asked for it or the project agreed on it in `CLAUDE.md`, is committed.
- If the account is shared between people, they are warned before many agents are launched, because the limit is everyone's.

## Models

The most capable model for planning, designing and judging; a cheaper one for executing closed contracts (`two-agents.md`). An expensive executor doing mechanical work spends the usage that is later missing for deciding well.
