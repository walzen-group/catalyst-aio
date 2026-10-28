# catalyst-aio

catalyst-aio is one Agent Skill that runs software work through a team of agents, in any harness that can spawn subagents. An orchestrator scopes the work, writes a plan and a spec per task, and dispatches implementer subagents that write the code and docs. Each wave ends with a fresh verifier subagent, which checks every diff against its spec and runs the whole-change check, and the orchestrator audits the verifier's hand-back and runs no gate itself.

## Relationship to the teacher

catalyst-aio is distilled from the full catalyst kit at commit a00dd80. That kit runs the same role split with dedicated machinery: herdr to multiplex terminal sessions, the dispatch, curator and session commands, and a quickchat layer. It also adds a status board, incident filing, and self-testing and instruction repair.

catalyst-aio keeps the judgment from the kit's skills: who plans, who writes, who verifies, and what each role reads before acting. Your harness's subagent facility covers the launch mechanics, so you start subagents the way that harness documents.

catalyst-aio adopts improvements from the kit. When a run fails, the orchestrator spawns a subagent that writes one entry per failure under .cortex/feedback/, in a format the teacher kit harvests and converts into a repair; references/feedback.md gives that format and the brief for the subagent that writes an entry. An agent running catalyst-aio never edits the skill files, because repairs land in the teacher first.

## Requirements

- A harness that loads Agent Skills. Without a skill loader, paste AGENTS.snippet.md into the project's AGENTS.md or CLAUDE.md.
- A subagent facility. catalyst-aio sends every change to a repo file to a subagent. Where the harness cannot spawn one, it says so and asks whether to run the work in the main session.
- Optional: effort selection per subagent. Where the harness sets effort, the orchestrator uses it. Where it does not, the brief states the intended level and the run continues on the session default.

## Install

### Claude Code

1. Copy the skill directory into ~/.claude/skills.

    ```bash
    mkdir -p ~/.claude/skills
    cp -r skills/catalyst-aio ~/.claude/skills/catalyst-aio
    ```

    A symlink keeps the install pointed at this checkout, so an edit to the skill needs no second copy:

    ```bash
    ln -s "$PWD/skills/catalyst-aio" ~/.claude/skills/catalyst-aio
    ```

2. Start a new session. Claude Code reads the skills directory when a session opens, so a session already running does not see the copy.

### Another harness

1. Find the skills directory in your harness's documentation. catalyst-aio installs as one directory named catalyst-aio under that path.

2. Copy the skill there.

    ```bash
    cp -r skills/catalyst-aio <harness-skills-dir>/catalyst-aio
    ```

3. If the harness discovers no skills directory, paste AGENTS.snippet.md into the project's AGENTS.md or CLAUDE.md, and replace the placeholder path with the absolute path to skills/catalyst-aio.

### Confirm the skill loaded

1. Ask the agent to use catalyst.

2. Read the reply for the file the agent opened. A loaded skill makes the agent read SKILL.md and ask scoping questions before it dispatches anything.

Expected result: the reply names skills/catalyst-aio/SKILL.md and asks what you want built.

A reply that starts work without naming SKILL.md means the skill did not load. Check the directory name and the install path, then start another session.

## Usage

Activation is by name. "Use catalyst" or "use catalyst-aio" starts a run. Multi-step work, delegable work and the word "subagent" do not start one; a session that would benefit from catalyst offers it in one line and waits for your go.

A run reports at these points:

| Point | What you see |
|---|---|
| Opening | Questions about intent, scope and open design questions |
| Before dispatch | A plan directory for multi-task work, with an index and one spec per task |
| At dispatch | The effort level each role runs at, and which subagent writes what |
| At the end | The verifier's hand-back: files changed, a diff verdict per task, gate evidence, open items and the questions waiting on you |

Small work runs the reduced path: no plan directory, one implementer subagent, one fresh verifier subagent, and the same effort report.

## Model routing

Every role runs on the session's model by default, and the orchestrator sets an effort level per task. references/effort.md defines the levels and the default for each role.

A harness that can run a subagent on a different model can fill the optional map in that file, which pairs each effort tier with a model id. The map ships empty, so an untouched install runs everything on the session model. The map also sets a concurrency cap per tier, with 2 on the top tier by default; edit the cap in the same table.

## What the skill contains

Every file is Markdown; the skill calls no external tool.

| Path | Contents |
|---|---|
| README.md | This file: what the skill is, install, usage and the files it writes |
| AGENTS.snippet.md | The instruction block for harnesses without skill discovery |
| .gitignore | Keeps the repo-local .tmp/ out of git |
| skills/catalyst-aio/SKILL.md | Entry point: the by-name gate, core principles, the role table, the routing table and the directory conventions |
| skills/catalyst-aio/references/orchestrating.md | The full lifecycle: understand, plan, dispatch a wave, verify, finish |
| skills/catalyst-aio/references/reduced-workset.md | The small-task path and the signals that promote a task to the full lifecycle |
| skills/catalyst-aio/references/subagents.md | How to brief, launch, wait for and stop a subagent |
| skills/catalyst-aio/references/verifier.md | The fresh verifier role, its procedure and the hand-back format |
| skills/catalyst-aio/references/planning.md | The plan directory, the index doc and the task spec anatomy |
| skills/catalyst-aio/references/effort.md | Effort tiers, role defaults and the optional model map |
| skills/catalyst-aio/references/test-first.md | The red-green procedure for fixes, with the recorded failing run |
| skills/catalyst-aio/references/writing.md | The doc writing convention and the condensed humanizer pass |
| skills/catalyst-aio/references/memory.md | The hand-written .cortex/memory store |
| skills/catalyst-aio/references/feedback.md | The .cortex/feedback log the teacher kit harvests |

## What a run leaves in your repo

| Path | Contents |
|---|---|
| .cortex/plans/<date>-<epic>/ | The index and one spec per task for a multi-task effort, kept after close-out as the record of how the waves ran |
| .cortex/memory/ with MEMORY.md | Project decisions, user corrections and gotchas the repo does not already state |
| .cortex/reports/ | User-facing deliverables, with verifier hand-backs under .cortex/reports/handbacks/ |
| .cortex/feedback/ | One file per catalyst-aio failure, in the format the teacher kit harvests |
| .tmp/ | Scratch: logs, one-off scripts and bulk data, kept out of git |

At close-out, the orchestrator extracts the settled decisions from the plan into .cortex/memory/ and sets the plan's Status line to a terminal value.

Scratch work goes to .tmp/ inside the repo. A path outside the repo cannot be reviewed with the change or cleaned up afterwards, so the skill never sends a worker to /tmp. The first use in a repo adds .tmp/ to that repo's .gitignore.
