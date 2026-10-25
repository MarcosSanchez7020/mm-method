# Adoption guide

From zero to a project that works with the methodology. Steps 2 to 6 can be done by the agent; steps 7 and 8 are GitHub configuration and are done by a person with administration permissions on the repository.

## Before you start

- Claude Code with plugin support (`claude plugin --help` lists the commands).
- Git, and the project repository on GitHub.
- Optional: the GitHub CLI (`gh`), so the agent can open the PRs.
- Optional: a second agent (for example, Codex) if two will work on the project.

## 1. Install the skill

```bash
claude plugin marketplace add MarcosSanchez7020/mm-method
claude plugin install metodologia-agentes@mm-method
```

## 2. Ask the agent to set up the project

In a new session, inside the project repository:

```text
We are going to work this repo with the methodology. Before creating files, give me as a list the decisions you need for the CLAUDE.md: who decides what, the go word, language and tone, who reviews by default, which files require my technical review, which agents and models we use and the CI budget.
```

The agent answers with a numbered list. With your answers it writes the files of steps 3 to 6 and records your answers in the log.

## 3. `CLAUDE.md`

The skeleton is in `references/templates.md` of the skill. The sections that cannot be missing:

| Section | What it contains |
|---|---|
| Source of truth | Which documents rule and where the original technical source is |
| What is not negotiable | The principles of the methodology and the product's own rules |
| The team | Who owns what and who decides what |
| How to work with the team | Go word, decisions as a list, language, reviews, models |
| How each module is worked | The six steps of the cycle, adjusted to the project |
| Memory in the repo | What goes in `MEMORY/` and when it is written |
| Agents | Folders, branches, messages, shared files, PRs (only if there are two) |
| Repository | Protected `main`, technical review, changelog, CI, routine after the merge |

If the project already has a `CLAUDE.md`, the agent proposes where each section fits and gives you as a list what conflicts.

## 4. The memory folder

```bash
mkdir MEMORY
```

With four files: `README.md`, `team_log.md`, `machines.md` and `owner_answers.md`. The skeletons are in `references/templates.md`. Secrets and third-party personal data never go there.

## 5. `CHANGELOG.md`

With one "[Unreleased]" section per branch. Every change, however small, carries a date, the file, what changed and who made it.

## 6. `AGENTS.md`, if there is a second agent

A short file that points to `CLAUDE.md` and to `MEMORY/`, with what is specific to that agent: its folder, its port and how it signs in the changelog.

The second agent's worktree, from the main folder:

```bash
git fetch --prune
git worktree add --no-track -b <agent-branch> .worktrees/<agent> origin/main
```

And `.worktrees/` in the `.gitignore`.

## 7. Protect `main`

On GitHub, as repository administrator:

1. **Settings → Branches** (or **Settings → Rules → Rulesets**): a rule for `main` that requires a PR before merging, requires the CI checks to be green and blocks force-pushes.
2. **Settings → General → Pull Requests:** leave "Allow merge commits" enabled and disable "Allow squash merging" and "Allow rebase merging", so that each merge keeps the branch's commits.
3. **The approval that GitHub authenticates:** a `.github/CODEOWNERS` file assigning the technical files to the owner and, in the rule for `main`, "Require review from Code Owners" and "Dismiss stale pull request approvals when new commits are pushed". The line in the PR description authenticates nobody. GitHub does not let a PR's author approve it: if the agent opens PRs with the owner's account, the agent needs its own account.

## 8. The PR template and the CI

`.github/pull_request_template.md`, with at least:

```markdown
## What changes

## Tests
- [ ] Local suite: <result>
- [ ] CI: <result>

## Technical review
- [ ] Technical review approved by <owner> (date: DD/MM/YYYY, commit: abc1234)
```

Only the owner checks the approval line.

In the CI workflows that run on every PR, this cancels the previous run when a new commit comes in, so you do not pay twice:

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

The list of files that require technical review (for example, `.github/technical-paths.txt`) can be used by a separate check that verifies the approval line and its commit. That check goes in its own workflow, which also listens to `edited`, to be re-evaluated when the owner checks the line and when a new commit comes in; the test suite does not listen to `edited`. The example is in the skill's `references/templates.md`, "Approval line check". The part that limits the check to PRs touching files on the list depends on each repository and the skill does not include it.

## 9. Several computers (optional)

On every computer other than the main one, a local hook marks the commits with the machine name. It is installed once per computer, from the main folder of the clone, because worktrees share the clone's hooks; the worktree agent does not install it. The script resolves the real path of the hook and does not overwrite an existing one:

```sh
hook=$(git rev-parse --git-path hooks/prepare-commit-msg)
if [ -e "$hook" ]; then
  echo "$hook exists: add the git interpret-trailers line below to it by hand."
else
  mkdir -p "$(dirname "$hook")"
  cat > "$hook" <<'EOF'
#!/bin/sh
case "$2" in merge|squash) exit 0 ;; esac
git interpret-trailers --in-place --if-exists doNothing --trailer "Machine: <machine-name>" "$1"
EOF
  chmod +x "$hook"
fi
```

The details (worktrees, `core.hooksPath` and what to do if a hook already exists) are in the skill's `references/templates.md`, "Machine hook". Then `git log --grep "Machine: <machine-name>"` finds what was done from that computer.

## 10. The first work session

```text
Read CLAUDE.md and MEMORY/. Then: git fetch --prune, git worktree list and git status. We are going to plan <the module>: read the original source and give me what it says, what the code does today and the decisions as a list. Do not write code until I say "<word>".
```

## Checklist

- [ ] The skill installed (`claude plugin list`).
- [ ] `CLAUDE.md` with its eight sections and the owner's answers.
- [ ] `MEMORY/` with its four files.
- [ ] `CHANGELOG.md` with "[Unreleased]".
- [ ] `AGENTS.md` and the worktree, if there is a second agent.
- [ ] `main` protected and merge commits only.
- [ ] The PR template with the approval line.
- [ ] `CODEOWNERS` and required code owner review, if the approval has to be authenticated.
- [ ] The CI running on every PR, with cancellation of the previous run.
- [ ] The machine hook on the computers that are not the main one.
