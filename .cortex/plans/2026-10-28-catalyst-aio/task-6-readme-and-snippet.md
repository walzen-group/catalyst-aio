# Task 6: README.md, AGENTS.snippet.md, .gitignore

Plan index: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/00-index.md`.
Inventory: `/home/nixos/repos/catalyst-aio/.cortex/plans/2026-10-28-catalyst-aio/inventory.md`
(read its counts line and any rows targeted at README, if present).
Read the index's Goal, Architecture, Resolved questions, Revision notes and
Global constraints.

## Context

catalyst-aio is one Agent Skill that keeps the full catalyst kit's judgment and
runs in any harness with a subagent facility. You write the repo's front door:
what it is, how it relates to the teacher kit, how to install it, and a snippet
for harnesses that do not auto-discover skills. Other workers write the skill
files in parallel; describe them by the fixed filenames in the index
Architecture.

## Target

Write exactly these files in `/home/nixos/repos/catalyst-aio/`:

- `README.md`
- `AGENTS.snippet.md`
- `.gitignore` (one line: `.tmp/`)

Teacher README for tone and facts: `/etc/nixos/nix/catalyst/README.md`.

## Change

### README.md

Sections:

1. What catalyst-aio is, in two or three sentences: the catalyst roles
   (orchestrator, implementer subagents, a fresh verifier per wave) as one
   skill, for any harness with subagents.
2. Relationship to the teacher: distilled from catalyst at commit `a00dd80`;
   what was left out (herdr, the dispatch/curator/session tools, quickchat,
   status board, incident filing, self-testing and instruction repair) and
   that improvements flow down from the teacher while catalyst-aio's
   `.cortex/feedback/` log flows up. Name `references/feedback.md` as the format.
3. Requirements: a harness that loads Agent Skills (or reads the AGENTS
   snippet) and can spawn subagents; per-subagent effort or model selection is
   optional.
4. Install, as numbered steps with fenced commands:
   - Claude Code: symlink or copy `skills/catalyst-aio` into `~/.claude/skills/`
     (this path is verified on this machine).
   - Other harnesses: put `skills/catalyst-aio` in the harness's skills
     directory, or paste `AGENTS.snippet.md` into the project's `AGENTS.md`
     / `CLAUDE.md`. Do not state skills paths for other harnesses as facts;
     you have not verified them. Say "check your harness's docs for its skills
     directory".
   - How to confirm it loaded: ask the agent to use catalyst and check it reads
     `SKILL.md`.
5. Usage: say "use catalyst" (activation is by name only); what the user sees
   (questions first, a plan for multi-task work, effort report at dispatch,
   a verifier's hand-back at the end).
6. Optional model map: point to `references/effort.md`.
7. Layout table: every file in the index Architecture with one line each.
8. Project files catalyst-aio creates in a repo it works on: the `.cortex/`
   paths and `.tmp/` from index Global constraint 8.

### AGENTS.snippet.md

A short block (aim for under 25 lines) a user pastes into `AGENTS.md` or
`CLAUDE.md`. It tells the agent: when the user asks for catalyst or
catalyst-aio by name, read `<path-to>/skills/catalyst-aio/SKILL.md` and follow
it, reading the references it names when it routes there; do not use catalyst
otherwise. Use a clear placeholder for the install path and a line telling the
user to replace it.

## Constraints

Index Global constraints 1 to 10. README is the one place allowed to name the
teacher kit (index Global constraint 2 exception). README gets the full
humanizer pass from `catalyst-v2-writing-docs`.

## Acceptance

```bash
cd /home/nixos/repos/catalyst-aio
cat .gitignore                     # exactly: .tmp/
grep -niE 'herdr|\bc2[dmr]\b|\bomp\b|steer|\bpane|\btabs?\b|A2A|A2U' AGENTS.snippet.md
# no output
grep -oE 'references/[a-z-]+\.md' README.md | sort -u
# every name is one of the ten in the index Architecture
wc -l AGENTS.snippet.md            # under 25
```

Run the `catalyst-v2-writing-docs` Final grep over README.md and
AGENTS.snippet.md and fix hits per its table. Record that the humanizer pass
and its closing audit ran.

## Emission discipline

Survey budget 10 minutes, then write files and save at least every 10 minutes.

## Report to

Your completion hand-back goes to the wave's meta-agent `meta-aio-w2`, by
`c2d steer --agent meta-aio-w2 --text "A2A: <hand-back>"`: files written,
`git status --short`, acceptance output, deviations. After sending it, run no
further commands, read no other agent's state, wait for no acknowledgment, and
go idle. If the steer fails, send it once more, then stop anyway.
