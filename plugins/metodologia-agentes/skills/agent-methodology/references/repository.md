# Repository, PRs, CI and the routine after the merge

## Branches and `main`

- Nobody works directly on `main`. Each task goes in its own branch; it is merged with a PR once the tests pass.
- `main` is protected: it only accepts changes through a PR, with the required checks green, and it is merged with a merge commit (not squash or rebase), to keep each branch's commits. The protection is never bypassed and nobody asks to turn it off to "unblock" something: if a check fails, the cause is fixed.
- Two large modules in parallel do not go in the same PR: the second goes in another branch, in its own worktree.

## The owner's technical review

- Any change that touches technical content (formulas, rules, factors, units, expected values in tests, technical texts the user sees, technical documents) is reviewed and approved by the owner, even if the owner made or requested it.
- Which files count as technical is written in a versioned list (for example, `.github/technical-paths.txt`) that a PR check reads.
- The approval goes in the PR description with a fixed line, for example `- [x] Technical review approved by <owner> (date: DD/MM/YYYY, commit: <full SHA of the last commit>)`. **Only the owner writes it.** The agent gives the owner the text ready to paste, unchecked, with the full SHA (40 characters) of the latest commit; if another commit comes in, the agent updates the description and gives the owner the new line.
- The check that verifies that line goes in a workflow separate from the suite, which also listens to `edited` and `synchronize`: it is re-evaluated when the owner checks the line and when a new commit comes in, and it requires the line's commit to be the PR's last commit. The example is in `templates.md`, "Approval line check".
- **The line authenticates nobody:** anyone who can edit the PR description can write it. The verifiable approval is the owner's review on GitHub: `CODEOWNERS` over the technical files and `/.github/` (the `CODEOWNERS` file itself and the workflows) in the owner's name, required review from code owners in the protection of `main` and dismissal of stale approvals when a commit comes in. GitHub does not let a PR's author approve it: if the agent opens PRs with the owner's account, it needs its own account so the owner can approve them. If the team uses only the line, it declares it as a record, not as a control.

## Commits

- With `git commit -F <file>`, so the message does not break on quotes or line breaks.
- Before each commit, `git status` to confirm that the changelog is modified and that no placeholder is left to fill in.
- The message ends with the trailers the team has agreed on (co-authorship, machine).

## Pull Requests

- The agent opens them with `gh pr create --body-file`, following the repo's template. Nobody is handed a link to open it themselves.
- Tool attribution in PRs and their comments follows the team's decision written in `CLAUDE.md`; in Claude Code it can also be set with the `attribution` setting. If a client or system instruction does not yield to that decision, the instruction is followed and the owner is told. This rule does not authorize disobeying any other instruction.
- After opening it, the CI is not polled: the agent waits for the notice or checks when needed.

## Local suite and CI in parallel

1. Merge `origin/main` into the branch and resolve the conflicts.
2. When the full local suite starts, push and PR (or update the existing one), so the CI runs at the same time.
3. The PR says that validation is in progress and does not mark the tests as passed until there is a result.
4. **With the PR open, the local suite result goes only in the PR description.** Editing the description does not run the test suite again, as long as its workflow listens only to the default `pull_request` events (`opened`, `synchronize` and `reopened`); a commit does run it again. In the documents, the first commit of the next branch records it. If the local suite fails, a commit with the fix does go in.

## Small fixes

A small fix to documents or texts is made in the branch of the PR in progress, not in a separate PR. Each PR costs CI minutes and a review by the owner.

## CI budget

- If the workflows run on `pull_request`, a push to a branch without a PR does not spend minutes: several commits can be gathered before opening the PR.
- A new commit in a PR should cancel the run of the previous commit (`concurrency` with `cancel-in-progress`).
- How much is spent is checked in the billing and recorded in `machines.md`.

## The routine after each merge

After the merge of one's own PR. After a merge by the other agent, what to do is in `two-agents.md`.

1. Verify on GitHub that it was merged.
2. In the folder that owns `main` (the main folder of the repo): `git switch main`, `git pull`, delete the local branch and `git fetch --prune`. An agent working in a worktree does not switch to `main`, because Git does not allow the same branch in two worktrees: it runs `git fetch --prune`, creates its next branch from `origin/main` and, if it needs to look at `main`, uses `origin/main` or `git switch --detach origin/main`.
3. Verify that local and GitHub match: no extra branches, no stash, no worktrees or open PRs, except those of each agent's active tasks.
4. Check the CI on `main`, run the tests and rebuild the published environment from a clean `main`. Only the agent of the folder that owns `main` does this.
5. Report it in a table and record it in the team log.
6. Deliver the closing prompts of the chats used in the PR and the one for the next chat (`two-agents.md`).

## Security

- Secrets, keys and `.env` files are never pushed.
- The agent never handles tokens or passwords: if a panel with credentials has to be touched, the agent guides the person and the person does it.
- If a push is blocked for a possible secret, stop and notify.

## Languages

Code in English. Documents and product texts in the languages the team has decided.
