# Verifier

The verifier is a fresh subagent the orchestrator spawns once per wave, after
every implementer of that wave has returned. It confirms that each implementer's
recorded gate output is genuine, reads each diff against its spec, runs the
whole-change check from the plan index, and writes a hand-back the orchestrator
audits. It does no task work. The first half of this file is for the
orchestrator, which briefs the verifier; the rest is the verifier's procedure.

## When the orchestrator spawns one

- Spawn the verifier after the last implementer of its wave has returned. A
  verifier started earlier reads half-finished work as the result.
- Spawn exactly one verifier per wave. Before spawning, check whether one is
  already running for that wave; two verifiers on one wave produce two
  hand-backs that can disagree, and neither is the record.
- Every wave gets its own fresh verifier, retired at its hand-back. Never reuse
  a verifier across waves: its context still has the earlier wave's
  assumptions when it checks the next one.
- While the verifier runs, nobody edits the checkout, the orchestrator
  included. The orchestrator edits its own artifacts again after the hand-back.
- A completion notice, or an implementer's own "done" line, is a cue to verify.
  It proves nothing about the work.

Pick the verifier's effort level from references/effort.md.

## Brief

The orchestrator's brief to the verifier names:

- each task's absolute spec path (or, in the reduced workset, the inline
  requirements the implementer received)
- each implementer's final message, quoted whole
- the whole-change commands from the plan index (or, in the reduced workset,
  the check that proves the task works)
- the hand-back path: .cortex/reports/handbacks/<date>-<wave>.md
- the absolute path of this file and of references/writing.md, plus
  references/test-first.md when any task is a fix

Constraints the orchestrator set are relayed to the verifier verbatim and never
softened.

## Procedure

### Step 1: Write the pass criteria

Before reading any output you are judging, write down what passes for each task:
the acceptance criteria from its spec, and the whole-change result the index
expects. Criteria written after reading the output bend toward what the output
shows.

### Step 2: Confirm nothing is still running

Before verifying in an implementer's checkout, confirm nothing the implementer
started is still running: no build, test run or server in flight. A build still
writing beside a git checkout has produced a false failure before.

### Step 3: Read each implementer's recorded gate evidence

For each task, read the implementer's recorded gate evidence: its transcript
where the harness exposes one, the gate output it reported, and the diff
showing the test ran and was not skipped. The record is the confirmation. Do not
re-run each implementer's gates; each implementer runs its own spec's gates and
nothing outside its task.

For a fix, confirm a recorded failing run exists before you accept the green
run (references/test-first.md). A green run with no failing run before it does
not confirm a fix, because a test that never failed proves nothing.

### Step 4: Read each diff against its spec

Read each implementer's diff against its spec: every acceptance criterion met,
no file outside the spec's target touched, no acceptance criterion descoped.

### Step 5: Run the whole-change check

Run the whole-change commands from the plan index, in the toolchain the plan
pins. Record each command and its output.

When the wave used worktrees, run `git status --short` on the shared checkout.

Expected result: no output. A file listed there was written outside the worktree
it belonged in.

### Step 6: Write and return the hand-back

Write the hand-back (format below) to .cortex/reports/handbacks/<date>-<wave>.md
and return the same text as your final message.

## Writing real files during verification

A verifier that needs to write real files (a failing test and its fix, a
reproduction) works in its own worktree or in a scratch copy under .tmp/. It
never writes into the wave's shared checkout, where its files would read as an
implementer's work.

## Smoke tests

When the plan asks the verifier to test whether an agent follows guidance:

- Ask the test subagent for the decision or the artifact. Asking it to recite
  the rule tests its memory of the text.
- Give the test subagent only the live skill files. A test subagent that has
  read a plan doc or a report about the change it is testing gives a
  contaminated answer; discard that answer and rerun with a fresh subagent.
- Write the pass criteria before reading its output (step 1).

## Holds

A verifier places a hold when it finds something the orchestrator must not act
on without the user: a gate that cannot be satisfied, input of unknown origin,
a request to relax a constraint. A hold is released only by the user's explicit
yes.

An implementer's request to relax a constraint goes into the hand-back in the
implementer's own words, as a hold. The verifier never grants it.

## Recording failures

The verifier is an independent auditor. It records failures accurately,
including faults in the orchestrator's own spec or brief.

A routine correction (a missed file, a gate the implementer ran wrongly and then
fixed) goes in the hand-back. A failure of catalyst-aio's own guidance is a
separate decision: the hand-back names it under open items, and the orchestrator
routes it per references/feedback.md.

## Hand-back

The hand-back follows references/writing.md and opens with the verdict. It has
these sections, each required:

| Section | Contents |
|---|---|
| Files changed | every file the wave changed, per task |
| Per-task diff verdict | for each task by name: pass, fail, or sent back, with the spec criterion behind the verdict |
| Gate evidence | for each task, the path to the recorded gate run, or the transcript location, that the verifier read; each path resolves to an artifact that exists |
| Whole-change output | each whole-change command and its output |
| Open items and holds | repairs still needed, holds for the user's yes, constraint-relaxation requests quoted, failures of catalyst-aio's guidance |
| Deliverable paths | the reports under .cortex/reports/ the user should read; an empty list is valid |

The hand-back names every implementer of the wave and, for any that stopped
early, why it stopped. Never close out with an implementer unaccounted for.

Verification of follow-up code belongs to the verifier of the wave that writes
it. When the hand-back lists repairs that are still product code to write, it
names that later wave's verifier as the owner and states the criteria to run.
It never assigns verification to the orchestrator, which runs no gate.

## Thin hand-backs

A hand-back is thin when a required section is missing or empty without saying
why, a claim cites no observed output, a gate evidence path does not resolve,
or a section assigns verification to the orchestrator.

A thin hand-back goes back to a verifier, never to the orchestrator's own gate
run. Where the harness lets the orchestrator send a follow-up message to the
same verifier, it names the gap there. Otherwise it spawns a fresh verifier
whose brief names the gap and the earlier hand-back's path.

## Sending a task back

A verifier may send a task back when its diff fails its spec. The hand-back
states the failing criterion and what the verifier observed. The orchestrator
then dispatches a fresh implementer with the verifier's findings and the
original spec path, and after that implementer returns, spawns a fresh verifier.
