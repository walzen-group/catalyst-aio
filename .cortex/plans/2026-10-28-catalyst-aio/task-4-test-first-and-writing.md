# Task 4: test-first.md and writing.md

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Inventory (your contract): `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`.
Read the index's Goal, Architecture, Resolved questions, Revision notes and
Global constraints, then every inventory row whose Target is
`references/test-first.md` or `references/writing.md`.

## Context

catalyst-aio is one Agent Skill that keeps the full catalyst kit's judgment and
runs in any harness with a subagent facility. Because it ships as one skill,
it cannot rely on separate writing or humanizer skills being installed: your
`writing.md` carries the writing convention and the humanizer pattern list
itself. `test-first.md` carries the test-first procedure for any change with a
checkable outcome. Other workers write the remaining files in parallel.

## Target

Write exactly these files:

- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/test-first.md`
- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/writing.md`

Primary teacher sources: `/etc/nixos/nix/catalyst/skills/catalyst-v2-sdd-rules/SKILL.md`,
`/etc/nixos/nix/catalyst/skills/catalyst-v2-writing-docs/SKILL.md`,
`/etc/nixos/nix/catalyst/skills/humanizer/SKILL.md`. The inventory decides what
is kept; the sources give you the full wording.

## Change

### references/test-first.md

The four steps (test first, record the failing run against current behavior,
minimal fix, green), the failing run as source of truth, what counts as a
recorded red run, what the verifier checks (no green accepted without a
recorded red), and the exception path: a red run that cannot be produced is a
question to the user, and a confirmed exception is written into the report.

### references/writing.md

Self-contained writing convention for anything a catalyst-aio role writes for
a human (reports, README-type docs, plan prose, user replies). Keep every
inventory row targeted here. Include:

- The structure rules and wording rules the inventory keeps from the teacher's
  writing skill.
- A humanizer section: the AI-writing pattern list (as a compact table:
  pattern, example, fix) and the closing two-prompt audit. Do not depend on a
  separately installed humanizer skill.
- The final grep command and its triage table, adjusted so it does not
  reference teacher-only files.
- Which texts get the full humanizer pass (user-facing docs and reports) and
  which get the style rules only (plan docs, briefs, hand-backs).

## Constraints

Index Global constraints 1 to 10. Writing about banned words necessarily
contains them; keep each banned word inside code spans or the grep pattern,
and say in your report which Final-grep hits are such quotations.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio/skills/catalyst-aio/references
grep -niE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2' test-first.md writing.md
# no output
grep -oE 'references/[a-z-]+\.md' test-first.md writing.md | sort -u
# every name is one of the ten in the index Architecture
```

Plus a coverage table in your report: every inventory row number targeted at
your two files and the file + heading where it landed. Any row you could not
place is listed with the reason.

Run the `catalyst-v2-writing-docs` Final grep over both files; fix hits per its
table except quotations of banned terms in writing.md, which you list.

## Emission discipline

Survey budget 10 minutes, then write files and save at least every 10 minutes.

## Report to

Your completion hand-back goes to the wave's meta-agent `meta-aio-w2`, by
`c2d steer --agent meta-aio-w2 --text "A2A: <hand-back>"`: files written,
`git status --short`, acceptance output, the coverage table, deviations. After
sending it, run no further commands, read no other agent's state, wait for no
acknowledgment, and go idle. If the steer fails, send it once more, then stop
anyway.
