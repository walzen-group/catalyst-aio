# Memory

`.cortex/memory/` is the project's durable knowledge. A fresh agent session reads it to recover the decisions, user corrections, and gotchas that earlier sessions recorded. Reading needs no permission or gate.

## Layout

`.cortex/memory/MEMORY.md` is the index. It has one line per entry file, in this exact form:

```
- <file>.md - <description>
```

The filename is bare: no directory, no backticks, no wikilinks. The description is one plain-prose sentence. It contains no wikilinks, no em or en dash, no ` - ` sequence, and it does not open with "See". The one shape lets a reader scan the index and update a line without guessing.

Each fact cluster gets its own file, `.cortex/memory/<type>-<slug>.md`, with frontmatter:

```
---
name: project-token-rotation
description: Session tokens rotate in place every 24 hours.
type: project
date: 2026-10-28
---
```

Match the name to the file stem, and the description to the sentence on the index line. The date records the day the fact was written, in ISO form. The type picks the prefix and says what the entry records:

| Type | What it records |
|---|---|
| project | decisions with their rationale, the people who made them, and the date |
| feedback | user corrections and the workflow preference behind them |
| reference | non-obvious tooling, repository layout, and external systems |

The body states the fact, its reason, and its provenance: who decided it, when, and what it replaced.

## What belongs

Keep a fact the repository cannot derive:

- Decisions with rationale: what was chosen, when, by whom, and why. A decision that overrides an earlier one belongs here with the earlier choice named, so a reader does not follow the old path.
- User corrections and preferences that change how work is done.
- Gotchas that burned real time: a tool quirk, a false green test, an environment trap.
- Pointers to external state: a dashboard URL, a ticket number, an account name.
- A settled answer to a question the project will ask again.

The test is rediscovery: would a fresh agent otherwise have to learn this from code, logs, or a person? If yes, write it down. If the repository's code or docs already say it, leave it there and link the file.

## What stays out

- Facts derivable from the code, the repository docs, or git history. A restatement goes stale the moment the code changes.
- Ephemeral task state. The plan directory and the run report record that.
- Secrets, tokens, and private keys.
- Catalyst-aio process lessons. How the orchestrator, implementer, or verifier should behave belongs in the skill or in the feedback log (`references/feedback.md`); project memory keeps project facts, unless the user sanctions one process entry.
- A rule already stated in the skill. The skill is the single home, and memory does not restate it. Delete an entry whose remaining content is a pointer at the skill.

Two files that state the same fact drift apart at the first edit, and a reader who finds both cannot tell which one is current.

## Write discipline

Capture at decision time, by hand. A later consolidation pass loses the details that made the decision clear.

1. Search the store for an existing entry on the fact.

   ```
   grep -rn "<distinctive phrase>" .cortex/memory/
   ```

   Expected result: the entry file and line, or no output. A match means step 2 updates that file.

2. Write the entry file or edit the existing one. Keep one file per fact cluster; never create a second file for a fact an entry already states.

3. Add the index line, or update it when the file changed. The description in the index and in the frontmatter stay the same sentence.

4. Date every relative fact absolutely. Write "the 2026-10-14 deploy", never "last week", because the reader may read the entry months later.

5. Delete an entry that turned out wrong, together with its index line. Rewrite a superseded decision in place, naming the new decision and its date. Contradictory memories are worse than none, because a reader has no way to tell which one is current.

Memory files live only inside `.cortex/memory/`. A copy at the repository root or in a harness directory drifts from the store and gives the next session a second answer. The harness pointer below is the only file outside the tree; it states where the store lives and repeats no fact.

## Adoption

A project that already has agent memory moves it under `.cortex/memory/` in one change.

1. Create the store and the index.

   ```
   mkdir -p .cortex/memory
   touch .cortex/memory/MEMORY.md
   ```

2. Move every existing memory file into `.cortex/memory/`.
3. Merge the old index lines into MEMORY.md, rewriting each to the exact `- <file>.md - <description>` form.
4. Delete the old memory directory in the same change.
5. Check that no other memory directory is still live.

Expected result: one memory location, named in MEMORY.md.

Two live memory locations are the failure to avoid. The next session reads one and writes the other, and the two stores drift apart.

### Harness memory

Some harnesses keep file memory of their own. Leave one `reference-*` file in that harness's memory directory, stating that `.cortex/memory/` is the memory of record and that all new memory goes there. The pointer contains no facts and no index; it sends the reader to the store. Every real entry is written under `.cortex/memory/`.

## Close-out extraction

At a plan's terminal status, the close-out step in `references/orchestrating.md` extracts the plan's durable signals into memory. The orchestrator writes these entries by hand, in the same pass that sets the status.

Candidates:

- Settled decisions, with the user's answer and the reason.
- Resolved questions from the plan's index document.
- Gotchas a delegate hit and reported.
- User feedback given mid-flight that changes a later choice.

The plan's run artifacts and any fact the repository can derive are not candidates. Gate output, wave logs, and diffs stay in the plan directory.

The plan directory stays after extraction. It records how the waves ran, and the user prunes plan directories when they stop being useful. Memory keeps the facts; the plan directory keeps the history.

## Manual review

No automated pass keeps the store current. A person reads it at each effort close and prunes by hand. Run the same review when the index stops answering a question quickly.

1. Read MEMORY.md and every entry file.
2. Judge each entry by whether a fresh agent would consult it in the next effort. Relevance beyond the closing effort decides; an entry this effort never touched can still be the reason the next session avoids a trap.
3. Keep an entry that the repository's own agent instructions point readers to. Removing it strands a pointer those instructions depend on, and the next review adds the entry back.
4. Delete entries that stopped proving useful or that later work disproved, with their index lines. Merge duplicate files and rewrite stale entries to the current decision.
5. Fix index lines whose description no longer matches the file.

Expected result: every index line matches a file, every file states a fact a reader would consult, and no two files answer the same question.

Pruning is a judgment a reader makes from the entries and the repository, not a count of how often an entry was opened. The store has no scores, no pins, and no tombstone directory; a deleted entry is gone.

## Committing

The memory files sit in the checkout and travel with it. Whether a change is committed is the user's call; ask, the same as for any other repository change.
