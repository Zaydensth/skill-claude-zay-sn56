# AUTOMATION — how the agent drives itself

This is the operational layer on top of `CLAUDE.md`. It maps every recurring job to the tool/skill that
does it, and defines the self-driving loop. Placeholders (`<VPS_IP>`, `<MINER_REPO>`, `<BASE_MODEL>`,
`<VALIDATOR_IMAGE>`) → fill from your own `user`/`project` memory. Playbooks live in `playbooks/`.

## The master loop (OODA, self-paced)
```
OBSERVE   pull the real state (API/logs/git/VPS/gen output) — never assume
ORIENT    reconcile against the validator code (gradients-ai/G.O.D main)
DECIDE    pick the highest-leverage next action; prepare MULTIPLE variants offline
ACT       do it; VERIFY by data (CPU-first); record what you learned
LOOP      repeat; check in with the user at milestones + when a decision is theirs
```
Guardrail: **maximize CPU/offline work before touching a GPU** so no GPU sits idle waiting for you to
write code. Prepare all variants, then run one dense GPU session.

## Job → tool map (what to reach for)
| Job | Use | Playbook |
|---|---|---|
| Heavy analysis / design / audit | **Workflow** tool (fan-out subagents → adversarial verify → synthesize) | `research-and-review.md` |
| One focused sub-task in parallel | **Agent** tool (subagent) | `research-and-review.md` |
| Web/multi-source research | **deep-research** skill / Workflow | `research-and-review.md` |
| Who-won / rankings / task facts | **tournament-task-audit** skill (api.gradients.io) | `checks-and-audits.md` |
| What a miner actually ran at runtime | **gradients-grafana-logs** skill (Loki) | `checks-and-audits.md` |
| Upstream changes before they land | **god-upstream-tracker** skill | `checks-and-audits.md` |
| Study competitor repos | **tournament-miner-research** skill | `checks-and-audits.md` |
| Recurring monitor / poll | **/loop** skill (interval or self-paced) + **ScheduleWakeup** | `self-driving-loop.md` |
| VPS: connect/install/smoke/train/eval | Bash over SSH + docker | `vps-operations.md` |
| Summary / memory / dashboard / alert | memory files + PushNotification | `reporting-and-memory.md` |
| Verify a change end-to-end | run it (CPU asserts, then GPU) — never claim "done" unmeasured | `vps-operations.md` |

## Non-negotiables (repeat of the guardrails that bite in automation)
- **Verify by data, never fabricate.** VERIFIED (ran/read) vs INFERRED, always.
- **Read the validator code** before claiming any mechanic; your gen must match what eval parses.
- **VPS power is the user's** — never power-off; ask. **Never submit the on-chain fee** — user does that.
- **Don't push to GitHub** unless it's a validated fix AND approved. **LICENSE/NOTICE byte-identical.**
- Prefer **/loop + ScheduleWakeup** to babysitting; report at milestones, not every step.
