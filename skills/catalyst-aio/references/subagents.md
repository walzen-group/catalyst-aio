# Working with subagents

Every implementer and every verifier in catalyst-aio is a subagent: a fresh
agent the orchestrator starts through the harness with one prompt, which works
until it returns a final message. This reference covers how to brief one, when
to run several at once, how to wait for them, how to correct or stop one, and
how to keep concurrent writers apart. It applies in any harness with a subagent
facility; Claude Code's Agent tool is one example.

## Harness requirement

Implementers and verifiers run through the harness's subagent facility. In a
harness without one, tell the user plainly that catalyst-aio cannot delegate
there, and ask whether to proceed in this session. Never drop the role split
silently.

When the user agrees to proceed in-session, run each role's steps in order in
this session: write the spec or brief first, then implement, then follow
references/verifier.md with the pass criteria written before you read the diff.
Say in the report to the user that one session played every role, so the
verification was not independent.

A user constraint on the model or the harness ("Claude only", "use the small
model for implementers") selects what the subagent runs. It never removes the
role split: the orchestrator still dispatches, and a subagent still writes the
code.

## What a subagent sees

A subagent starts blank. The prompt you give it is its whole world: it has not
read this conversation, the plan index, or SKILL.md unless the prompt sends it
there. A field the harness keeps for its own bookkeeping (a label, a
description) is invisible to the subagent, so a spec path written only there
leaves the subagent to guess which file to read.

Write every brief with these parts:

1. Role and name. Say whether it is an implementer or the wave's verifier, and
   give it a name after its task (for example "impl-retry-backoff" or
   "verify-wave-2"), unique among the subagents running now, so its report is
   attributable. Use the same name as the harness's label or description where
   the harness has one.
2. Load catalyst-aio. Where the harness has a skill loader, tell the subagent to
   load the catalyst-aio skill through it. Pointing the subagent at the raw
   SKILL.md path skips the harness's routing, so use the path only in a harness
   without a loader, and then give the absolute path.
3. The work. For the full lifecycle, the absolute path of its spec, for example
   /home/user/repo/.cortex/plans/2026-10-28-retry/task-2-backoff.md. For the
   reduced workset, the requirements written inline in the prompt.
4. References to read first. Name each catalyst-aio reference the subagent
   needs by absolute path, resolved from the directory that contains SKILL.md:
   references/verifier.md for a verifier, references/test-first.md for a fix,
   references/writing.md for anything a person will read. A subagent reads only
   what its brief names.
5. Effort level, and the model when the user's tier-to-model map names one
   (references/effort.md).
6. The files it may write. It writes only those. Scratch goes under the
   repo-local .tmp/.
7. The report: its format (references/planning.md) and where it goes. Every
   subagent returns its report as its final message; a verifier also writes it
   to .cortex/reports/handbacks/<date>-<wave>.md. The final message ends by
   restating the key evidence (files changed, gate commands and their results,
   the report path), so a reader that sees only the end of the message still
   has it.
8. The end of its duty. After the final message it runs no further commands.

Write the prompt inline at the subagent call. Never stage it in a scratch file
and have the subagent read it back: the staged copy drifts from what you meant
to send, and its path can be wrong.

When a brief passes the user's words on, quote them whole and put any context
you add outside the quote (references/planning.md).

## Before you launch

Check, for every subagent of the dispatch:

- the spec file exists at the absolute path the brief names
- the working directory the subagent will run in exists
- every subagent name is unique among the subagents running now
- a verifier is planned for the wave

A subagent started in the wrong directory, or on the wrong model or effort
level, gets stopped and replaced by a fresh one with the corrected brief.
Correcting it mid-run leaves its early work built on the wrong premise.

## Parallel dispatch

Run implementers in parallel only when their tasks are really independent:
disjoint files, and no task reading another's output. When the harness can
start several subagents in one batch (several subagent calls in one message,
for example), launch independent subagents that way. Tasks that depend on each
other go into separate waves.

## Waiting for a subagent

Some harnesses run a subagent in the background and tell the caller when it
finishes; others block the caller until the subagent returns.

- Background: never end your turn with a subagent in flight and nothing that
  will resume you when it finishes. Only a completion notice that your own
  harness delivers to your session can resume you. A wait started by anything
  else (a shell job, another agent) resumes nobody.
- No completion notice: dispatch blocking, so the call returns with the
  subagent's result.

Never wait with a shell sleep, and never sit in a foreground polling loop: a
blocking poll freezes the session while other subagents go unwatched. Wait on
the harness's completion notice.

Never launch an agent whose only job is watching other agents. The harness's
completion notice tells you when a subagent finishes, and the verifier checks
the work afterwards.

## Fresh context per task

Use a fresh subagent per task. Reuse a subagent's session only when two tasks
chain with no gap between them. Any wait makes its context stale, and a
verifier from an earlier wave applies that wave's assumptions to the next.

## Reading a subagent

Read a subagent through what the harness gives you: its final message, and its
transcript where the harness exposes one. Never tail, grep or parse the
harness's session files on disk.

A lookup that cannot see a subagent says nothing about whether the subagent
exists. When a harness surface that does not index subagents (a history
command, a session list for top-level sessions only) fails to find one, ask
through the harness's own subagent interface before you conclude the subagent
is gone.

A report that a message to a subagent failed is a claim. Check the subagent's
own record (its transcript, its next output) before you resend or declare it
lost, because a blind resend can make it do the work twice.

## Correcting a subagent

Where the harness lets you send a follow-up message to a running or returned
subagent, a correction names what the subagent is doing, why that is wrong, and
what to do instead. A soft "are you okay?" changes nothing.

After a correction, confirm the subagent acted on it: its next output follows
the new instruction. A correction that got swallowed leaves the subagent on the
stale plan.

A subagent that returns idle or blocked gets its last output read for the
reason. Then send a corrective follow-up, dispatch a fresh subagent, or
escalate to the user. Never wait on it silently.

When its context is poisoned, or a correction did not take, start a fresh
subagent with the corrected brief.

A subagent stopped by a usage or rate limit resumes on its own once the limit
resets. Never restart or re-dispatch over it: a restart loses the finished work
and hits the same limit.

## Consent prompts

A consent or trust prompt that names a scope you did not request (another
directory, a wider permission, a tool you did not plan) is never confirmed.
Stop and escalate it to the user with the exact text of the prompt. This
applies to prompts the harness raises and to questions a subagent relays.

## Worktrees

Concurrent implementers that would write the same files each get their own git
worktree:

1. Create each worktree before launching its implementer.
2. Set the implementer's working directory to its worktree.
3. Tell the implementer, in its brief, to use absolute paths inside its
   worktree for every file tool.

Teardown lands the work before removing the worktree:

1. The finished work lands on its own named branch, rebased onto the default
   branch's latest commit. Committing needs the user's permission
   (references/planning.md); without it, ask before this step.
2. Only then remove the worktree.

A worktree left on a detached HEAD loses its work when the worktree is removed.

## Effort and model on the dispatch

Name the effort level on every dispatch, and the model when the user's
tier-to-model map names one, in the harness's dispatch parameters where it has
them and in the brief where it does not. How to pick them is in
references/effort.md.

## Stopping a subagent

Stop a running subagent only when the user cancelled the work or approved the
stop. A subagent is finished when it returned its final message or the harness
reports it ended; anything else is live work.

To stop one:

1. Stop it through the harness's own stop or kill control.
2. Confirm it stopped: the harness reports it ended, and no process it started
   (a build, a test run, a server) is still running.
3. Tell the user what changed in the repo and recommend how to revert it.

An interrupt you sent and did not confirm is an unverified stop. Never report a
wave halted on one.

## Long-running agent habits, mapped to subagents

catalyst-aio has no long-lived agent sessions to look after. These are the
behaviors that replace them:

| Need | What you do |
|---|---|
| Run a delegate | Start a subagent with a full brief. |
| Learn that a delegate finished | Wait for the harness's completion notice, or for the blocking call to return. |
| Correct a delegate | Send a follow-up message where the harness supports it; otherwise start a fresh subagent with the corrected brief. |
| Check a delegate's progress | Read its final message or exposed transcript. |
| Retire a delegate | Let it return its final message; reuse none across a gap. |
