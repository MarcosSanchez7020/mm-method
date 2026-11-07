# Orchestrator, thinking workers and executors by contract

Once the agreed word has been given, the main chat does not do the work: it orchestrates it. A cheap orchestrator coordinates and judges at the gates; the thinking workers use the strong model at high effort; the executors use an economical but capable model and decide nothing. Which model is which is written in `CLAUDE.md`.

## Who does what

- **The orchestrator** (the main chat) only coordinates and judges at the gates between phases. It does not write code, does not run suites by hand (only short git and status commands) and does not read agent transcripts: it reads each one's summary in its schema. It reads usage (context and weekly) at every phase change (`costs.md`). It can be a cheap model: whatever calls for thinking, intermediate work included, goes to a thinking worker.
- **The thinking workers:** diagnosis, design, refutation, synthesis and verification by hand. Strong model, high effort.
- **The executors:** they code, write tests, run mutation, write documents. Economical model, enough to write well and follow clear instructions. They receive exact contracts (files, signatures, behavior case by case, which function to call, the test cases with their expected value and when it is finished) and decide nothing: whatever calls for judgment goes back to the orchestrator, who passes it to a thinking worker or to the owner.
- If an execution task is delicate, the orchestrator raises the model and says so.

## The phases

- **A. Documents.** What the authorized change touches in the documents (a thinking worker). What was decided for the module was already written and reviewed before the agreed word, in step 2 of `cycle.md`.
- **B. Design of the change.** A designer writes the exact contract; a refuter tries to bring it down against the technical source and the written decisions. Both with the strong model at high effort. No refuter only if the piece is simple and the owner decides so.
- **C. Execution.** Executors by contract, on disjoint files, two attempts each (the second picks up what the first did without rewriting); independent tests with the by-hand values from the contract; mutation on a copy.
- **D. Verification.** A thinking worker verifies by hand through the real chain; an independent validator, with real commands and counts, who for every code fix shows by mutation on a copy that the new tests detect the cause; and the lab. The validators do not count as a review (`testing-and-reviews.md`).
- **E. Review.** By the other agent, in rounds until "no findings".
- **F. Closing.** The full suite once, on the final commit, in parallel with the push and the PR (`repository.md`).

Phases A to D chain without stopping, with the orchestrator's gates. It stops before E only for something against the technical source, for a refuter discrepancy that the written decisions do not close, or for an executor that does not finish in two attempts; that goes to the owner, in a list.

## The common script of the workflows

Each phase is a workflow with a standard script:

- common rules for all agents: exact folder and branch; no git that writes (the commits are made by the orchestrator); the project's interpreter and environment; expected values never copied from the code; no `xfail`, `skip` or normalizations that hide a failure; bounded writes (for example, at most 150 lines per write); line endings respected; the formatter only on what was touched; output only in the schema;
- one output schema per agent, under 25 lines;
- two attempts per executor.

## The orchestrator's commits

- The code and documents of a fix go in one commit. The decisions and documentary clarifications go in separate commits. A merge of `main`, only with the conflict resolution and the adjustments it requires.
- Each commit with the changelog, the memory and the map up to date (`plan-map.md`).

## Review in rounds

Finding → fix workflow → commit → re-check prompt, in one go, with the reviewer's numbering, the HEAD, the exact diff and what was not run. The PR, and the push that opens it, only with "no findings", and then the full local suite in parallel with CI.

## Before the agreed word

For whatever is not coding (planning, taking inventory, reading documents, drawing up decision lists), the main chat reads and consolidates by itself, with few agents (`costs.md`, "Workflow size").
