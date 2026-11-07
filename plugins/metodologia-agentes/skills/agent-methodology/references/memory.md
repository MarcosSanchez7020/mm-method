# Memory lives in the repository

An agent's local memory stays on one computer. The repository travels with Git to all machines, to the cloud and to the other agent. That is why everything needed to keep working is written in the repo.

## What goes where

| File | What it holds |
|---|---|
| `CLAUDE.md` | The team's lasting rules and preferences. It loads by itself in every session. |
| `AGENTS.md` | For agents that are not Claude Code: it points to `CLAUDE.md` and to the memory, with the rules specific to those agents. |
| `MEMORY/README.md` | What is in the folder and how it is used. |
| `MEMORY/team_log.md` | Per machine and by date: every commit, push, PR, branch, state change and the work left uncommitted. |
| `MEMORY/machines.md` | How each machine is set up: versions, paths, ports and the tricks that are needed. |
| `MEMORY/owner_answers.md` | The owner's answers, verbatim (`decisions.md`). |
| `CHANGELOG.md` | Every change, however small. |
| The plan map | The plan by blocks, with checkboxes that are ticked in the same commit that does each thing, and its generated drawing (`plan-map.md`). |
| The source-of-truth documents | Scope and decisions, requirements, data model, diagrams. |

Secrets, keys, passwords, tokens, `.env` files and third parties' personal data never go in the repo.

## Rules

- **Write it down at the moment**: each owner answer to the verbatim record, each commit, push or PR to the team log, what is new about a machine to the machines file. At the latest, before each commit.
- **Memory travels with each branch.** At the start, the agent runs `git fetch` and reads the memory of `main` and of its own branch. What happens after a branch's last commit (the push, the PR, the merge) is recorded in the next branch.
- **Local memory only points to the repo.** If a computer has something in its local memory that is not in the repo, in the first session on that machine it is shown to the owner and only what belongs to this project is moved to the repo, without secrets or personal data and without treating anything as confirmed that is not.
- **Memory tools are support.** If a memory plugin conflicts with what is written in the repo, the repo wins.

## Optional complement: claude-mem

claude-mem is a third-party plugin (`thedotmack/claude-mem`, on GitHub) that stores observations of the sessions on each computer and gives them back to the agent at the start. The methodology does not need it: using it is each user's decision. If it is used, these adjustments keep it from clashing with the methodology:

- **The repo rules.** What claude-mem remembers is support; if it clashes with `CLAUDE.md`, the repo memory or the documents, the repo wins.
- **It does not replace the record.** The owner's decisions are quoted from the repo's verbatim record, not from a recollection.
- **Independent reviews.** A reviewer chat that should not carry what is already known is opened, if the owner decides so, with the plugin disabled (`claude plugin disable claude-mem@thedotmack`) and it is enabled again afterwards (`claude plugin enable claude-mem@thedotmack`).
- **Weekly usage.** Its settings (`~/.claude-mem/settings.json`) include weekly usage thresholds (`CLAUDE_MEM_QUOTA_THRESHOLD_SEVEN_DAY` and `CLAUDE_MEM_QUOTA_THRESHOLD_SEVEN_DAY_OPUS`) above which it pauses its capture; adjust them to the margin you want to keep.
- **Data: where it is stored and where it goes.** It stores what happens in the sessions on the computer (in `~/.claude-mem`) and, according to its own documentation, sends the observations to the AI provider configured for compressing them (Claude, OpenRouter or Gemini); a local installation also contacts cmem.ai once, at signup. Before enabling it, review which provider the data goes to and how it handles it, and do not use it in sessions that read secrets or personal data that should not leave the computer.

If you decide to use it: `claude plugin marketplace add thedotmack/claude-mem` and `claude plugin install claude-mem@thedotmack`. Its requirements and documentation are in its repository.

## The changelog

- Each entry: date, file, what changed and who ("(Person, Agent)").
- It goes in the "[Unreleased]" section of the branch until it is pushed.
- A commit is never made without its changelog entry.
- When the PR is merged, the next branch of the agent that opened it gives the section its title: "DD/MM/YYYY — <topic> (`first commit` to `last`, merged into `main` with PR #N, `merge commit`)".

## Several people and several machines

- **Who is working:** the computer's Git name (`git config user.name`). If it is not clear, for example in a cloud session, ask before recording or committing. If the agent's account is shared, the person and the machine count, not the account.
- **From which machine:** what is done from a computer other than the main one is marked with its short name:
  - in commits, a `Machine: <name>` trailer, added by a local `prepare-commit-msg` hook that is not pushed (the hook is in `templates.md`, "Machine hook");
  - in the changelog, "(Person, Agent; <machine>)";
  - in PRs, "Made from: <machine>".
- On a new machine, the agent asks the owner whether to install the hook and with what short name, shows the script first and installs it only if the owner says yes.
- Each machine has its own section in the team log and in the machines file.

## The daily check of versions and health

With several computers and several people, a different version on one machine breaks what works on another, and a tool that updates itself can go down without anyone noticing. That is why, at the start of the first chat of the day on each computer, the agent checks:

- the versions: the agent's client and its plugins, the other agent (its app and its CLI), the project's runtime and the dependencies installed in each worktree (the virtual environment in Python, `node_modules` in Node) against the pinned dependencies file, and the tools the project uses (for example, Docker, `gh`, `git`, Node);
- the health: the local services (the memory plugin, the lab, the database), and if one went down, how to bring it up;
- the repository: the CI of `main` and the open alerts (code, secrets, dependencies).

If something changed, it records it with the date in the machine's entry in the machines file, and in the team log that the check was done. A tool that updates itself is pinned to a version when the installer allows it; if not, it is recorded as a risk.

## When a chat is closed

Because of a usage limit, a full context, a threshold (`costs.md`) or because the task or the block is finished:

1. Memory and plan map up to date on the branch, with its changelog entry.
2. Commit and push, if the owner asked for it or the project agreed on it in `CLAUDE.md`.
3. A prompt for the next chat, in a code block, that says which model and effort it is opened with, starts by reading `CLAUDE.md`, the memory and the plan map, and says where the work stands.
4. If the project has a hub, the closing report with its schema (`hub.md`).

Uncommitted work is never left without noting it in the team log.
