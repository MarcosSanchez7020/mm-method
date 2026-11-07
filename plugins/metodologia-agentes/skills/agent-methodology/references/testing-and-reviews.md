# Independent tests, mutation and reviews

They are three different things and they do not replace one another. The tests say whether the code gives what it should. Mutation says whether the tests detect errors. The reviews look for what nobody thought of testing.

## Independent tests

- Each testing agent writes in its own new file, without seeing or copying the other agents' tests.
- The expected values are worked out by hand, from the technical source, from the documents or from the books, with their reference. They are never copied from the code under test: a test that copies the code's result only checks that the code gives what it gives.
- Each one with its own oracle, an independent way of computing the expected result.
- If a test finds an implementation error, it is marked strict `xfail` (for example, `strict=True` in pytest) with the reason, so that it fails as soon as the error is fixed and someone reviews it.
- When a change must not alter results, the output of `main` is compared field by field against the new one over thousands of cases.

## Mutation

- Always on a copy of the commit, for example with `git archive` into a temporary folder; never in the repository.
- The runner separates a failed assertion from an exception and from a collection error, with a JUnit report. The documents say what counts as "detected": generally, only the failed assertion.
- A surviving mutant is classified before concluding anything:
  - **a test is missing**, or a test does not look at what it should: the test is added or fixed;
  - **equivalent**: the change does not alter any observable behavior, for example in code that no path reaches or in a transformation that gives the same result for every valid input. It is declared equivalent only with written evidence: why no valid input distinguishes it, with a reference to the code. Without that evidence, it counts as a missing test.
- Declared equivalents are listed separately in the mutation report, with their evidence, and count neither as detected nor as pending.

## The lab is not a test

The test page calls the core's public functions and never reimplements its rules. If the lab computes something on its own, sooner or later it drifts out of line with the core and shows results the product does not give.

## Adversarial reviews

- **Different lenses**, one per reviewer, for example:
  - correctness against the technical source;
  - numerical, data and concurrency robustness;
  - conventions and security.
- **One verifier per finding**, who tries to refute it. Only what survives is fixed.
- **What another agent brings is one more finding:** it goes through the verifier and is never applied directly.
- **Nobody reviews their own work.** The reviews of what an agent did (texts, designs and code) and the verification of its findings are done by the other agent, even if an earlier decision says otherwise. That option is not offered to the owner: when the work is the asking agent's, the question of who reviews is already answered. The rest of who reviews (for example, a person's work, or which reviews are done in a phase) is asked of the owner before each review, unless the team has written a rule in `CLAUDE.md` for a phase.
- **What counts as a review:** text reviewers, code reviewers and the verifiers of each finding. Independent tests, the lab, mutation and mechanical validators (those that check with real commands and counts, `orchestration.md`) are not reviews: they continue even when there is a reviewer and do not replace one.
- **The reviewer** works in a new read-only chat: it reads, verifies each finding itself and does not change files, branches or configuration. It leaves its report in its chat and stops; if the project has a hub, it does not report to the hub: the owner pastes the result wherever it is needed (`hub.md`).
- **In rounds, until "no findings".** After fixing, a re-check prompt with the reviewer's numbering, the HEAD, the exact diff since the previous round and what was not run. The PR, and the push that opens it, only with "no findings" (`orchestration.md`).
- **Independence:** having the other agent review work that it did itself earlier (for example, the design it now codes) is not fully independent; if the owner chooses it for cost, the owner is told and it is noted. With a single agent in the project, a new read-only chat of that agent reviews, without the memory of the chat that did the work, and it is noted that it is not independent.

## Format of a finding

```
[P1|P2|P3] <file>:<line>: <what is wrong, in one sentence>
Evidence: <the quote from the code, the document or the source>
Impact: <what happens to the user or to the result>
Verification: <confirmed | refuted, with the reason>
```

The priorities: P1, incorrect technical result or data loss; P2, broken contract or badly handled edge case; P3, wording, convention or clarity.
