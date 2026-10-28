# Task 3: planning.md and effort.md

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Inventory (your contract): `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`.
Read the index's Goal, Architecture, Resolved questions, Revision notes and
Global constraints, then every inventory row whose Target is
`references/planning.md` or `references/effort.md`.

## Context

catalyst-aio is one Agent Skill that keeps the full catalyst kit's judgment and
runs in any harness with a subagent facility. Your two references tell the
orchestrator how to write plans and task specs for blank-context subagents, and
how to pick an effort level per role on a single model. Other workers write the
remaining files in parallel; link to them by the fixed filenames in the index
Architecture.

## Target

Write exactly these files:

- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/planning.md`
- `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/references/effort.md`

Primary teacher sources: `/etc/nixos/nix/catalyst/skills/catalyst-v2-planning-artifacts/SKILL.md`,
`/etc/nixos/nix/catalyst/skills/catalyst-v2-model-picking/SKILL.md`. The
inventory decides what is kept; the sources give you the full wording.

## Change

### references/planning.md

The plan directory `.cortex/plans/<date>-<epic>/` with `00-index.md` and
`task-N-<slug>.md`. Index doc contents (Status line, header note, goal,
resolved questions, revision notes, global constraints, task table, tracks,
effort allocation, out of scope, whole-change verification, smoke test). The
Status line rule and the terminal values. Task spec anatomy (context, target,
change, constraints, acceptance, emission discipline, report-to) and every
spec rule the inventory keeps: self-contained, carry the symptom never your
hypothesis, environmental premises established, one task per spec, why for
taste calls, verification burden, acceptance inviolable, report a diff never a
commit, shared-checkout append-only git rule, user-facing deliverables to
`.cortex/reports/`, fixes carry the test-first steps (link
`references/test-first.md`), every spec carries the writing convention (link
`references/writing.md`). In catalyst-aio the report-to target is the
orchestrator (the subagent's final message); the implementer's duty ends with
that message.

### references/effort.md

Effort-based routing on one model:

- Default: every role runs on the session's model. Effort is the dial.
- An effort table: levels `low`, `medium`, `high`, `max` with task shape and
  examples (low: mechanical, already decided; medium: multi-file with a clear
  spec and checkable acceptance; high: open-ended, debugging, design,
  environment bring-up, verification; max: only on the user's explicit
  directive).
- A role default table: orchestrator, implementer (by task shape), verifier.
  Pick the defaults from the inventory's adapted rows; the teacher ran
  reviews, verification and debugging one tier above spec'd implementation.
- Start low, escalate on evidence; spec quality lowers the effort a task needs
  but never the effort judgment under blockage needs; lock the allocation
  before dispatch and do not fiddle; stuck-delegate order before raising effort.
- Where the harness cannot set effort per subagent: say so in the dispatch
  report, run the default, and state the intended effort in the brief.
- Optional model map: a small editable table (tier name to model id) for
  harnesses that let a subagent run a different model, with an explicit note
  that the map is optional and its model column empty by default. Include a
  per-tier concurrency cap column; per the user's answer in the index, the top
  tier ships with a default cap of 2 concurrent subagents, user-editable, and
  work beyond the cap queues or runs on the next tier down.
- The effort decision is reported to the user at dispatch (the only record
  of routing in a reduced workset).

Avoid teacher model ids and harness names in rules; an example row may name a
model generically ("your strongest model").

## Constraints

Index Global constraints 1 to 10.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio/skills/catalyst-aio/references
grep -niE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2|opus|deepseek|sonnet|kimi' planning.md effort.md
# no output
grep -oE 'references/[a-z-]+\.md' planning.md effort.md | sort -u
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
