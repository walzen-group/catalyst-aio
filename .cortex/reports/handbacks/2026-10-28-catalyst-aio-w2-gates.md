# Wave 2 gate evidence: catalyst-aio

Meta-agent meta-aio-w2. Work under verification: `skills/catalyst-aio/`,
`README.md`, `AGENTS.snippet.md`, `.gitignore` in
`/home/nixos/repos/catalyst-aio`.

Criteria were written before the corresponding output was read; results follow
each one. Worker gates were read from their hand-backs and not re-run. The
whole-change check is this meta's own run.

## 1. Whole-change verification (index section "Whole-change verification")

Run from `/home/nixos/repos/catalyst-aio`.

### Check 1: no dropped-machinery vocabulary in the skill

```bash
grep -rniE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U|catalyst-v2' skills/ AGENTS.snippet.md
```

Result: exit 1, no output. PASS. README is outside this check by design; its
only mention is the provenance left-out list (see open items).

### Check 2: reference list matches

```bash
ls skills/catalyst-aio/references/
grep -oE 'references/[a-z-]+\.md' skills/catalyst-aio/SKILL.md | sort -u
```

Result: `diff` of the two lists is empty (MATCH). Both lists hold the same ten
files: effort, feedback, memory, orchestrating, planning, reduced-workset,
subagents, test-first, verifier, writing. PASS.

### Check 3: size

```bash
wc -l skills/catalyst-aio/SKILL.md skills/catalyst-aio/references/*.md
```

Result:

| File | Lines |
|---|---|
| SKILL.md | 274 |
| references/effort.md | 73 |
| references/feedback.md | 114 |
| references/memory.md | 135 |
| references/orchestrating.md | 258 |
| references/planning.md | 86 |
| references/reduced-workset.md | 118 |
| references/subagents.md | 218 |
| references/test-first.md | 134 |
| references/verifier.md | 166 |
| references/writing.md | 406 |
| total references | 1708 |

SKILL.md is 274 lines, under the roughly 500-line guidance. References are
uncapped per Global constraint 6. PASS. No sampled rule is abbreviated for
length; each lands in its target file (section 2).

### Check 4: frontmatter

Result: `name: catalyst-aio` (equals the directory name), `description` 781
characters on one line (limit 1024), `metadata.version: "0.1.0"`,
`metadata.distilled-from: "catalyst a00dd80"`. The block parses as YAML: keys
`name`, `description`, `metadata`; `metadata` holds `version` and
`distilled-from`. PASS.

### Check 5: cross-links resolve

Every `references/<name>.md` path named in `skills/`, `README.md` and
`AGENTS.snippet.md` was tested for existence. Result: ten paths, ten OK, no
missing. PASS.

### Check 6: style

`catalyst-v2-writing-docs` Final grep over all eleven skill files, README.md and
AGENTS.snippet.md. Result: 38 hits in four files, all triaged as sanctioned:

| File | Hits | Triage |
|---|---|---|
| writing.md | 27 | the pattern list, bad examples, the triage table and the grep pattern itself, each a quoted banned term in a code span; the file's own triage table says leave them |
| SKILL.md | 3 | `does not hold` (a reading staying true), `is a hold` (a stop awaiting the user's yes), `Hold it` (retain text); none reports data being somewhere |
| orchestrating.md | 3 | `A hold the verifier placed`, `a hold from the verifier`, `is held and quoted`; the same governance noun |
| verifier.md | 5 | `## Holds` and four uses of the same governance noun |

No em dash, en dash, cost metaphor, closer, triage-banned AI word or empty-shape
verb stands in prose outside those quotations. README.md, AGENTS.snippet.md and
the remaining reference files have zero hits. PASS.

## 2. Inventory coverage sampling

Thirty-eight rows were sampled across all ten files; every one lands as a rule
in its target file. Full sampling list:

| Row | Class | Target | Landed (evidence) |
|---|---|---|---|
| 1 | adapt | SKILL.md | "When catalyst-aio applies", by-name gate |
| 4 | adapt | SKILL.md | routing table plus "read the matching reference before any orchestration action" |
| 5 | keep | SKILL.md | "Ground truth is the user's word" |
| 23 | adapt | SKILL.md | "Writing for the user" points at references/writing.md with the short form |
| 41 | keep | orchestrating.md | Step 1 Understand |
| 60 | adapt | memory.md | Close-out extraction, hand-written entries |
| 61 | adapt | memory.md | Close-out: "The plan directory stays after extraction" |
| 69 | adapt | feedback.md | "Who writes the entry": fresh subagent, orchestrator hands the case |
| 76 | keep | reduced-workset.md | opening: delegation is the default |
| 88 | adapt | verifier.md | Step 3: read the record, do not re-run gates |
| 96 | keep | reduced-workset.md | "Reduced means less process" |
| 107 | keep | reduced-workset.md | "Promotion to the full lifecycle", five signals |
| 110 | adapt | planning.md | "Index doc" table, all fourteen sections |
| 123 | keep | planning.md | "Label a suspected mechanism as a hypothesis" |
| 134 | adapt | planning.md | Report-to: duty ends at the final message |
| 135 | adapt | planning.md | fix spec states the four steps, points at references/test-first.md |
| 137 | adapt | planning.md | "Every spec includes the writing convention by path" |
| 145 | adapt | effort.md | model map: top tier cap 2, user-editable, queue or next tier |
| 155 | adapt | effort.md | per-task review medium, final review high |
| 173 | adapt | verifier.md | hand-back follows references/writing.md and opens with the verdict |
| 175 | adapt | verifier.md | "Never close out with an implementer unaccounted for" |
| 184-189 | keep | test-first.md | Four steps, Rule, recorded-red-run definition, impossible-red-run path, carried-everywhere table |
| 190 | adapt | memory.md | Committing is the user's call |
| 200 | keep | memory.md | "What stays out" |
| 216 | adapt | verifier.md | "A completion notice ... is a cue to verify. It proves nothing" |
| 220 | adapt | verifier.md | Step 5: `git status --short` on the shared checkout |
| 237 | adapt | orchestrating.md | Step 1: noisy investigation runs in a subagent |
| 241, 247, 249, 250 | keep/adapt | writing.md | Reports lead with the verdict; humanizer pass, closing audit, Final grep with triage table |
| 294-317 | keep | writing.md | Pattern list table, 21 rows plus the weak-tell note |
| 318-320 | keep | writing.md | "Briefs and specs", five rules |
| 321 | keep | both worksets | the loop and the reduced steps each name the sync rule |

Cross-worker cross-link rows, all five verified:

| Row | Link | Verified |
|---|---|---|
| 4 | SKILL.md routes into the ten references | table names all ten |
| 23 | SKILL.md points at references/writing.md | file exists, rule states the short-form fallback |
| 135 | planning.md points at references/test-first.md | file exists, four steps named |
| 137 | planning.md names references/writing.md by path | present in Spec rules |
| 173 | verifier.md points at references/writing.md | present in the Hand-back section |

Unplaced or compressed rows: none found.

## 3. User answers landed

| Answer | Evidence | Verdict |
|---|---|---|
| Plan dirs kept at close-out | orchestrating.md Step 7 item 5: "Keep the plan directory. catalyst-aio never deletes a plan directory"; memory.md "Close-out extraction": "The plan directory stays after extraction"; README.md project-files table: "kept after close-out" | PASS |
| Feedback entry by a fresh subagent | orchestrating.md "Complaints about catalyst-aio behavior" step 3; feedback.md "Who writes the entry" plus the brief it quotes; SKILL.md "What the orchestrator writes itself": the entry "belongs to the fresh subagent that wrote it" | PASS |
| Default cap of 2 on the top tier | effort.md model map: "The top tier ships with a cap of 2 concurrent subagents, a default the user can edit"; README.md: "with 2 on the top tier by default" | PASS |

## 4. Cross-file consistency

| Item | Result |
|---|---|
| Role names | orchestrator, implementer, verifier appear with the same meaning in SKILL.md and every file that names roles. "Worker" appears as the contract's generic noun for a delegated agent (inventory rows 133 and 134 use it), never as a fourth role. |
| Plan paths | `.cortex/plans/<date>-<epic>/` in SKILL.md, orchestrating.md, planning.md, reduced-workset.md, README.md |
| Memory paths | `.cortex/memory/` plus `MEMORY.md` in SKILL.md, memory.md, README.md |
| Reports | `.cortex/reports/<date>-<slug>.md` in SKILL.md, planning.md, memory-adjacent text, README.md |
| Hand-back location | `.cortex/reports/handbacks/<date>-<wave>.md` in SKILL.md, orchestrating.md, planning.md, reduced-workset.md, subagents.md, verifier.md (two places) |
| Feedback path | `.cortex/feedback/` in every file; feedback.md gives the exact `<YYYY-MM-DD>-<slug>.md` form, SKILL.md the shorter `<date>-<slug>` placeholder |
| Scratch | repo-local `.tmp/`, gitignored; `/tmp` named only as the path never to use |

One defect was found and repaired during verification: planning.md wrote the
hand-back path as `<cycle>.md`. Steered to aio-t3-planning, which changed it to
`<date>-<wave>.md`; a fresh grep after the steer shows all seven mentions agree.

## 5. Smoke test

Setup: `skills/catalyst-aio/` copied to
`.tmp/smoke/catalyst-aio/` (11 files). Actor: `claude -p --model
claude-opus-5-5 --disallowedTools Skill`, run from `.tmp/smoke`, prompt on
stdin, only instruction source the copied SKILL.md.

First attempt failed at the CLI parser: `--disallowedTools` is variadic and
consumed the prompt words as deny rules (`Permission deny rule "Your" matches no
known tool`, then `Error: Input must be provided either through stdin or as a
prompt argument`). No model call happened and no answer was produced. The rerun
passed the prompt on stdin and is the recorded run.

Recorded run: exit 0, 29.5 s, output at
`.tmp/smoke/answer.txt` (2975 bytes), stderr empty. Prompt stored at
`.tmp/smoke/prompt.txt`. The sandbox holds only those files and the copy; the
actor wrote nothing else, and no repo file changed during the run.

Answer, quoted whole:

```
I've only read the skill so far. I haven't looked at the repo or started anything.

**Files I read**, all under `.tmp/smoke/catalyst-aio/`:
- SKILL.md
- references/reduced-workset.md (one `--version` flag counts as a small task, so it runs the reduced workset: no plan directory, and the implementer gets its brief inline)
- references/effort.md
- references/subagents.md
- references/planning.md
- references/verifier.md
- references/test-first.md (a new flag is a change to observable behavior, so it falls under the test-first rule)
- references/writing.md

I skipped orchestrating.md, memory.md and feedback.md because nothing in this task routes to them yet.

**Who writes the code:** one fresh implementer subagent called `impl-version-flag`. I'm the orchestrator, so I don't edit any repo file. I write the brief and dispatch it. The implementer follows test-first: it writes a test for `--version`, runs it against the current code and records the failure (the "red run"), adds the flag, then runs the same test command until it passes. It leaves everything uncommitted and reports the files it changed, `git diff --stat`, and both test runs.

**Effort levels:**

| Role | Level | Reason |
|---|---|---|
| Orchestrator (me) | this session's own setting | effort.md never dispatches the orchestrator |
| Implementer | low | Adding a flag is mechanical once the brief names the version source. If I can't name where the version string comes from, I'll raise it to medium. |
| Verifier | medium | effort.md runs checks on low-effort chores at medium |

I'll set these through the Agent tool's effort parameter and also write them into each brief.

**After the implementer returns:**
1. I spawn one fresh verifier, `verify-version-flag`. Its brief has the same inline requirements, the implementer's final message quoted in full, and the check that proves the flag works: the new test, plus running the CLI with `--version`.
2. The verifier writes its pass criteria before it reads anything. It then confirms the red run exists and uses the same command as the green run, and reads the diff against the requirements. It also runs the check itself.
3. It writes a hand-back to `.cortex/reports/handbacks/2026-10-28-<wave>.md` and returns the same text to me.
4. I audit that hand-back without re-running any gate myself. If a required section is missing or a claim has no output behind it, it goes back to a verifier. If the verifier sends the task back, I dispatch a fresh implementer with its findings and then a fresh verifier.
5. Finally I report the effort levels, what changed, the hand-back path and anything still open. I write a memory entry only if the task turns up a decision or gotcha worth keeping. I'll ask before any commit.

Before I dispatch, I need two answers from you. Which CLI do you mean, or should I check whether the repo has only one? And where should the version string come from: a package manifest, a constant in the code, or `git describe`?
```

Criteria verdict:

| Criterion | Verdict |
|---|---|
| Names SKILL.md and the reduced-workset reference | PASS: first two files listed, with the reason for the workset |
| A subagent writes the code | PASS: one fresh implementer subagent, orchestrator edits no repo file |
| An effort level per role | PASS: table covers orchestrator, implementer, verifier |
| A fresh verifier checks the diff and the whole change | PASS: fresh verifier, red-run check, diff against the requirements, runs the check |
| The orchestrator runs no gate | PASS: "I audit that hand-back without re-running any gate myself" |
| No herdr, no dispatch tool | PASS: neither appears; the Agent tool is named as an example only |

No contamination (no herdr, c2d or catalyst-v2 citation), so no rerun. Overall:
PASS.
