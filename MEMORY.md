# MEMORY INDEX — second brain (boxed by category)

> This file is loaded EVERY session. One line per memory: `- [Title](file.md) — one-line hook`.
> Never put memory content here — only pointers. Detail lives in `memory/<name>.md` (frontmatter + fact).
> New agent → read [_AGENT_ONBOARDING.md](_AGENT_ONBOARDING.md) first.
>
> Memory file format:
> ```
> ---
> name: <kebab-slug>
> description: <one-line summary — used to decide relevance on recall>
> metadata: { type: user | feedback | project | reference }
> ---
> <the fact. for feedback/project add **Why:** and **How to apply:**. link others with [[name]].>
> ```
> Types: user = who the user is + preferences · feedback = how you should work (with why) ·
> project = ongoing goals/constraints not in the code · reference = pointers to external resources.

Categories (fill in as you learn — start empty, grow over time):

## §0 PROFILE & WORKING STYLE (iron rules)
- _(save a `user` memory: who the miner is, which track(s), hotkeys→repo map, git author + push token)_
- _(save `feedback` memories as the user corrects you: wait-before-execute, verify-by-data, autonomy scope, …)_

## §1 TRAIN — validator flow + local/smoke
- _(how the validator generates + trains a task; local smoke-test gotchas; env/format contract)_

## §2 EVAL — how scoring actually works
- _(read validator/scoring/* + eval harness; PvP/rank-quantile mechanics; dedup criterion; forfeit paths)_

## §3 PARAM / TRAINING-DYNAMICS
- _(learning-rate/epoch findings; over-training vs PvP; per-architecture notes — earn these from real runs)_

## §4 READ DATA — observability
- _(tournament APIs, Grafana/Loki, dashboards, task-audit pointers)_

## §5 TOURNAMENT RESULTS — post-mortems
- _(per-tournament results + why you won/lost, with the by-code root cause — your most valuable memories)_

## §6 EXPERIMENTS — your changes & branches
- _(what you changed, why, and the measured result; keep A/B outcomes)_

## §7 GAME / TASK STRATEGY — per-game
- _(per-game or per-task-family approach, verified in code + measured)_

## §8 PROJECT & ROADMAP
- _(project context, repo map, upstream roadmap, prep checklists)_
