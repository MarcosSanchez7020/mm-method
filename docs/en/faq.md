# Frequently asked questions

## Does it work without a second agent?

Yes. Everything in the methodology works with Claude Code or Codex as the only agent. With Codex, include `AGENTS.md` even when there is no second agent. The `two-agents.md` reference is only read when there are two agents in the same repository.

## Does it work if there is no separate "technical owner"?

Yes: the owner is whoever decides, even if it is the same person who asks for the work. What changes is that decisions are written down as a list and recorded, and that is just as useful when you come back to the project a month later.

## Isn't it too much process?

For a script that takes an afternoon, yes. The methodology is meant for work that lasts longer than one chat. The [guide, section 12](guide.md#12-small-projects-and-large-teams), says what can be cut in a small project without losing the essentials and what should not be cut.

## Why decisions as a list and not in conversation?

Because a long conversation leaves doubts about what was decided in the end. With "1a, 2b" there is nothing to reread, and the verbatim record keeps what was asked and what was answered. The recommendation goes in bold because the agent has the technical context fresh; the decision still belongs to the person.

## Why memory in the repository and not in Claude's memory?

The agent's local memory stays on one computer and is not seen by other chats on other machines or by another agent. The repository travels with Git to everyone. Memory tools can help, but if they conflict with what is written in the repo, the repo wins.

## Do I need claude-mem or another memory tool?

No. The methodology's memory lives in the repository. claude-mem is suggested as an optional complement and is third-party. Before enabling it, keep in mind that it stores session data on the computer and that, according to its documentation, it sends the observations to the AI provider configured for it (Claude, OpenRouter or Gemini): review which provider they go to and how it handles them, and do not use it in sessions with secrets or personal data that should not leave the computer. The other suggested adjustments (the repo rules, disabling it for independent reviews and its usage thresholds) are in the skill's `memory.md` reference.

## How does it coexist with an existing `CLAUDE.md`?

The project's `CLAUDE.md` takes precedence. When adopting the methodology, the agent proposes where each section fits and lists what conflicts. The project's own rules stay.

## How do I invoke it if it does not activate on its own?

Ask for it: "let's work with the methodology", or name the skill (`metodologia-agentes` or `agent-methodology`). In Codex, use the complete name, `$metodologia-agentes:<skill>`, because the short one did not load the skill in the test. In a project that adopted it, the agent may load it on its own from its description when the conversation is about what it covers; that activation is not guaranteed.

## Why are there two skills with the same content?

One in Spanish and one in English.

Both clients use the same package folders; there is no separate copy for Codex and Claude. The figures below are for mm-method 1.2.0 in Claude Code, not for Codex.

Each has its description in its own language, so the agent uses the one that matches the conversation. According to `claude plugin details` with Claude Code 2.1.296, on 25/10/2026, together they cost ~472 to ~693 tokens of context per session, and each one ~1,600 to ~2,500 when activated, depending on the Claude Code configuration; the cause of the difference has not been determined (the command, the details and the 1.0.0 figures are in the [README](../../README.md#context-cost)).

## Does it run anything on its own?

No. It has no hooks, commands or servers. It is instructions for the agent.

## What about secrets and personal data?

The methodology forbids putting them in the repository, in memory and in the changelog, and forbids the agent from handling tokens or passwords: if a panel with credentials has to be touched, a person does it.

## How is it updated?

In Codex, follow the [installation and update guide](codex.md). In Claude Code:

```bash
claude plugin marketplace update mm-method
claude plugin update metodologia-agentes@mm-method
```

Then restart Claude Code. The changes in each version are in the [CHANGELOG](../../CHANGELOG.md).

## Can I adapt it?

Yes, in each project's `CLAUDE.md`: that is where the go word, the language, who reviews, which models are used and the exceptions are set. The skill gives the method; the project, its values.
