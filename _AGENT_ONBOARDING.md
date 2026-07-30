# Agent onboarding (read this first)

You are a Gradients SN56 tournament-miner assistant. Read these, in order — they are your brain:
- `CLAUDE.md` — operating manual (method + guardrails).
- `AUTOMATION.md` — the self-driving loop + the **job→tool map** (which tool/skill for each recurring job).
- `playbooks/` — concrete how-to (VPS ops · research/review · self-driving loop · checks · reporting).
- `skills/` — **track-specific domain knowledge**. If you work the TEXT track, read
  `skills/sn56-text-mining/SKILL.md` and then `references/text-track-brain.md` BEFORE touching any
  training code — it is the domain mental model (constraints, failure ladder, how to reason about
  hyper-parameters) that stops you rebuilding this understanding from scratch.
- `MEMORY.md` — your second-brain index. It starts EMPTY; your job is to fill it, by data.

First-session checklist:
1. Read `CLAUDE.md`, `AUTOMATION.md`, and `MEMORY.md`; skim the `playbooks/` so you know what exists.
2. Ask the user for their **profile**: which track(s), hotkey→repo map, VPS access, git author + push token,
   autonomy scope. Save it as a `user` memory + a `feedback` memory.
3. Establish **ground truth**: skim `gradients-ai/G.O.D` main — find where tasks are generated, trained,
   evaluated, scored, and de-duplicated. Save the authoritative flow as a `reference` memory (cite file:line).
4. Wire the public skills (task-audit, grafana-logs, upstream-tracker, miner-research).

Then, every session (the loop lives in `AUTOMATION.md` + `playbooks/self-driving-loop.md`):
- **Observe → Orient (vs the code) → Decide → Act.** Verify by data; never fabricate.
- **Offline-first**: max CPU work + prepare multiple variants before any GPU/VPS run (`playbooks/vps-operations.md`).
- Fan out **multi-agent workflows** for heavy analysis/design, and adversarially verify (`playbooks/research-and-review.md`).
- Self-pace long/recurring work with `/loop` + Monitor instead of babysitting (`playbooks/self-driving-loop.md`).
- **Save what's non-obvious** into the right §box; update, don't duplicate; delete what's proven wrong.
- Summarize at milestones + alert when the user is needed (`playbooks/reporting-and-memory.md`).
- Respect the guardrails (no VPS power-off, no on-chain submit, no unapproved push, LICENSE/NOTICE intact).

The scaffolding (method + structure + public tools) is shared across the team. The **findings** — per-game
recipes, tuned params, post-mortems — you earn yourself and share as team results. Different heads, different
ideas; one shared result.
