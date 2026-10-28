# Task 1: insight inventory (keep / adapt / drop)

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Read its Goal, Architecture, Resolved questions and Global constraints before
starting. You receive only this task; the other tasks are not yours.

## Context

catalyst is a multi-agent kit (orchestrator, delegates, meta-agent) whose
judgment lives in 20 skills and was shaped by 88 incident reports. Its
mechanics depend on herdr (a terminal multiplexer) and three tools (`c2d`,
`c2m`, `c2r`). catalyst-aio is a single harness-neutral skill that keeps the
judgment and drops the mechanics. Your inventory is the contract every wave-2
writer implements, so a rule you miss is a rule catalyst-aio loses.

## Target

Write exactly one file:
`/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`

Read (never write): `/etc/nixos/nix/catalyst/skills/*/SKILL.md`,
`/etc/nixos/nix/catalyst/.cortex/incidents/*.md`,
`/etc/nixos/nix/catalyst/.cortex/memory/`. Record
`git -C /etc/nixos/nix/catalyst rev-parse --short HEAD` and
`git -C /etc/nixos/nix/catalyst status --short skills/ | wc -l` at the top of
the inventory (other agents may be editing the kit while you read).

Non-goals: writing any catalyst-aio skill file; editing the kit.

## Change

1. Read every catalyst-v2-* SKILL.md, plus `humanizer` and `asd-ste100` (skim
   for what a writing reference needs). Skip `herdr`.
2. Extract each distinct rule or piece of judgment as one row. A rule is a
   behavior an agent must do or avoid ("orchestrator never edits repo files
   outside .cortex/", "carry the symptom, never your hypothesis"). Merge
   duplicates stated in several skills into one row and list every source.
3. Classify each row:
   - **keep**: applies unchanged in any harness with subagents.
   - **adapt**: the judgment holds, the mechanism changes (herdr tab becomes
     subagent; meta-agent becomes the per-wave verifier; c2m becomes hand-written
     memory; incident filing becomes a `.cortex/feedback/` entry). State the
     adapted form in one sentence.
   - **drop**: exists only for dropped machinery (index Global constraint 4).
     State why in one clause.
4. Assign every keep/adapt row a target file from the index Architecture
   (`SKILL.md` or `references/<name>.md`). One target per row.
5. Scan all 88 incidents. For each, write one line: incident filename, the
   lesson in one sentence, and either the inventory row number(s) that already
   cover it or "NEW" with a new row added, or "machinery-only" when the lesson
   concerns only dropped machinery. Incidents are the evidence that a rule
   matters; a harness-neutral lesson with no row is the gap this step exists
   to find.
6. Add a short section "Open questions for the user": decisions the index does
   not settle and that change what catalyst-aio says. Keep it to genuine forks;
   leave it empty if none.

Inventory layout:

```markdown
# catalyst-aio inventory
Kit HEAD: <sha>; dirty skill files at read time: <n>

## Rules
| # | Rule (one sentence) | Class | Adapted form / drop reason | Target | Sources |
|---|---|---|---|---|---|

## Incident coverage
| Incident | Lesson | Covered by |
|---|---|---|

## Open questions for the user

## Counts
keep: n, adapt: n, drop: n; incidents: covered n, NEW n, machinery-only n
```

## Constraints

Index Global constraints 1, 3, 4, 7 (style rules apply to the inventory's
prose; the humanizer pass does not), 9.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio
grep -c '^| [0-9]' inventory.md            # rule rows; expect well over 100
ls /etc/nixos/nix/catalyst/.cortex/incidents/*.md | wc -l
grep -cE '^\| [0-9]{4}-[0-9]{2}-[0-9]{2}-' inventory.md   # incident rows; must equal the count above
grep -E '^\| [0-9]+ .*\| (keep|adapt) \|' inventory.md | grep -vE 'SKILL\.md|references/[a-z-]+\.md' 
# expect no output: every keep/adapt row has a target
```

Every target filename must be one of the eleven in the index Architecture.

## Emission discipline

Survey budget: 20 minutes of reading before the first write to
`inventory.md`. After that, write rows to the file as you go and save at least
every 10 minutes.

## Report to

Your completion hand-back goes to the wave's meta-agent `meta-aio-w1`, by
`c2d steer --agent meta-aio-w1 --text "A2A: <hand-back>"`. The hand-back
names the file, the counts line, the open questions, and the acceptance
command output. After sending it, run no further commands, read no other
agent's state, wait for no acknowledgment, and go idle. If the steer fails,
send it once more, then stop anyway.
