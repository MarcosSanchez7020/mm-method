# Two agents in the same repository

When two agents work together, for example Claude Code and Codex, the risk is not technical but one of coordination: one steps on the other's work, one assumes what the other did, or the rules of one chat never reach the other. These rules apply to both agents and to any new chat of either one. They are written in `CLAUDE.md` (and `AGENTS.md` points there).

## Folders and branches

1. **Each agent in its own folder.** One in the main folder of the repo; the other in a Git worktree (`.worktrees/<agent>`). Only the main folder has `main`: the other agent never switches to that branch. Neither opens, edits, switches branches in, or runs commands that write to the other's folder, nor deletes the other's worktree or branches.
2. **Each task belongs to one agent, in its own branch.** The owner assigns each task. Tasks that touch the same module go in series; only tasks in different modules go in parallel.
3. **Each branch is created from an up-to-date `main`:**

   ```bash
   git fetch --prune
   git switch --no-track -c <branch> origin/main
   git push -u origin <branch>   # the first push
   ```

   Without `--no-track`, the branch ends up tracking `main` and a wrong push can go where it should not.
4. **Exception: one designs and the other programs.** The branch of the one who programs is created from the branch of the one who designed (not from `main`), to bring in the design. The one who designed stops writing to their branch as soon as the other takes it. The one who programs opens the PR, which carries the commits of both. Each exception is written in `CLAUDE.md`.

## Communication

- **The agents do not talk to each other directly: the owner passes the messages.** If one needs something from the other, it writes the message ready to paste, in a code block. The only exception is the read-only reviewer.
- **Neither assumes what the other did:** it verifies in Git and in the memory.
- **Decisions belong only to the owner.** Each agent passes its own to the owner and records the answers in `owner_answers.md` on its branch. If a decision affects the other agent's task, it says so and prepares the message for the owner.
- **A task can be added to an agent that is already working without interrupting it**, with a "steer" message that adds the new work to what it is already doing.

## Shared files

- The changelog, the memory and the documents are touched by both branches.
- Before opening a PR: `git fetch` and merge `origin/main` into the branch (merge, never rebase or force-push). Conflicts are resolved by keeping both sides, never by deleting the other's work, and the full suite is run after merging.
- The title of the changelog section of a merged PR is set by the next branch of the agent that opened that PR. The other agent leaves it as it is.

## Servers and published environment

- Each agent has its own port to test its branch and shuts its server down when finished.
- The published environment is built from `main` and is rebuilt by a single agent, in its routine after each merge.

## PR and merge

- Each agent opens its own PR and gives the owner the technical approval line with its last commit. The owner writes it and does the merge. Neither touches the other's PR.
- After a merge by the other agent, neither abandons its active task:
  - the agent of the main folder verifies the merge, runs `git fetch --prune` and checks the CI of `main`; the switch to `main` and the rebuild of the environment wait until its folder is at a commit;
  - the agent with a task in progress keeps its branch, runs `git fetch --prune` and merges `origin/main` into it (merge, never rebase) before continuing.
- After the merge of its own PR, each agent creates its next branch from `origin/main` when it is assigned the next task. The worktree agent never switches to `main` (Git does not allow the same branch in two worktrees); if it needs to look at `main`, it uses `origin/main` or `git switch --detach origin/main`.
- After each PR in which the other agent's chats were used, the main agent gives the owner, complete and in code blocks, the closing prompt of each chat that was used and the prompt of the next chat. The next-chat prompt is self-contained and starts by reading `AGENTS.md`, `CLAUDE.md` and the memory.

## Models by function

- Planning, designing, coordinating and judging: the most capable model.
- Executing (reading, programming bounded tasks, independent tests, mutation): a cheaper model, but a sufficient one.
- **Executors do not decide.** They receive exact contracts: files, signatures, behavior case by case, which function to call, the test cases with their expected value, and when the task is finished. Anything that calls for judgment goes back to the coordinator or to the owner.
- If an execution task is delicate, the coordinator can raise the model and says so.
