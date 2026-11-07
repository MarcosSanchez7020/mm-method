# The plan map

When the work moves from a single module to a long stretch, with several fronts, several chats and several agents, the plan lives in a file in the repository: the plan map, by blocks, with checkboxes. It lets any chat know where the project stands and what falls to it, and ensures nothing is taken as done without evidence.

## What it has

- A general summary: where the project comes from, what was decided and when.
- The blocks at a glance: a table with each block, its status ("pending", "in progress", "done" or "postponed"), which blocks it depends on and who does it.
- What runs in parallel and what goes in series.
- What the map does not cover: what is still missing from the project and is not in the map, with the document where it is.
- Each block, one by one: its steps as checkboxes, with who does them, and what is "left open".
- A check log, by date.

## Rules

1. **Everything that gets done is ticked in the map in the same commit that does it.** `- [ ]` is a pending step; `- [x]` is a done step, followed by who, the date and the commit or the evidence (a PR, a link, an answer from the owner in quotation marks). Nothing is ticked before it is done, and nothing that was not verified.
2. **The owner's steps** (approve, merge, answer, paste a prompt) are ticked by the agent that receives their answer, with their verbatim words and the date.
3. **Each agent ticks its own in its branch.** When `main` is merged, the other agents' ticks are kept: conflicts in the map are resolved by keeping both sides.
4. **A block's status is changed when it really changes.** What a block leaves open is written in its "left open".
5. **Nothing is deleted.** If the plan changes (a new decision, a block that is split or postponed), it is changed in the map with the date and the owner's phrase, and in the owner's answers record; what no longer applies is struck through with `~~` and the reason is given.
6. **Every chat reads the map at the start**, after `CLAUDE.md` and the memory, to know which block the project is in and what falls to it.
7. **New work goes through the owner.** New work, or something from the list of what the map does not cover whose moment has come, is first passed to the owner as a decision and, if they approve it, enters as a new block or as steps of a block.
8. **Numbers are assigned by a single chat.** Blocks are numbered consecutively; if several chats add to the map at the same time, the numbers are set by the one that coordinates (the hub, if there is one: `hub.md`), so that two branches do not create the same block.

## The drawing

- A script in the repository, with no dependencies, reads the map and generates the drawing (blocks, dependencies, series and parallel), for example an SVG and a text diagram. The drawing is not edited by hand, and the map is not altered to make the drawing come out right.
- Whoever ticks a checkbox regenerates the drawing and commits it in the same commit.
- A test in the suite checks that the drawing matches the file (the script has a check mode), so that they do not drift apart.
- If the drawing conflicts when `main` is merged, it is not resolved by hand: both sides of the map are kept and the drawing is regenerated.
- If the project has a status page, the drawing is shown there, below the status.

## Map, memory and changelog

- The map says where the plan stands; the team log, what was done from each machine; the changelog, what changed in each file. All three are noted in the same commit.

The skeleton is in `templates.md`, "The plan map".
