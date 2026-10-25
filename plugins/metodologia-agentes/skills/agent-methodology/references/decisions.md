# Decisions: how they are requested, recorded and written

## Who decides what

- The technical owner decides what is technical. That includes how results, states and warnings are presented to the user, because a bad presentation changes what the user understands.
- If another person on the team owns an area (for example, interface design), that person's decisions are passed to them. Who decides what is written in `CLAUDE.md`.
- Agents do not decide: they propose, recommend and explain. Subagents that execute tasks do not decide either: whatever calls for judgment goes back to the coordinating agent or to the owner.

## How a decision is requested

- **In a numbered list**, each decision with its options labeled by letter (a, b, c). The recommended option goes first and in bold. That way the owner answers "1a, 2b" with no misunderstandings.
- **With its weight.** In one or two lines: what changes with each option, what it costs and what risk it leaves. If it helps in deciding, it comes with a sweep or a measurement using the real code. The agent picks the cases and they are informational.
- **What is implicit is not asked.** What already follows from a written decision is stated as "I take this as decided; tell me if not".
- **Our interpretations go separately**, in their own list, marked as our interpretation, with their source.
- **If an instruction can be read two ways**, ask before touching documents. For example, how much to remove, or whether a rule applies to one task or to all of them.
- **Right away.** Decisions are passed on as soon as they exist, without waiting for what is only informational. Whatever arrives later is reported separately.
- **Against the technical source**, never silently: say "the source says this", with the exact reference, and propose the option that complies with it.
- **What the owner did not say is not attributed to the owner.** If an answer leaves something open, ask.

## The verbatim record

Each answer from the owner is added to the record (`MEMORY/owner_answers.md`) at the moment it arrives, at the latest before the next commit. Each entry carries:

1. What was asked, with all its options and the recommended one.
2. The owner's words as spoken, colloquial parts included, in a quote block.
3. In parentheses, how the answer is read and **what was not said**.

If the owner answered with a text the agent proposed (a button, a suggested reply), that is stated. That way nobody confuses the agent's wording with the owner's.

Why so literal: weeks later, when a new chat or the other agent needs to know why something is the way it is, the quote settles the question without interpretation.

## Agent clarifications

When the agent clarifies something the owner did not say (a detail of how something is set up, a consequence), it is written as "<agent>'s clarification, to be confirmed" until the owner confirms it. Then it is changed to "confirmed by <owner> on <date>", with their words.

## Writing down what was decided

- Decisions are written in the documents that are the source of truth (scope, requirements, data model, diagrams) and in the changelog, with the owner's words, the date and their answer ("1a").
- Rules that must always hold go in `CLAUDE.md`, with their changelog entry, on the branch being worked on. If a rule changes, it is changed there.
- After writing weighty decisions, a short review of those texts (`testing-and-reviews.md`), and the calculations of any examples that end up written are checked.
- The agent confirms to the owner what it understood. The commit, only when the owner asks for it.

## Known and accepted

What the owner left as "known and accepted", or for later, is not fixed or touched unless the owner says so, even if the agent sees room for improvement. It can be brought up as a reminder, but not changed.
