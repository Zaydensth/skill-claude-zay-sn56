# Gradients SN56 miner — agent starter pack (general, no private research)

Hand this to a teammate so their Claude Code agent starts with the same **method + second-brain structure**
we use — WITHOUT any of our research/experiments/identity. Their agent builds its own findings from here.

## What's inside
- `CLAUDE.md` — the agent operating manual (method: read-validator-code, OODA, verify-by-data, offline-first,
  multi-agent workflows, memory protocol, tournament mechanics, public skills, guardrails).
- `MEMORY.md` — the second-brain **index skeleton** (category boxes, empty — fill by data).
- `_AGENT_ONBOARDING.md` — first-session checklist for a fresh agent.

## Install (their machine)
1. **CLAUDE.md** → the project root of their miner repo (or `~/.claude/CLAUDE.md` for global). Claude Code
   auto-loads it as project instructions.
2. **MEMORY.md** + **_AGENT_ONBOARDING.md** → their agent's memory dir, e.g.
   `~/.claude/projects/<their-project>/memory/`. Keep `memory/*.md` fact files alongside it as they grow.
3. First run: tell the agent *"read _AGENT_ONBOARDING.md, then set up my profile."* It'll ask for their
   hotkey→repo map, VPS access, git author, and autonomy scope, and start its own second brain.
4. (Optional) copy the **public** skills they want (`tournament-task-audit`, `gradients-grafana-logs`,
   `god-upstream-tracker`, `tournament-miner-research`) into `~/.claude/skills/`.

## NOT included (on purpose)
Our per-game recipes, tuned params, experiment/A-B results, competitor intel, hotkeys/repos/VPS IPs — the
competitive edge. The teammate earns their own; results get pooled as one team.
