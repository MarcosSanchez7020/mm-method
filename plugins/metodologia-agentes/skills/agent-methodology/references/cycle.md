# The cycle of each module

A module is a piece the owner decides on as a unit: a check, a rule, a product feature. Each one goes through these six steps, in order.

## 1. Plan

- Read the technical source in its original, to the letter (the standard's PDF, the specification), not from memory or from a summary. If the source cites another one, it is cited as a reference and marked as our interpretation.
- Give the owner, in a single message:
  - what the source says, with the reference;
  - what the product does today;
  - which cases it applies to;
  - how much the decision weighs; if useful, a sweep with the real code, with cases the agent chooses and that are informational;
  - the decisions, as a list (`decisions.md`);
  - separately, the interpretations: edge cases, what is compared with what, what happens if there is no data.

## 2. Write down what was decided

- The owner decides. The agent writes the decisions into the documents, in the owner's words; what the agent clarifies goes separately, to be confirmed.
- A short review of those texts: two reviewers who are given the answer log. Who reviews is asked of the owner beforehand, unless there is a written rule (`testing-and-reviews.md`).
- The calculations in the examples that end up written down are checked.
- The agent confirms to the owner what it understood. The commit, only when the owner asks for it.

## 3. Program, only with the owner's word

- The agent writes the core and checks it by hand: the numbers in the documents and in the books, and one complete case through the real chain.
- Then it launches, without asking, because programming is already authorized:
  - independent testing agents, each in its own new file;
  - mutation on a copy;
  - an agent for the test page (the lab page).
- Adversarial reviews, on the other hand, are asked about (`testing-and-reviews.md`).
- The agents format only their own files and do not leave temporary configuration files behind.

## 4. The lab page, as soon as it exists

- A local page that calls the core's public functions, so the owner sees real cases without waiting for the reviewers.
- It is served on the agent's port, the local link is given to the owner, and all the examples are run in the browser.
- The published environment (the one others see) is built only from `main`.

## 5. Fix and close

- The confirmed findings are fixed.
- If an independent test contradicts a fix, the written decision wins, and the technical source wins over everything. If the decision says nothing, the test is adjusted without weakening it and that is stated. Whatever is the owner's decision is asked of the owner, as a list.
- If a test compares the text of a message, the format is part of the decision.
- Before removing an `xfail` that turned into XPASS, check that the test really tests something.
- The documents move to "programmed", a short review of only what changed, and mutation repeated on the final code.

## 6. Push and verify in parallel

1. Changelog and README up to date; commit.
2. Merge `origin/main` into the branch (merge, never rebase) and resolve the conflicts keeping both sides.
3. When the full local suite starts, push and PR, so the local suite and the CI run at the same time.
4. The PR declares the validation in progress and does not mark the tests as passed until there is a result. Afterwards its description is updated with both results (`repository.md`).
5. The technical approval and the merge belong only to the owner.

## Why this order

Planning before programming avoids writing code for a rule the owner was going to decide differently. Writing down what was decided before programming leaves a source of truth to test against. And the independent tests, the mutation and the reviews attack the code from three different sides: what it should give, whether the tests detect errors, and whether someone who did not write it finds faults.
