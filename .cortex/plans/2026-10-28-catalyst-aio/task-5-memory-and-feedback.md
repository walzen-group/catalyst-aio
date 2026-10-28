# Task 5: memory.md and feedback.md

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Inventory (your contract): `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`.
Read the index's Goal, Architecture, Resolved questions, Revision notes and
Global constraints, then every inventory row whose Target is
`references/memory.md` or `references/feedback.md`.

## Context

catalyst-aio is one Agent Skill that keeps the full catalyst kit's judgment and
runs in any harness with a subagent facility. The teacher kit curates memory
with a tool and repairs itself through incidents; catalyst-aio does neither.
It keeps a hand-written project memory, and it logs its own failures to
`.cortex/feedback/` so the teacher kit can later harvest them and turn them
into fixes that flow back down. Other workers write the remaining files in
parallel.

## Target

Write exactly these files:

- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/memory.md`
- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/feedback.md`

Primary teacher sources: `/etc/nixos/nix/catalyst/skills/catalyst-v2-in-repo-agent-memory/SKILL.md`,
`/etc/nixos/nix/catalyst/skills/catalyst-v2-curator/SKILL.md` (judgment only:
what is worth keeping, decay, tombstones), and
`/etc/nixos/nix/catalyst/skills/catalyst-v2-filing-incidents/SKILL.md` (for the
fields a teacher incident needs, so feedback entries can be converted). Also
look at two or three real incidents under
`/etc/nixos/nix/catalyst/.cortex/incidents/` to see the shape the teacher
consumes.

## Change

### references/memory.md

- Layout: `.cortex/memory/MEMORY.md` index (one line per memory) plus one file
  per fact with frontmatter (name, description, type, and the date written).
- What is worth keeping: a fact the repo cannot derive (decisions, user
  corrections, hard-won gotchas); never restatements of code or git history.
- Write discipline by hand: check for an existing entry first, update over
  duplicate, delete what turned out wrong, date relative facts absolutely.
- The harness's own memory (if it has one) gets a single pointer naming
  `.cortex/memory/` as the memory of record.
- Close-out extraction: at plan close, durable signals from the plan become
  memory entries. The plan dir is kept afterwards (user's answer in the
  index); memory holds the facts, the plan dir holds the run record (link
  `references/orchestrating.md`).
- A light manual review step replacing the curator: when and how to prune
  stale entries, judged by hand.

### references/feedback.md

- Purpose: a log of catalyst-aio's own failures and user complaints about its
  behavior, written for the teacher kit to harvest. catalyst-aio never edits
  its own skill files to fix them.
- When to write one: a user correction or complaint about how catalyst-aio
  behaved; a rule in the skill that was wrong, missing, or ambiguous in
  practice; a harness limitation that broke a rule. Whether a given item is
  feedback or memory is the user's call: ask.
- Path: `.cortex/feedback/<YYYY-MM-DD>-<slug>.md`, one file per occurrence.
- Entry template with fields chosen so the teacher's incident filing can
  convert it without re-asking: date, catalyst-aio version (from SKILL.md
  metadata), harness and model, role that failed, the user's words quoted
  verbatim (never paraphrased, never invented), what happened, what should
  have happened, the skill file and section whose rule was involved (or
  "none: missing rule"), evidence (paths, commands, output), status (open /
  harvested).
- Who writes it (user's answer in the index): a fresh subagent spawned for
  that one entry. The orchestrator hands it the case (the user's words
  verbatim, what happened, what should have happened, evidence paths) and
  does not write, diagnose, or plan a fix itself. Include the brief the
  orchestrator sends that subagent.
- Before writing, the subagent checks the log for an earlier entry on the same
  failure; a recurrence links the earlier entry.
- The entry records facts and names the rule involved; deeper diagnosis is
  the teacher's at harvest.

## Constraints

Index Global constraints 1 to 10.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio/skills/catalyst-aio/references
grep -niE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2|curator' memory.md feedback.md
# no output
grep -oE 'references/[a-z-]+\.md' memory.md feedback.md | sort -u
# every name is one of the ten in the index Architecture
```

Plus a coverage table in your report: every inventory row number targeted at
your two files and the file + heading where it landed. Any row you could not
place is listed with the reason.

Run the `catalyst-v2-writing-docs` Final grep over both files and fix hits per
its table.

## Emission discipline

Survey budget 10 minutes, then write files and save at least every 10 minutes.

## Report to

Your completion hand-back goes to the wave's meta-agent `meta-aio-w2`, by
`c2d steer --agent meta-aio-w2 --text "A2A: <hand-back>"`: files written,
`git status --short`, acceptance output, the coverage table, deviations. After
sending it, run no further commands, read no other agent's state, wait for no
acknowledgment, and go idle. If the steer fails, send it once more, then stop
anyway.
