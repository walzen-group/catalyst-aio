# Task 2: orchestration core (SKILL.md + four references)

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Inventory (your contract): `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`.
Read the index's Goal, Architecture, Resolved questions and Global constraints,
then every inventory row whose Target is one of your five files.

## Context

catalyst-aio is one Agent Skill that keeps the full catalyst kit's judgment and
runs in any harness with a subagent facility. You write its entry point and the
four references that define who does what: the orchestrator plans and
delegates, implementer subagents write code, a fresh verifier subagent per wave
checks the work, the orchestrator audits the verifier's report. Four other
workers write the remaining references in parallel; link to them by the fixed
filenames in the index Architecture.

## Target

Write exactly these files under `/home/nixos/repos/catalyst-aio/skills/catalyst-aio/`:

- `SKILL.md`
- `references/orchestrating.md`
- `references/reduced-workset.md`
- `references/subagents.md`
- `references/verifier.md`

Non-goals: the other six references, README, anything outside `skills/catalyst-aio/`.

## Change

### SKILL.md

Frontmatter per index Global constraint 5. The description states the trigger:
use only when the user has asked for catalyst or catalyst-aio by name; multi-step
work, delegable work, or the word "subagent" are not triggers. Body sections:

1. When catalyst-aio applies (the by-name gate, and the non-triggers).
2. Core principles: inventory rows targeted at SKILL.md (user's word is ground
   truth; ambiguity is a question; a gate you cannot satisfy is a question;
   an enumerated go-ahead authorizes only what it names; never manufacture the
   user's words; REQUIRED references are a gate; you enter orchestration by
   acting).
3. Roles table: orchestrator, implementer, verifier. Who owns what, who never
   does what. The orchestrator's own writes reach only `.cortex/` working
   artifacts (plans, memory, reports, feedback); any other repo file is
   delegated work regardless of size. catalyst-aio agents never edit the
   catalyst-aio skill files.
4. Routing table: situation to reference file, covering all ten references.
   Mark which references are REQUIRED before which action.
5. Directory conventions per index Global constraint 8.
6. Harness requirement: a subagent facility. Without one, tell the user and ask
   whether to proceed in-session (index orchestrator decision).

### references/orchestrating.md

The full lifecycle loop, adapted: Understand (formalize under-specified work
with the user first) / Plan / Dispatch a wave of implementer subagents /
Verify with one fresh verifier subagent per wave / Finish (audit the
hand-back, re-run no gate, write memory, set plan Status terminal, extract
durable signals into memory, keep the plan dir: per the user's answer in the
index, plan dirs are never deleted by catalyst-aio). Plus: "orchestrator does
not implement" with the rejected rationalizations; who verifies what table;
stuck-delegate order (spec, environment, split, then effort up); cancel
handling; attribution and user-authority rules; feedback routing (a user
complaint about catalyst-aio behavior becomes a `.cortex/feedback/` entry
written by a fresh subagent the orchestrator spawns for that one case; the
orchestrator hands over the user's words verbatim, what happened and what
should have happened, and does not write, diagnose or plan the fix itself;
whether something is recorded as feedback or memory is the user's call; see
`references/feedback.md`).

### references/reduced-workset.md

Small-task path: no plan dir, inline brief, still one implementer subagent,
still a fresh verifier subagent, still the effort report to the user. The
promotion signals to the full lifecycle.

### references/subagents.md

Harness-neutral delegation mechanics, replacing everything herdr and the
dispatch tool did:

- A subagent starts blank: the brief is its whole world. Brief skeleton:
  role, the absolute spec path (or inline requirements), which catalyst-aio
  reference files it must read by absolute path (the orchestrator resolves the
  skill directory at runtime), the effort level, the report format, and the
  rule that it writes only its named files.
- Parallel dispatch only for real independence; launch independent subagents
  in one batch when the harness allows it.
- Background vs blocking subagents: when the harness can run subagents in the
  background, never end your turn with one in flight and no way to be resumed
  by its completion; when it cannot, dispatch blocking.
- Fresh context per task; never reuse a subagent across a gap.
- Worktree isolation for concurrent writers to the same files; teardown lands
  work on a named branch.
- Effort and model selection go on the dispatch where the harness supports it
  (point to `references/effort.md`).
- Stopping a subagent: an interrupt is not a verified stop; check, then tell
  the user what changed and how to revert.
- Map from teacher concepts the inventory marked adapt (tab → subagent, wake →
  harness completion notice, steer → follow-up message or a fresh subagent).
  Write the mapping as behavior, without teacher vocabulary (index Global
  constraint 2).

### references/verifier.md

The verifier role, adapted from the teacher's meta-agent verification half:

- Spawned fresh once per wave, after every implementer of the wave returned;
  never reused across waves; does no task work.
- Brief from the orchestrator: each task's spec path, each implementer's
  report, the whole-change commands from the plan index.
- Procedure: confirm each implementer's recorded gate output is genuine
  (read it, do not re-run each gate); read each diff against its spec; run the
  whole-change check; for fixes, confirm a recorded failing run exists before
  accepting green (point to `references/test-first.md`).
- Hand-back written to `.cortex/reports/handbacks/<date>-<wave>.md` and
  returned as the subagent's final message, with required sections:
  files changed, per-task diff verdict, gate evidence (paths), whole-change
  output, open items and holds, deliverable paths. A thin hand-back goes back
  to a verifier, never to the orchestrator's own gate run.
- A verifier may send a task back: the orchestrator dispatches a fresh
  implementer with the verifier's findings, then a fresh verifier.

## Constraints

Index Global constraints 1 to 10.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio
wc -l skills/catalyst-aio/SKILL.md skills/catalyst-aio/references/{orchestrating,reduced-workset,subagents,verifier}.md
# SKILL.md under ~500 lines; references uncapped (index Global constraint 6)
grep -rniE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2' skills/catalyst-aio/SKILL.md skills/catalyst-aio/references/{orchestrating,reduced-workset,subagents,verifier}.md
# no output
grep -oE 'references/[a-z-]+\.md' skills/catalyst-aio/SKILL.md | sort -u
# exactly the ten names in the index Architecture
awk '/^description:/{print length($0)-13}' skills/catalyst-aio/SKILL.md
# <= 1024
```

Plus a coverage table in your report: every inventory row number targeted at
your five files, and the file + heading where it landed. Any row you could not
place is listed with the reason.

Run the `catalyst-v2-writing-docs` Final grep over your five files and fix hits
per its table.

## Emission discipline

Survey budget 15 minutes, then write files and save at least every 10 minutes.

## Report to

Your completion hand-back goes to the wave's meta-agent `meta-aio-w2`, by
`c2d steer --agent meta-aio-w2 --text "A2A: <hand-back>"`: files written,
`git status --short`, acceptance output, the coverage table, deviations. After
sending it, run no further commands, read no other agent's state, wait for no
acknowledgment, and go idle. If the steer fails, send it once more, then stop
anyway.
