# Playbook — checks & audits (pull the real state)

Never reason from memory about what happened — pull it. These are the OBSERVE tools.

## Tournament results / rankings / who-won
- Skill **tournament-task-audit** (or hit `api.gradients.io/auditing/tasks/<task_id>` directly): returns the
  environment, base model, per-hotkey rank + score + trained-repo, winner. Reconstruct group standings from it.
- Per-game head-to-head + scoring: verify how the raw win-rate maps to the advancing metric by reading
  `validator/scoring/*` in `gradients-ai/G.O.D` — don't assume a leaderboard % equals the tournament score.

## What a miner actually ran at runtime
- Skill **gradients-grafana-logs** (Grafana/Loki, no auth): the real training command, hyperparameters dumped
  at start, timings, errors — for any task_id + hotkey. Useful to see what winners did.

## Trained-model forensics (no GPU)
- Pull a tournament model's `loss.txt`, `config.json`, `generation_config.json`, `added_tokens.json`,
  tokenizer/chat_template from HF. `loss.txt` reveals footprint + whether they used an eval split. Compare
  yours vs theirs to rule out format/tokenizer differences before blaming strategy.

## Upstream changes (before they hit you)
- Skill **god-upstream-tracker**: new branches/commits in `gradients-ai/G.O.D`. Check before each tournament —
  new games, format changes (e.g. tool-calling envelopes), scoring tweaks, dataset whitelist changes.

## Competitor study
- Skill **tournament-miner-research**: read other miner repos/branches to understand approaches (method study,
  not copying).

## Verify-by-data discipline
- After any change, **exercise it** and observe (CPU asserts first, then GPU). "It should work" is not done.
- Reconstruct known-good numbers (e.g. re-derive a published score from the raw data) to confirm you understand
  the mechanic before you trust your own new numbers.
