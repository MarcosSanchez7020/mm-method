# Context, usage, thresholds and token economy

The context of a chat and the weekly usage of an account are finite. A long project spends them without anyone noticing, and when they run out, the work is left half done.

## Token economy

- **The cost of a chat is its context times its turns.** Each turn pays again for everything the chat has read so far; for that reason one long chat with many short turns costs more than several short chats.
- **One chat per block** of the plan map. When the block ends, the chat closes with its prompt for the next one (`memory.md`).
- **No acknowledgements and no step reports.** A turn only when it adds something: a decision, a milestone, a closing. The hub and the owner verify at milestones, not at every step (`hub.md`); what each chat verifies on its own does not change.
- The results of a workflow or of many agents are not read in full: they are summarized with a scratchpad script that prints only what is needed, or each agent returns a summary with a schema of fewer than 25 lines. The orchestrator does not read transcripts (`orchestration.md`).
- Text corrections spread across many files are delegated to an agent with exact instructions.
- Whole files that have already been read are not read again.
- Without lowering quality: the economy cuts turns and reads, not verifications.

## Thresholds

`CLAUDE.md` sets, for the project, a warning threshold and a closing threshold for each of these three values: the context of the chat that orchestrates, the weekly usage of the expensive model and the weekly usage of all models. It is enough for one of them to come close or be reached.

- **When approaching** the warning threshold, the agent says so.
- **When reaching** the closing threshold, everything is closed without deleting anything: memory, map and changelog up to date, committed if the owner asked for it or the project agreed on it. For context, the owner is given a prompt to continue in a new chat. For weekly usage, the owner is told what is left and how much is estimated to be spent on it, and the owner decides whether to continue.
- They are read at every change of phase and when closing every milestone. An agent that watches the values while waiting spends tokens on the wait; if an automatic warning is wanted, it is a script without a model, or the client's monitor.

## Workflow size

- **In tasks that are not programming** (planning, taking inventory, reading documents, building lists of decisions), the agent reads and consolidates by itself. If an independent review is needed, it uses few agents (2 to 4) and verifies only what is delicate. Before launching more than 6, it says how many and for what, and waits for a yes.
- **When it comes to programming**, once the owner's explicit "go" is given, the agent launches the agents and workflows it needs: independent tests, mutation, lab, validators (`orchestration.md`). Reviews are done by the other agent (`testing-and-reviews.md`).
- Agents are not launched for topics the owner did not ask for: ask first.

## Weekly usage

- When the account is close to its limit, the agent says so and proposes options: hand the design or the reviews to the other agent, work without subagents, or wait for the reset.
- Before it runs out, everything has its note in the team log and, if the owner asked for it or the project agreed on it in `CLAUDE.md`, is committed.
- If the account is shared between people, they are warned before many agents are launched, because the limit is everyone's.

## Models

A cheap orchestrator that coordinates and judges; the strong model, with high effort, for designing, refuting and verifying; an economical one for executing closed contracts (`orchestration.md`). An expensive executor doing mechanical work spends the usage that is later missing for deciding well.
