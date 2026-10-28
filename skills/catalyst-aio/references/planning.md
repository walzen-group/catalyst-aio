# Planning artifacts

A plan produces two artifacts in order: the index doc, which describes the whole change, and one task spec per delegated piece. Write the index first, because every spec inherits its global constraints and its settled answers. Both live in a plan directory, `.cortex/plans/<date>-<epic>/`, which contains `00-index.md` and one `task-N-<slug>.md` per task. The date is the day the plan opened; the epic is a short slug for the change.

## Plan directory

Use the directory for every multi-task effort. Work that fits the reduced workset gets an inline brief and no plan directory (references/reduced-workset.md); a second deliverable, a second track, or a fence a third agent enforces moves the work here.

State the paths in every brief. A project that also uses another skill suite usually has that suite's defaults for plans and reports, and a delegate that reads those instructions writes where the other suite says. The brief names the `.cortex/plans/` directory, the `.cortex/reports/` path for a deliverable, and the `.tmp/` location for scratch, so the delegate never has to choose a path.

## Index doc

| Section | States |
|---|---|
| Status line | the plan's state, on the first line |
| Header note | which workset runs the plan, and that each implementer receives only its own task doc plus the global constraints |
| Goal | the outcome, in a short paragraph |
| Architecture | the file layout the change produces, with the task that writes each file |
| Tech stack | only the toolchain facts that constrain the work |
| Source spec and resolved questions | a link to the design doc when one exists, and every question the user answered with the answer |
| Revision notes | one dated block per change made after the plan opened |
| Global constraints | invariants every task includes, written once |
| Task table | doc, task name, area, dependencies |
| Tracks | parallel groupings; a track is the unit of one dispatch wave |
| Effort allocation | task to effort level, plus the model when the project keeps a map (references/effort.md) |
| Out of scope | explicit non-goals |
| Whole-change verification | exact commands from the repo root, in pinned toolchains, with the expected output |
| Smoke test | the end-to-end scenario, the exact prompt for a fresh agent, expected observable output, and pass criteria written before the output is read |

### Status line

The first line of `00-index.md` is `> **Status: ACTIVE** (2026-10-28)`, with the day the plan opened. A plan kept in one file writes `**Status:** ACTIVE (2026-10-28)` in its header block instead. Set the line when the plan is created: a plan without one never gets a status at close-out. Task docs have none; the index line covers the plan.

At close-out the orchestrator replaces ACTIVE with one terminal token: COMPLETE, DONE, CANCELLED, SUPERSEDED, or ABANDONED. A qualifier that reopens work, such as `COMPLETE - INTEGRATION OPEN`, is not terminal. Leave the plan ACTIVE until the remaining work is done or the plan is cancelled; a terminal line is what starts memory extraction (references/memory.md) and tells the reader the plan is closed.

### Rules for the index

- Plan docs are the source of truth. When a brief and the plan disagree, the plan wins and the brief gets corrected.
- Every acceptance criterion must be satisfiable by the listed tasks. A criterion that no task meets is a planning bug, found before dispatch.
- Verify where the subject runs in production. A check environment that cannot run the subject verifies nothing; the index names the toolchain and its version per repo.
- Record every question the user answered, with the answer they gave. Mark orchestrator decisions as decisions, so a delegate can tell a user statement from an inference. A delegate that finds a question already answered does not re-ask it.
- Revision notes quote the correction that forced the change, with its date.
- A user-facing deliverable is a report, not a run artifact. An audit, findings, or recommendation the user will read goes to `.cortex/reports/<date>-<slug>.md`, written under references/writing.md. Prose drafts and gate evidence stay in the plan directory; scratch scripts, logs, intermediate JSON, and bulk or binary data go to the repo-local `.tmp/`, which is gitignored on first use, never to `.cortex/` or `/tmp`.
- Names a plan proposes for user-visible things (a renamed file, a skill, a slug) are the user's taste call. Settle them before dispatch and record the chosen name and the reason in the task doc. A delegate dispatched on the orchestrator's proposal churns the rename again when the user overrides it.
- Global constraints state each task's exact report path and the `.tmp/` location when the task writes scratch; each delegate's report repeats those paths.
- Effort allocation is written before the first dispatch (references/effort.md). Changing it while a wave runs needs a failed run behind it.

## Task spec

A delegate starts blank: no conversation history, no memory of the plan. Its spec doc is the whole world it works from, and a reference the spec does not name does not exist for it. Write one task per spec; two tasks in one spec get about 70% quality on both. A spec has six parts, and a Report-to section closes it.

### Anatomy

| Section | Contents |
|---|---|
| Context | two to four sentences: which system, why the task exists, where it sits in the larger change |
| Target | exact files, symbols, and directories to touch, plus explicit non-goals |
| Change | step-by-step add, remove, rename: APIs, signatures, patterns (point at an existing example), exact constants and names, code sketches where shape matters |
| Constraints | the plan's global constraints, copied verbatim |
| Acceptance | exact commands and what green looks like, a negative check where useful, and the verification burden |
| Emission discipline | a survey budget in minutes, then a file or report write about every ten minutes; long turns with zero writes stall invisibly |
| Report-to | where the result goes, and the duty that ends there |

### Spec rules

- Self-contained. Paths are absolute or relative to a root the spec states, and facts are quoted. A delegate cannot open a reference the spec does not name.
- Carry the symptom, never your hypothesis. State the user's observed symptom in their words, the trigger, when it started, and what the system did before. Require the delegate to verify the mechanism before changing anything. Label a suspected mechanism as a hypothesis to confirm or kill. A wrong system model in a spec is unrecoverable: the delegate applies the fix faithfully to the wrong cause.
- Establish environmental premises. Each claim about the running environment states its evidence, a command and its output, or the spec makes establishing it the delegate's first step. Mark each premise observed or inferred. The user's words are testimony; where the claim is checkable in the repo or on the machine, the check is the evidence. Never restate a claim about the user's machine as a claim about a file in the repo. Name the consumer: infrastructure with no consumer is speculative scaffolding, and the user decides whether to build it. Never fence an unevidenced premise with "settled research, do not re-investigate".
- Include the why for taste calls. Naming, file layout, and any choice the delegate could reverse on its own get the principle behind it.
- State the verification burden. Name the tests to run and the project-wide checks not to run, so the delegate does not run the full suite and report noise.
- Gate acceptance on the effect. A config that parses proves the delegate typed it, not that the system uses it. An assembly-level test through argv or a fake invoker does not exercise the integration path; a live run is the proof. Where no gate can observe the purpose, name the experiment that would, and record the gap in the spec.
- Acceptance criteria are inviolable. A blocker is a report with the criteria intact. Never author a fallback that lets the delegate delete the task's observable purpose.
- Report a diff, never a commit. The report lists files changed, the `git diff --stat`, gate output, and deviations; Report-to says where it goes. Changes stay uncommitted unless the user authorized a commit, and `git add` is fine.
- A task whose deliverable is a report the user reads states the exact `.cortex/reports/<date>-<slug>.md` path in its Target and its Report-to, so the delegate cannot file it in the plan directory.
- Append-only git on a shared checkout. When several workers share one checkout and commits are authorized, Constraints state the rule: the branch tip moves forward only by adding commits. `git reset` in any mode, `git rebase`, `git commit --amend`, and history reordering are forbidden, including on the worker's own unpushed tip. A commit that needs redoing stays in place and is reported with a proposed follow-up commit. A worker that rewrote history is stopped, and a delegate the orchestrator dispatches repairs the branch.
- A fix spec states the test-first steps: write the test, run it against the unwanted behavior and record the failing run, implement the minimal fix, run the test to green. Point at references/test-first.md and require the red run in the report.
- Every spec includes the writing convention by path: references/writing.md. A delegate never reads SKILL.md, so a style rule stated only there misses it.
- Quote the user's words whole when a spec includes them. No summarizing, reordering, or dropped qualifier, and your own context goes outside the quote. Never place your research, note, or proposal inside the quoted block: the delegate reads the quote as the user's statement and everything else as yours.
- State each rule once, plainly, at the step where the delegate acts. Narrative scaffolding around a rule dilutes it, and the delegate skips rules buried in prose.
- An example gets copied literally. Show the exact value: the real path, the exact model string, the command that runs. A generic placeholder in an example comes back in the work.

### Report-to

Every spec ends with Report-to. For an implementer the recipient is the orchestrator, and the report is the subagent's final message: files changed, the `git diff --stat`, the gate commands with their results, the red run for a fix, deviations, and the paths the spec named. A task whose deliverable the user reads also writes that report to the path its spec names. A verifier sends its final message and writes `.cortex/reports/handbacks/<date>-<wave>.md` (references/verifier.md).

The implementer's duty ends at that message. Afterwards it runs no further commands, does not read another agent's state, waits for no acknowledgment, and stops. Checking delivery and chasing a reply is the orchestrator's work; the implementer states the result once, accurately, and then goes idle.
