# Gradients SN56 miner — agent starter pack (general, no private research)

Hand this to a teammate so their Claude Code agent starts with the same **method + second-brain structure**
we use — WITHOUT any of our research/experiments/identity. Their agent builds its own findings from here.

## What's inside
- `CLAUDE.md` — the agent operating manual (method: read-validator-code, OODA, verify-by-data, offline-first,
  multi-agent workflows, memory protocol, tournament mechanics, public skills, guardrails).
- `MEMORY.md` — the second-brain **index skeleton** (category boxes, empty — fill by data).
- `_AGENT_ONBOARDING.md` — first-session checklist for a fresh agent.
- `AUTOMATION.md` — **the operational layer**: the self-driving loop + a job→tool map (which tool/skill for
  each recurring job). Read this to make the agent *work and prompt itself*.
- `playbooks/` — concrete how-to for the jobs you run often:
  - `vps-operations.md` — connect · install/match validator container · stage model · **CPU smoke** · **real
    train** · **real eval** · monitor · teardown, plus the gotchas that actually bite.
  - `research-and-review.md` — multi-agent Workflow patterns (fan-out → adversarial verify → synthesize;
    diagnose→design; code-review).
  - `self-driving-loop.md` — pacing, `/loop` + Monitor + ScheduleWakeup, when to self-drive vs check in.
  - `checks-and-audits.md` — pull the real state (task-audit, grafana logs, upstream tracker, forensics).
  - `reporting-and-memory.md` — milestone summaries, memory updates, alerts, dashboard.
- `skills/sn56-text-mining/` — **the TEXT-track domain brain** (installable Claude Code skill). Round
  structure and dethrone rules · what the validator actually measures per task type (instruct / DPO /
  GRPO / continuous-SFT) and the duplicate/quality checks that can zero a good model · the container
  pipeline from clone to upload and where a miner may be clever · VPS setup and a cheapest-proof-first
  run ladder · how to build an evaluation that predicts the hidden score · the public API/Loki/tracker
  sources and how to query them · a ranked catalogue of failure modes each with a detection method · and
  how to REASON about learning rate, schedule, batch, epochs and packing. Mechanics and reasoning only —
  **no tuned constants and no borrowed recipes**, so the agent earns its own numbers.

## Install (their machine)
1. **CLAUDE.md** → the project root of their miner repo (or `~/.claude/CLAUDE.md` for global). Claude Code
   auto-loads it as project instructions.
2. **MEMORY.md** + **_AGENT_ONBOARDING.md** → their agent's memory dir, e.g.
   `~/.claude/projects/<their-project>/memory/`. Keep `memory/*.md` fact files alongside it as they grow.
3. First run: tell the agent *"read _AGENT_ONBOARDING.md, then set up my profile."* It'll ask for their
   hotkey→repo map, VPS access, git author, and autonomy scope, and start its own second brain.
4. **TEXT track**: copy the domain skill so Claude Code loads it automatically —
   ```bash
   cp -r skills/sn56-text-mining ~/.claude/skills/
   ```
   Then, in the first session: *"read the sn56-text-mining skill, then references/text-track-brain.md."*
   The agent should do this **before** it writes or changes any training code.
5. (Optional) copy the other **public** skills they want (`tournament-task-audit`, `gradients-grafana-logs`,
   `god-upstream-tracker`, `tournament-miner-research`) into `~/.claude/skills/`.

## NOT included (on purpose)
Our per-game recipes, tuned params, experiment/A-B results, competitor intel, hotkeys/repos/VPS IPs — the
competitive edge. The teammate earns their own; results get pooled as one team.
