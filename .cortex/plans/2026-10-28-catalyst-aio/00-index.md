> **Status: COMPLETE** (2026-10-28)

# catalyst-aio: harness-neutral, single-skill distillation of catalyst

Execution: full lifecycle, `catalyst-v2-orchestrating-delegates`. Each
implementer receives only its own task doc plus the global constraints below.

## Goal

Build `catalyst-aio` in `/home/nixos/repos/catalyst-aio`: one Agent Skill that
carries the judgment of the full catalyst kit (the "teacher") and runs in any
agent harness that has a subagent facility. No herdr, no `c2d`/`c2m`/`c2r`, no
Node or bash tools, no self-improvement loop. Model routing is effort-based on
one model, with an optional tier-to-model map.

## Architecture

```
/home/nixos/repos/catalyst-aio/
  README.md                       (task 6)
  AGENTS.snippet.md               (task 6)
  .gitignore                      (task 6, contains .tmp/)
  skills/catalyst-aio/
    SKILL.md                      (task 2)  entry point, principles, roles, routing
    references/
      orchestrating.md            (task 2)  full lifecycle loop
      reduced-workset.md          (task 2)  small-task path
      subagents.md                (task 2)  harness-neutral delegation mechanics
      verifier.md                 (task 2)  the per-wave verifier role + hand-back
      planning.md                 (task 3)  index doc + task spec anatomy
      effort.md                   (task 3)  effort tiers, optional model map
      test-first.md               (task 4)  test-first procedure for fixes
      writing.md                  (task 4)  doc writing convention + condensed humanizer
      memory.md                   (task 5)  plain-markdown .cortex/memory
      feedback.md                 (task 5)  .cortex/feedback log for the teacher
```

The filenames above are fixed. Tasks link to each other's files by these names
before the files exist.

Tech stack: Markdown only. Agent Skills format (a `SKILL.md` with YAML
frontmatter, plus a `references/` directory).

## Source material (teacher)

- Teacher kit checkout: `/etc/nixos/nix/catalyst` (git HEAD `a00dd80` at plan
  time; two other agents are editing its skills for an opus-5-5 model rename, so
  the working tree may move. Read it; never write to it.)
- Skills: `/etc/nixos/nix/catalyst/skills/*/SKILL.md` (20 skills)
- Incidents: `/etc/nixos/nix/catalyst/.cortex/incidents/*.md` (88 reports)
- Kit memory: `/etc/nixos/nix/catalyst/.cortex/memory/`

## Resolved questions (user answers, 2026-10-28)

User's request, quoted: "create a slightly reduced version of catalyst that
takes all of the insights and allows it to run with any harness (generic
skills, no dispatch tools etc, only subagentic workflows etc). it's called
catalyst-aio. So model routing would be mostly effort-based on a single model
(optional), no dependency on herdr, and the self-improvement loop isn't
necessary as we would pull that in from the "teacher" parent full catalyst
suite."

| Question | User's answer |
|---|---|
| One skill or several? | One skill + references |
| Meta-agent under plain subagents? | Fresh verifier per wave: after a wave's subagents return, the orchestrator spawns one fresh verifier subagent that checks each diff against its spec and runs the whole-change check. Orchestrator still runs no gates. |
| Memory and failure recording? | Memory + upstream feedback file: plain-markdown `.cortex/memory/` written by hand (no c2m), plus a `.cortex/feedback/` log of failures the teacher catalyst can later harvest. |
| Packaging? | Plain Agent Skills dir + AGENTS.md snippet. (Not chosen: install.sh, Claude Code plugin manifest.) |

Inventory open questions (user answers, 2026-10-28, after wave 1):

| Question (inventory row) | User's answer |
|---|---|
| Plan directory at close-out (rows 60, 61) | Always keep. Plan dirs stay as the record of how each wave ran; memory extraction still happens; the user prunes plan dirs. |
| Who writes a `.cortex/feedback/` entry (row 69) | Fresh subagent per entry. The orchestrator hands the case (user's words verbatim, what happened, what should have happened) to a fresh subagent, which writes the entry; the orchestrator does not write or diagnose it. |
| Concurrency cap in the optional model map (row 145) | Default cap of 2 on the top tier, user-editable. |

Orchestrator decisions (not user-stated, recorded so delegates do not re-ask):

- A harness with no subagent facility: the orchestrator tells the user that
  catalyst-aio cannot delegate there and asks whether to proceed in-session.
  It never silently drops the role split.
- Activation gate stays: catalyst-aio runs only when the user names catalyst
  (or catalyst-aio).
- catalyst-aio agents never edit the catalyst-aio skill files. Improvements
  arrive from the teacher; failures go to `.cortex/feedback/`.

## Revision notes

- 2026-10-28, during wave 1: Global constraint 6 changed from a 250-line cap
  to "length follows content" on user correction (quoted in the constraint).

## Global constraints (every task includes these)

1. **Write only the files your task names.** Other workers write the other
   files in the same checkout at the same time.
2. **Harness-neutral text.** The skill files and README must not depend on or
   mention herdr, `c2d`, `c2m`, `c2r`, omp, panes, tabs, steer, wake commands,
   `A2A:`/`A2U:` markers, or `catalyst-v2-*` skill names. Exception: README's
   provenance section names the teacher kit. Name a harness's subagent tool only
   as an example ("Claude Code's Agent tool"), never as a requirement.
3. **Lessons as rules, without citations.** Teacher incident paths do not exist
   in catalyst-aio. State each kept lesson as a rule plus a one-clause why.
4. **Dropped machinery stays dropped:** herdr/multiplexer ops, the dispatch tool,
   quickchat, session save/resume, status board, curator, incident filing,
   self-testing, meta-agent instruction repair, kit-vs-project tree split.
5. **Agent Skills format.** `skills/catalyst-aio/SKILL.md` frontmatter:
   `name: catalyst-aio` (matches the directory), `description` of 1024
   characters or fewer, and `metadata` with `version: "0.1.0"` and
   `distilled-from: "catalyst a00dd80"`.
6. **Length follows content.** User correction, quoted: "slightly reduced
   doesn't mean less text in the skill.mds, it was more about the surrounding
   control mechanisms, so you may go above 250 when necessary". The reduction
   is the machinery (tools, herdr, self-improvement loop). Never trim or
   compress a judgment rule to save lines. Reference files have no line cap.
   SKILL.md stays under roughly 500 lines (Agent Skills guidance) by moving
   detail into references, never by dropping it.
7. **Writing style.** Follow the catalyst doc writing convention: invoke the
   `catalyst-v2-writing-docs` skill before your first write and run its Final
   grep over every file you wrote. README.md also gets the humanizer pass that
   skill mandates.
8. **Directory conventions catalyst-aio prescribes** (use these exact paths in
   the text): `.cortex/plans/<date>-<epic>/`, `.cortex/memory/` + `MEMORY.md`,
   `.cortex/reports/` (user-facing reports; verifier hand-backs under
   `.cortex/reports/handbacks/`), `.cortex/feedback/`, and repo-local `.tmp/`
   (gitignored) for scratch. Never `/tmp`.
9. **No commits.** Leave changes uncommitted. `git add` is fine.
10. **Inventory is the contract.** Wave 2 tasks implement the keep/adapt rows
    of `inventory.md` assigned to their files. A row you cannot place is a
    report item, never a silent drop.

## Tasks

| Doc | Task | Area | Depends on |
|---|---|---|---|
| task-1-insight-inventory.md | Inventory every teacher rule: keep / adapt / drop, target file | plan dir only | none |
| task-2-orchestration-core.md | SKILL.md, orchestrating, reduced-workset, subagents, verifier | skills/catalyst-aio | 1 |
| task-3-planning-and-effort.md | planning.md, effort.md | references | 1 |
| task-4-test-first-and-writing.md | test-first.md, writing.md | references | 1 |
| task-5-memory-and-feedback.md | memory.md, feedback.md | references | 1 |
| task-6-readme-and-snippet.md | README.md, AGENTS.snippet.md, .gitignore | repo root | 1 |

## Tracks

- Wave 1: task 1 alone.
- Wave 2: tasks 2 to 6 in parallel (disjoint files, shared checkout, no
  worktrees needed).

## Agent allocation (locked)

| Agent | Task | CLI | Model | Effort/thinking | Why |
|---|---|---|---|---|---|
| aio-t1-inventory | 1 | claude | claude-opus-5-5 | default | Contract-defining: every later file implements it |
| meta-aio-w1 | wave 1 meta | omp | opencode-go/deepseek-v4.1-flash | max | Meta default |
| aio-t2-core | 2 | claude | claude-opus-5-5 | default | Contract-defining: SKILL.md and the role split |
| aio-t3-planning | 3 | omp | opencode-go/deepseek-v4.1-flash | max | Mid-tier, spec'd |
| aio-t4-writing | 4 | omp | opencode-go/deepseek-v4.1-flash | max | Mid-tier, spec'd |
| aio-t5-memory | 5 | omp | opencode-go/deepseek-v4.1-flash | max | Mid-tier, spec'd |
| aio-t6-readme | 6 | omp | opencode-go/deepseek-v4.1-flash | max | Mid-tier, spec'd |
| meta-aio-w2 | wave 2 meta | omp | opencode-go/deepseek-v4.1-flash | max | Meta default |

Opus concurrency: one Opus delegate per wave (cap is 2).

## Pre-work

None. No status board for this effort.

## Out of scope

- install.sh, Claude Code plugin manifest, LICENSE choice.
- Any edit to the teacher kit at `/etc/nixos/nix/catalyst`.
- Installing catalyst-aio into `~/.claude/skills` or any harness on this machine.
- A sync tool from teacher to catalyst-aio.

## Whole-change verification (wave 2 meta)

Run from `/home/nixos/repos/catalyst-aio`:

```bash
# 1. no dropped-machinery vocabulary in the skill
grep -rniE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2' skills/ AGENTS.snippet.md
# expected: no output

# 2. every reference named in SKILL.md exists, and every reference file is named
ls skills/catalyst-aio/references/
grep -oE 'references/[a-z-]+\.md' skills/catalyst-aio/SKILL.md | sort -u
# expected: the two lists match exactly (10 files)

# 3. size
wc -l skills/catalyst-aio/SKILL.md skills/catalyst-aio/references/*.md
# expected: SKILL.md under ~500 lines; references uncapped

# 4. frontmatter
sed -n '1,/^---$/{p}' skills/catalyst-aio/SKILL.md | head -20
# expected: name: catalyst-aio; description <= 1024 chars; metadata.version, metadata.distilled-from

# 5. cross-links resolve: every references/*.md path mentioned in any file exists

# 6. style: catalyst-v2-writing-docs Final grep over all files; hits triaged per its table
```

Plus: diff against each task spec, and inventory coverage (sample at least 15
keep/adapt rows across files and confirm each landed as a rule).

## Smoke test (wave 2 meta)

Copy `skills/catalyst-aio/` to `/home/nixos/repos/catalyst-aio/.tmp/smoke/catalyst-aio/`.
Start a fresh subagent (any harness) whose only instruction source is that copy.
Prompt, verbatim: "Use catalyst to add a `--version` flag to a small CLI in
this repo. Before doing anything, tell me which files of the skill you read,
who will write the code, what effort level each role runs at, and what happens
after the implementer returns." Pass criteria, written before reading output:
it names SKILL.md and the reduced-workset reference; says a subagent writes the
code; gives an effort level per role; says a fresh verifier subagent checks the
diff and the orchestrator runs no gate. It mentions no herdr or dispatch tool.
