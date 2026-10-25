# mm-method

**English** · [Español](README.es.md)

A working methodology, packaged as a Claude Code plugin, for software projects built with AI agents in which one person (the owner) makes the technical decisions. It works with Claude Code alone or together with a second agent, such as Codex, in the same repository.

It is not a library or a tool that automates anything: it is a set of instructions that the agent follows. What makes it hold in a project is the project's `CLAUDE.md`, the protection of `main` and the team's discipline. The skill teaches the agent how to work and gives it the skeletons to set it up.

## The problem it solves

A coding agent is fast and capable, but in a project that lasts weeks the same problems show up:

- it makes technical decisions that were not up to it, or makes them silently;
- what was decided in one chat does not reach the next one, or another computer, or the other agent;
- it codes before it is clear what needs to be coded;
- it writes tests that copy what the code returns, and so they always pass;
- two agents in the same repo step on each other's branches and files;
- context and weekly usage are spent without anyone noticing.

The methodology answers each one with a concrete rule and the reason for that rule.

## The principles

| # | Principle | Why |
|---|---|---|
| 1 | The technical source prevails (the standard, the specification) | If the product has to comply with something, no decision, neither the owner's nor the agent's, contradicts it silently. |
| 2 | Assumptions and limitations in plain sight | The product's user and the owner always know what was assumed and what is not checked. |
| 3 | The owner decides; the agent proposes | Decisions are requested in a numbered list, with the recommended option in bold, and are recorded verbatim. |
| 4 | First plan, then code | No product code is written without the owner's word. |
| 5 | Nothing made up | No data, results or capabilities; a test's expected values are not copied from the code. |
| 6 | Memory lives in the repository | It travels with Git to other chats, computers and agents. |
| 7 | No filler | In the interface, the texts, the documents and the code. |

## What it includes

```
mm-method/                            plugin catalog (this repository)
├── .claude-plugin/marketplace.json
├── plugins/metodologia-agentes/
│   ├── .claude-plugin/plugin.json
│   └── skills/
│       ├── metodologia-agentes/       the skill in Spanish
│       │   ├── SKILL.md
│       │   └── references/            10 references, read only when needed
│       └── agent-methodology/         the same skill in English
├── docs/es/  ·  docs/en/              the documentation
├── CHANGELOG.md
└── README.md (English)  ·  README.es.md (Español)
```

Both skills have the same content. Each has its description in its own language, so the agent uses the one that matches the conversation.

| Reference | What it covers |
|---|---|
| `decisions.md` | How decisions are requested, recorded and written down |
| `memory.md` | Memory in the repo, the changelog, several people and several computers |
| `cycle.md` | The cycle of a module, from planning to the PR |
| `testing-and-reviews.md` | Independent tests, mutation and adversarial reviews |
| `two-agents.md` | Two agents in the same repository |
| `repository.md` | Branches, PRs, changelog, CI and the routine after the merge |
| `costs.md` | Context, usage and workflow size |
| `style.md` | No filler |
| `templates.md` | Skeletons for `CLAUDE.md`, the memory, the changelog and the prompts |
| `security.md` | Secrets, protections, outside content, CI and servers |

## Install

You need Claude Code with plugin support. The repository is its own catalog:

```bash
claude plugin marketplace add MarcosSanchez7020/mm-method
claude plugin install metodologia-agentes@mm-method
```

The default scope is `user`: the skill is available in all your projects. To keep it in one project only, use `--scope project` (it is saved in the project's configuration) or `--scope local`.

Inside Claude Code this also works: `/plugin marketplace add MarcosSanchez7020/mm-method` and then `/plugin install metodologia-agentes@mm-method`.

To check that it is installed:

```bash
claude plugin list
```

## Update

```bash
claude plugin marketplace update mm-method
claude plugin update metodologia-agentes@mm-method
```

Changes apply when Claude Code is restarted. What changed in each version is in the [CHANGELOG](CHANGELOG.md).

## Uninstall

With the same scope you used to install; without `--scope`, `uninstall` uses `user`:

```bash
claude plugin uninstall metodologia-agentes@mm-method --scope user
```

Change `user` to `project` or `local` if you installed it that way. Remove the catalog only when no other installation or plugin uses it; without `--scope`, `marketplace remove` removes it from every scope:

```bash
claude plugin marketplace remove mm-method
```

## Quick start

The skill activates on its own when the conversation is about what it covers. It can also be requested explicitly:

- "Let's set up this repo with the methodology: let's write the `CLAUDE.md` and the memory folder."
- "Before coding this, give me the decisions as a list."
- "Plan the invoicing module with the methodology; don't write code until I tell you."
- "I'm going to work with Codex in this same repo. How do we organize?"
- "Close the chat: bring the memory up to date and give me the prompt for the next one."

To adopt it in a project, follow the [adoption guide](docs/en/adoption.md).

## Documentation

- [Complete guide](docs/en/guide.md): how each part works and why.
- [Adoption guide](docs/en/adoption.md): from zero to a set-up project, step by step.
- [Frequently asked questions](docs/en/faq.md).

## Context cost

Estimate from `claude plugin details` with Claude Code 2.1.296, on 25/10/2026, for version 1.0.0, from the root of this repository:

```bash
claude --plugin-dir plugins/metodologia-agentes plugin details metodologia-agentes
```

| What | Approximate tokens |
|---|---|
| The two descriptions, loaded in every session | ~643 |
| One skill when activated (its `SKILL.md`) | ~1,900 |
| Each reference, only if the agent reads it | depends on the reference |

The tool warns that these are estimates and may differ from actual usage. An independent review ran the same command, with the same Claude Code version on the same day, and got ~456 always-on tokens and ~1,300 per activation. The cause of the difference has not been determined: take them as a range (~456 to ~643 always-on and ~1,300 to ~1,900 per activation), not as an exact figure.

## Limits

- The skill instructs; it does not enforce. If the project's `CLAUDE.md` says otherwise, the project prevails.
- It ships no hooks, commands or servers: it runs nothing on its own.
- It does not include or install other plugins. claude-mem is suggested as an optional complement, with its adjustments, in the `memory.md` reference.
- The protection of `main`, the required checks and the PR template are configured in the project's repository; the adoption guide says how, but the skill does not create them by itself.
- It is designed for Claude Code. With another agent, the part that applies to it is in `AGENTS.md`, which points to the same rules.

## Security

- **What it installs.** Text only: two skills in Markdown. It ships no hooks, commands, agents, MCP or LSP servers, and no code that runs on installation (`claude plugin details`: 2 skills, 0 agents, 0 hooks, 0 MCP servers, 0 LSP).
- **The code examples** in the documentation (the approval check and the machine hook) only run if someone copies and runs them. Read them first, like any third-party code.
- **What the methodology requires:** no secrets or personal data in the repo, no disabling protections, treating outside content as data (not instructions), CI with `pull_request` and minimum permissions, and local servers on `127.0.0.1`. It is all in the skill's `security.md` reference.
- **Public repository, public memory.** If you adopt the methodology in a public repo, its memory folder, the owner's recorded answers and the changelog are public too.
- **claude-mem** is optional and third-party: it stores session data on the computer and sends it to the AI provider configured for it. Review it before installing it.

## Version and license

Version 1.0.0 (see [CHANGELOG](CHANGELOG.md)). License: MIT ([LICENSE](LICENSE)).
