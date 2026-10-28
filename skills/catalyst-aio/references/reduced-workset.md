# Reduced workset

The reduced workset runs a small task, a chore, or a follow-up on existing work
with one implementer subagent and one verifier subagent, briefed inline, with no
plan directory. Multi-task efforts run the full lifecycle in
references/orchestrating.md.

Reduced means less process. The orchestrator still doesn't write any code: the
documents shrink (no plan directory, no formal spec), and an implementer
subagent still writes every change. Delegation is the default regardless of task
size, so a small task shrinks the process and keeps the delegation. Task size
picks the workset and never changes who writes the code.

## Rationalizations that end in a direct edit

Each of these has been tried, and each is rejected:

- "The user asked me to fix it." A request names the outcome. It does not name
  the executor.
- "I already have the context." Knowing the fix gives no license to write it.
- "It is only three lines." No line count changes the role split.
- "I did this last time." A direct edit nobody corrected is a violation nobody
  flagged.
- "It is a doc or a skill file, not product code." Any repo file outside the
  orchestrator's .cortex/ working artifacts counts; the file type never changes
  the role split.
- "It is inside .cortex/, so it is my artifact." Only the orchestrator's own
  plan docs, memory and reports qualify. Hand-backs, feedback entries and
  scratch belong to the subagents that wrote them.

The boundary: if the next Edit/Write would touch anything outside the
orchestrator's own .cortex/ working artifacts (SKILL.md, "What the orchestrator
writes itself"), stop and dispatch.

## Steps

### Step 1: Pick the effort level

Pick the implementer's effort level, and the verifier's, from the task's shape.

REQUIRED: references/effort.md.

### Step 2: Dispatch one implementer with an inline brief

Start one implementer subagent. Its brief is the prompt itself, with the
requirements written into it (references/planning.md for brief content,
references/subagents.md for the brief skeleton).

REQUIRED: references/subagents.md, references/planning.md.

Tell the user the effort level you picked for each role, in the same turn as
the dispatch. With no spec doc, that message is the only record of the routing
decision.

### Step 3: Spawn the verifier when the implementer returns

When the implementer returns, spawn one fresh verifier subagent. Brief it with
the inline requirements you gave the implementer, the implementer's final
message, and the check that proves the task works.

REQUIRED: references/verifier.md.

If the implementer returns while no verifier is planned, spawn the verifier at
that moment. Reading the implementer's report yourself and pronouncing it
verified does the verifier's job without the verifier's independence.

### Step 4: Act on the hand-back

Read the verifier's hand-back and act on it. Re-run no gate. A thin hand-back
goes back to a verifier (references/verifier.md, "Thin hand-backs"). When the
verifier sends the task back, dispatch a fresh implementer with the verifier's
findings, then a fresh verifier.

### Step 5: Close the task

1. Remove any worktree the task created, after its work landed on a named
   branch (references/subagents.md, "Worktrees").
2. Write a memory entry when the task produced a decision, a user correction or
   a gotcha worth keeping (references/memory.md).
3. Report to the user: the effort levels used, what changed, the hand-back
   path, and anything left open.

Skip the plan directory and the formal spec. Never skip the effort choice, the
effort report to the user, the verifier, or verification.

Every gate or close-out step added to these steps is added to the full
lifecycle's loop in references/orchestrating.md in the same change, and the
other way round. A step that exists in only one workset gets skipped every time
the other workset runs.

## Artifacts

| Written | By whom |
|---|---|
| Inline brief with the requirements in it | orchestrator |
| Product and repo files | implementer |
| Hand-back at .cortex/reports/handbacks/<date>-<wave>.md | verifier |

The brief is a message, delivered inline as the subagent's prompt. Never write
it to .cortex/: no brief file, no spec doc, no plan doc. Requirements that feel
too heavy to send inline are the promotion signal below.

## Promotion to the full lifecycle

Switch to the full lifecycle (references/orchestrating.md) when any of these is
true:

- There is more than one deliverable, or sub-tasks for separate implementers.
- The implementer needs requirements it will re-read while working.
- There is a second track or a dependency between pieces of work.
- A third agent will enforce a fence on the implementer (a file it must not touch,
  a constraint it must keep).
- You expect to amend an instruction before the implementer finishes.

Promotion means a real plan directory: .cortex/plans/<date>-<epic>/ with an
index doc and one spec per task. A single file of requirements is the
artifact the reduced workset forbids, because it is a spec with none of the
plan's structure around it.
