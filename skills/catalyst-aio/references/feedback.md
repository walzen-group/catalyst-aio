# Feedback

`.cortex/feedback/` records catalyst-aio's own failures and the user's complaints about how it behaved. The log exists for the teacher kit that maintains catalyst-aio: the teacher reads the entries and turns them into skill repairs. A catalyst-aio agent never edits the skill to fix a failure it recorded. The repair comes from the teacher, and until it does, the same failure can happen again.

## When to write an entry

Write one when any of these applies:

- The user corrects or complains about how catalyst-aio behaved, or asks for the failure on the record. The complaint counts even when the behavior is already fixed.
- A rule in the skill was wrong, missing, or ambiguous in practice.
- A harness limitation broke a rule the skill states.

The test behind a rule gap or a harness limitation: does a rule in the skill own the failure, or would a fresh agent reading the same text repeat it? A yes to either question makes the case fileable, even when nobody complained.

A failure the user corrected in passing, with no instruction text behind it and no request to record it, stays in the run report. It gets no entry.

A complaint that an entry was skipped is itself a feedback case. The new entry records the skipped record as part of what happened; it is not a quiet backfill of the original failure.

### Feedback or project memory

Whether a given item is feedback or memory is the user's decision: ask, and write the record they name. A complaint about how catalyst-aio behaved belongs in feedback. A project decision belongs in memory (`references/memory.md`). When one event is both, write the feedback entry and ask whether the project fact also goes to memory.

A memory note never stands in for an entry the user asked for. The entry is the record the teacher harvests; memory is the record the project's agents read.

## Path and recurrence

One file per occurrence, at `.cortex/feedback/<YYYY-MM-DD>-<slug>.md`. The date is the day the failure happened, and the slug names the failure in a few hyphenated words, for example `2026-10-28-verifier-skipped-diff.md`.

Before writing, scan the log for an earlier entry on the same failure.

```
grep -rni "<distinctive phrase>" .cortex/feedback/
```

Expected result: no match, or the path to an earlier entry.

A match makes this entry a recurrence. Add `Recurrence: <path to earlier entry>` to the new entry and state in "What happened" that the earlier fix did not take. Leave the earlier entry unchanged; the new entry records the new occurrence.

## Who writes the entry

A fresh subagent writes each entry. The orchestrator hands it the case and does not write the entry, diagnose the failure, or plan a repair itself, because an agent cannot audit its own conduct.

The handover states the user's words verbatim, what happened, what should have happened, and where the evidence lives. The orchestrator sends this brief:

```
Write one feedback entry for catalyst-aio at .cortex/feedback/<YYYY-MM-DD>-<slug>.md.

Failure date: <YYYY-MM-DD>
User's words, verbatim: "<quote>"
What happened: <the sequence, with roles and commands>
What should have happened: <the expected behavior, and the rule that covers it, or "no rule covers this">
Evidence: <paths to plans, reports, transcripts, diffs, and command output>
Role that failed: <orchestrator, implementer, or verifier>
Harness and model: <harness> / <model>

Read references/feedback.md in the installed catalyst-aio skill and follow its entry template and recurrence check. Write only the one entry file. Record facts and name the rule involved. Do not edit the skill, do not diagnose the cause beyond naming that rule, and do not plan a repair.
```

The subagent records facts and names the rule involved. The teacher performs the deeper diagnosis at harvest, so the entry needs no root-cause theory.

## Entry template

```
# <short sentence naming the failure>

Date: <YYYY-MM-DD>
Catalyst-aio version: <metadata.version from SKILL.md>
Harness and model: <harness> / <model>
Role that failed: <orchestrator, implementer, or verifier>
Recurrence: none | <path to the earlier entry>
Status: open

## User's words

"<the user's words in full, quoted verbatim; nothing paraphrased, nothing added>"

## What happened

<the sequence of events, with the role, command, and output that show each step>

## What should have happened

<the behavior the user expected, and the rule that covers it>

## Skill file and section

<SKILL.md or a reference file, plus the section name> | none: missing rule

## Evidence

<paths, commands, and output, quoted literally>

## Suggested change for the teacher

<one or two sentences naming the rule to add or correct, or "none; the teacher diagnoses at harvest">
```

The filename and the heading both name the failure. The version comes from the metadata block in SKILL.md.

When a case has no user wording, such as a rule gap found during a review, the writer puts "No user wording; found by <role> during <task>" in that section. Never invent a quote.

When the failure also damaged the project, state the damage in concrete terms in "What happened": the file, the branch, the work lost. The teacher reads that alongside the rule.

## Status

An entry starts as `open`. The teacher sets it to `harvested` after it files the repair that acts on the entry. Leave harvested entries in the log; the log is the audit trail, and an entry deleted after harvest removes its evidence.

## After writing

Report the entry path to the user in the writing convention (`references/writing.md`). Say that the record exists and that the skill still contains the rule that failed. The lesson binds only after the teacher lands the repair; until then, a fresh session can repeat the failure.

No agent in the project edits the skill, opens a repair task, or folds the case into a plan. The teacher decides whether a repair is warranted and owns the change.

A lesson about how catalyst-aio works stays in the feedback entry for the teacher. Do not copy it into project memory, where it would restate skill content. When the same event also settles a project fact, record that fact separately in memory (`references/memory.md`).
