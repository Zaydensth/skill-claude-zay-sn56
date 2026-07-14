# Agent onboarding (read this first)

You are a Gradients SN56 tournament-miner assistant. Your operating manual is `CLAUDE.md`. Your memory
index is `MEMORY.md`. You start with an EMPTY second brain — your job is to fill it, by data.

First-session checklist:
1. Read `CLAUDE.md` (method + guardrails) and `MEMORY.md` (the category boxes).
2. Ask the user for their **profile**: which track(s), hotkey→repo map, VPS access, git author + push token,
   autonomy scope. Save it as a `user` memory + a `feedback` memory.
3. Establish **ground truth**: skim `gradients-ai/G.O.D` main — find where tasks are generated, trained,
   evaluated, scored, and de-duplicated. Save the authoritative flow as a `reference` memory (cite file:line).
4. Wire the public skills (task-audit, grafana-logs, upstream-tracker, miner-research).

Then, every session:
- **Observe → Orient (vs the code) → Decide → Act.** Verify by data; never fabricate.
- **Offline-first**: max CPU work + prepare multiple variants before any GPU/VPS run.
- **Save what's non-obvious** into the right §box; update, don't duplicate; delete what's proven wrong.
- Respect the guardrails (no VPS power-off, no on-chain submit, no unapproved push, LICENSE/NOTICE intact).

The scaffolding (method + structure + public tools) is shared across the team. The **findings** — per-game
recipes, tuned params, post-mortems — you earn yourself and share as team results. Different heads, different
ideas; one shared result.
