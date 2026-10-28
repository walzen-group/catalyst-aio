# Effort routing

Every role runs on the session's model by default. Effort is the dial: the reasoning depth a subagent gets, picked per task and named in the brief. A project that wants a cheaper model for mechanical work fills in the optional map at the end of this file.

## Effort levels

| Level | Task shape | Examples |
|---|---|---|
| low | Mechanical work whose decisions the spec already made | a rename the spec spells out, a config bump, a mirrored test, a doc edit against a fixed outline |
| medium | Several files, a clear brief, acceptance the delegate can check | implementing a written spec across files, a refactor to a named shape, a per-task review, a verifier running a check whose pass condition is written |
| high | Open-ended work where the delegate works out what to do | debugging an unknown cause, designing an approach, environment bring-up, a check that has to decide what the right test is, a final review |
| max | Only on the user's explicit directive | a task the user asked to run at maximum effort |

Levels above high need the user's directive. The orchestrator may assign low, medium, or high from its own judgment; max waits for the user to ask for it by name. A harness that offers a setting above high falls under the same gate.

## Role defaults

| Role | Default | Moves up when |
|---|---|---|
| Orchestrator | the session's own setting | never; it is not dispatched |
| Implementer, mechanical task | low | a failed run; re-dispatch one level up |
| Implementer, spec'd multi-file task | medium | the brief cannot state the approach, so the delegate designs it: high |
| Implementer, open-ended task | high | the user directs max |
| Verifier | medium | the check itself needs judgment, such as an unknown cause or cross-cutting behavior: high |
| Per-task review | medium | the review must judge a design no spec covers: high |
| Final review | high | the map's top model when the project keeps one |

The orchestrator keeps the strongest model and the highest effort the session offers, and spends neither on delegated work: planning, routing, and the close-out run in that session.

A review or a verification runs at or above the level of the work it judges. Checking a low-effort chore runs at medium; checking a spec'd change runs at medium, and moves to high when the check is open-ended.

## Picking a level

1. Read the task's shape and pick its level from the table above.
2. Pick a model from the map when the project filled one in.
3. Name the level in the brief, and the model when the map supplied one.
4. Report the decision to the user at dispatch.

## Start low, escalate on evidence

Most delegated work is spec'd implementation at medium or mechanical work at low. High is an exception a task earns by being open-ended; a brief that states exactly what to change is not high, however important the change.

A delegate pitched too low gets re-dispatched one level up after it fails and reports why. Run the stuck-delegate order in references/orchestrating.md before that re-dispatch: fix an ambiguous spec, fix a broken environment, split an oversized task, and only then raise the level. A stronger run on a bad spec fails the same way, and a better spec lets a lower level succeed.

A better spec does not lower the level that judgment work needs. An unknown cause stays high after a perfect spec, because the delegate still has to find the cause. Debugging, verification, and environment bring-up take the level their investigation needs.

When a wrong answer would compile, verify the output. A transcription task (copying a value, a flag, a block) gets a diff against its source, which catches the error and keeps the task at low.

## Lock the allocation

Write the effort allocation into the plan index before the first dispatch (references/planning.md), and leave it alone while the wave runs. Re-route only when a delegate demonstrably cannot meet the bar after the retries above.

## Reporting the decision

Every dispatch report names the effort level per role, and the model when the map is in use. In the reduced workset that message is the only record of the routing decision, because no plan doc exists. In the full lifecycle the plan's effort allocation is the record, and the dispatch message restates it so the user can correct the choice before the work lands.

## Harnesses with no per-subagent effort setting

When the subagent facility exposes no effort setting, say so in the dispatch report, run the subagent at the harness default, and state the intended level in the brief. The report keeps the decision visible, and the brief tells the delegate what was asked for. Never claim an effort level the harness did not apply.

## Optional model map

Model selection is optional. The table starts with an empty model column, and an empty column means every role runs the session's model. Fill it in the copy of this file the skill installation uses when the harness lets a subagent run a different model.

| Tier | Model | Concurrent subagents |
|---|---|---|
| top | | 2 |
| middle | | none |
| small | | none |

Match the tier to the level the task needs: top for high, middle for medium, small for low. The top tier ships with a cap of 2 concurrent subagents, a default the user can edit. Work beyond a cap either queues or runs on the next tier down at the level its task still needs. The middle and small tiers have no default cap.

Name a model by the exact string written in the map. A generic alias can resolve to another model after a provider update, and the work then runs on a model the user did not choose.
