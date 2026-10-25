# Skeletons for adopting the methodology

What goes between `< >` is filled in by the team. None of this is filled with invented data: if a fact is not known, the owner is asked.

## `CLAUDE.md`

```markdown
# <Project name>

<What the product is, in two lines. Who the team is.>

## Source of truth
- `<scope and decisions document>`: scope, decisions, pending items.
- `<technical documents>`: requirements, data model, diagrams.
- `<original technical source>`: the standard or specification, in its original file.
- `CHANGELOG.md`: every change.
- `MEMORY/`: log per machine, setup of each machine and verbatim record of the owner's answers. Read at the start of every session.
- `AGENTS.md`: for agents that are not Claude Code.

## Non-negotiable
- The technical source takes precedence over any decision (<owner>, <date>).
- Assumptions and limitations in plain view, outward and inward.
- <Product-specific rules.>
- No filler.

## The team
- <Person>: owner of <area>. Decides <what>.
- <Person>: <area>. Decides <what>.

## How to work with the team
- Language and tone: <…>.
- First plan, then code. The word that authorizes coding: "<word>".
- Decisions in a numbered list, with lettered options and the recommended one in bold.
- Prompts go in the chat, in code blocks.
- Reviews: <who by default; ask before each one>.
- Models: <which one for planning, which one for execution>.

## How each module is worked
<The six steps of the cycle, adjusted to the project.>

## Lasting rules: always here
Any team rule that must always hold is written in this file, with its changelog entry, in the branch being worked on.

## Memory in the repo
<What goes in `MEMORY/` and when it is recorded.>

## Agents: how the two work together
<Folders, branches, messages through the owner, shared files, ports, PR and merge, reviews. The exceptions, with date and the owner's answer.>

## Repository
- Never directly on `main`; `main` protected; merge, not squash.
- Owner's technical review: files in `<list>`; only the owner writes the approval line.
- Changelog for everything; commits with `git commit -F`.
- Local suite and CI in parallel; the local result goes in the PR description.
- The routine after each merge.
```

## `MEMORY/README.md`

```markdown
# Project memory (in the repo)

- `team_log.md`: what was done from each machine, by date.
- `machines.md`: how each machine is set up.
- `owner_answers.md`: the owner's answers, verbatim.

The rules are in `CLAUDE.md`. No secrets or third parties' personal data go here.
```

## Answer record entry

```markdown
### <DD/MM/YYYY> — <Topic> (<owner>, <agent>)

<Context in one or two lines.> The agent gave them, with all their options (the recommended one in bold):
1. <question>: **a) <recommended>**; b) <…>.

Taken as decided: <…>. <Owner>:

> <their words, verbatim>

(<How it is read. What they did not say.>)
```

## Team log entry

```markdown
- <DD/MM/YYYY> (<agent>, session `<id>`): COMMIT `<hash>` (<what>) and PUSH of `<branch>`, <with or without a PR>. <Whatever else happened.> <What remains uncommitted, if anything.>
```

## Changelog section

```markdown
## [Unreleased] — <DD/MM/YYYY>: <topic> (branch `<branch>`)

### Decided
- **<Decision>** (<owner>, <date>: "<answer>"): <what remained>. (<owner>, with <agent>'s proposal)

### Changed
- `<file>`: <what changed>. (<person>, <agent>)
```

## Approval line in the PR template

```markdown
- [ ] Technical review approved by <owner> (date: DD/MM/YYYY, commit: abc1234)
```

## Prompt for the next chat

```text
New <agent> chat in <folder>, <machine>.

BEFORE ANYTHING: read CLAUDE.md and MEMORY/ from origin/main and from origin/<branch>. Then: git fetch --prune, git worktree list and git status.

WHERE IT STOPPED
- <What was done, with commits and PR.>
- <What is in progress and in which branch.>
- <What is running in another agent.>

WHAT COMES NEXT
1. <Step.>
2. <Step.>

PENDING
- <What is open, with its source.>
```


## Approval line check

A workflow separate from the test suite. It also listens to `edited`, to be re-evaluated when the owner checks the line, and to `synchronize`, to require a new approval when another commit comes in. The suite does not listen to `edited`, so editing the description does not run it again.

```yaml
name: Technical review
on:
  pull_request:
    types: [opened, edited, synchronize, reopened]
permissions:
  contents: read
jobs:
  approval:
    runs-on: ubuntu-latest
    steps:
      - name: Check the approval line and its commit
        shell: bash
        env:
          BODY: ${{ github.event.pull_request.body }}
          HEAD_SHA: ${{ github.event.pull_request.head.sha }}
        run: |
          pattern='^- \[x\] Technical review approved by [^()]+ \(date: [0-9]{2}/[0-9]{2}/[0-9]{4}, commit: ([0-9a-f]{7,40})\)$'
          lines=$(printf '%s\n' "$BODY" | tr -d '\r' | grep -E "$pattern" || true)
          count=$(printf '%s' "$lines" | grep -c . || true)
          if [ "$count" -ne 1 ]; then echo "Exactly one checked, well-formed approval line is expected; found $count."; exit 1; fi
          [[ $lines =~ $pattern ]]
          sha=${BASH_REMATCH[1]}
          case "$HEAD_SHA" in
            "$sha"*) echo "Valid format, approved on $sha." ;;
            *) echo "The approval is for $sha and the last commit is $HEAD_SHA."; exit 1 ;;
          esac
```

- **This check authenticates nobody.** It checks the line's format and that its commit is the PR's last commit, but anyone who can edit the description, the PR author included, can write it. The approval that GitHub does verify is a review: a `.github/CODEOWNERS` file assigning the technical files to the owner and, in the protection of `main`, required review from code owners and dismissal of approvals when a new commit comes in (`repository.md`). If the team keeps only the line, it declares it as a record, not as access control.
- It accepts exactly one checked, well-formed line, matched whole from start to end (also with CRLF line endings), and takes the commit from that same match. Text added after the line, another checked line or a name with parentheses make it fail.
- The description comes in through an environment variable and not inside the script, so that text in the PR cannot run commands.
- As written, it applies to every PR. Limiting it to the PRs that touch files on the technical list requires querying the PR's files in the GitHub API; that part depends on each repository and is not included here.
- Tested in a terminal with `bash --noprofile --norc -eo pipefail`, the way GitHub Actions runs a step with `shell: bash`, in ten cases: unchecked, the last commit abbreviated and full, another commit, text added with another commit after the line, a name with parentheses, two checked lines, CRLF line endings, an injection attempt and an empty description. The complete workflow has not been run on GitHub.
## Machine hook

It marks the commits made from a computer other than the main one. Git hooks belong to the whole clone: the main folder and its worktrees share them. So it is installed once per computer, from the main folder of the clone, by the person or the agent of that folder, never by the worktree agent, which does not write in the other's folder.

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

- `git rev-parse --git-path` gives the real path of the hook. In a worktree, `.git` is a file and not a folder, so `.git/hooks/` does not work as a fixed path.
- If the clone uses `core.hooksPath`, the path comes from that setting. If it is relative, Git resolves it in the working folder where the hook runs, so each worktree looks for its own copy; in that case, version the hook in that folder or use an absolute path.
- If a `prepare-commit-msg` hook already exists, it is not replaced: the `git interpret-trailers` line is added to it.
- The hook adds `Machine: <machine-name>` as a commit trailer, and `git log --grep "Machine: <machine-name>"` finds what was done from that computer.
- Tested with Git 2.55 on a clone with one worktree: the path seen from the worktree is the clone's hooks, a commit made in the worktree carries the trailer, a second installation does not overwrite the first, and `core.hooksPath` changes the path.
