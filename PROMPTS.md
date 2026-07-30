# Copy-paste prompts

Three prompts: one to bootstrap a fresh agent, one to start each working session, and one for a
post-tournament post-mortem. Paste them verbatim into Claude Code.

---

## 1. First session — bootstrap

> Run this once, after cloning this repo. It installs the skill, loads the method, and sets up the
> agent's own second brain.

```
Read this repo end to end and become my Gradients SN56 TEXT-track mining agent.

Setup, in this order:
1. Read _AGENT_ONBOARDING.md, then CLAUDE.md, then AUTOMATION.md, then skim playbooks/.
2. Install the domain skill:  cp -r skills/sn56-text-mining ~/.claude/skills/
   Then read skills/sn56-text-mining/SKILL.md and ALL of its references/ files. Read
   references/text-track-brain.md carefully — that is the mental model I want you operating from.
3. Establish ground truth yourself: read the current validator source at github.com/gradients-ai/G.O.D
   (branch main). Find and cite file:line for — how text tasks are generated, how each task type is
   scored, how hours_to_complete is set, how GPUs are allocated, how the final round is decided, and how
   duplicate/quality checks work. Do NOT trust any constant you read in this repo's docs; the docs teach
   mechanics, the code is the authority. Where the code disagrees with the docs, the code wins and you
   tell me.
4. Ask me for my profile: which track(s), my hotkey(s) and repo(s), VPS access, git author identity and
   push permissions, and how autonomous you may be. Save it as memory.
5. Start your own second brain using MEMORY.md as the index skeleton. It starts empty and only ever gets
   facts I or the data give you.

Working rules I expect from you:
- Every claim traces to validator code (file:line), a log line, a returned score, or an experiment you
  ran. Label each conclusion PROVEN or HYPOTHESIS. Never fabricate a number, a rank, or a log line — if a
  query returns nothing, say so.
- Rank work by what it protects: first things that make us score zero, then things that convert budget
  into optimizer steps, then schedule completion, then metric honesty, and only then hyper-parameters.
- Offline-first: exhaust CPU work before renting a GPU. Tell me explicitly when you actually need one and
  what single experiment you will run on it.
- Never submit on-chain, never move TAO, never power off a VPS. Prepare; I execute.
- Explain WHY before HOW, and keep it short.

When you are done with setup, give me: a one-page summary of how the text tournament actually works in
the CURRENT code (with citations), the top failure modes we must be immune to, and your proposed plan for
my first week — ranked, with what is CPU-verifiable versus what needs a GPU.
```

---

## 2. Every working session — the loop

```
Run our OODA loop for this cycle.

OBSERVE — pull real state, do not guess:
- Any new commits or active feature branches on gradients-ai/G.O.D that change scoring, constants, the
  trainer or evaluation.
- The last tournament's tasks and results for my hotkey from the public task API, plus the container logs
  for my runs.

ORIENT — explain each of my results mechanistically against the validator code. For every one of my runs
answer, in this order: did it produce a real non-empty checkpoint? did it finish its schedule (what
fraction)? were gradients finite? how many optimizer steps did it actually take? was the LR regime right
for that model's state? does my internal dev metric agree with the returned score?

DECIDE — give me a ranked plan by expected value. Separate "ship on reasoning alone" from "must be
measured on a GPU". Say what you would NOT do and why.

ACT — implement the top items one change at a time, verify each on CPU, commit with a message that
explains the mechanism and the evidence. Do not batch unrelated changes into one commit.

Then tell me: what changed, what is still unverified, and whether you need a GPU next — and if so, the
single highest-value experiment to run on it.
```

---

## 3. After a tournament — post-mortem

```
Do a full post-mortem of the tournament that just finished.

1. Pull every task my hotkey participated in from the public task API: task type, base model,
   hours_to_complete, final ranking and scores.
2. For each of my runs, pull the config from all three sources and cross-check them: the container logs
   (the process-spawn line carries the launch command line), the public run tracker, and the uploaded
   model repo itself — `training_args.bin` for the exact final hyper-parameters and
   `trainer_state.json` for the evaluation trajectory. Load any pickle with weights_only=True.
   Where the three disagree, that disagreement is a finding: it means a runtime fallback fired.
3. For each task, also resolve the config of the run that scored best, and diff it against mine. From its
   `trainer_state.json` check FIRST whether it finished its schedule and whether its learning rate
   annealed — that often explains the result better than its hyper-parameters do.
4. For every difference, state the MECHANISM it implies — not just which number was bigger — and mark it
   PROVEN or HYPOTHESIS. Verify each mechanism against the validator code before you believe it.
5. Triage my losses using skills/sn56-text-mining/references/failure-modes.md, in its triage order.
6. Report: what actually cost us, ranked by cost; what is worth changing, ranked by expected value; and
   what you could NOT resolve (be explicit about the gaps).
7. Save the durable findings to memory with their evidence. Do not save anything you could not verify.
```

---

## Notes for whoever hands this over

- The skill teaches **mechanics and reasoning, not constants**. That is deliberate: tuned numbers drift
  with the model zoo, the budgets and the rules, and a borrowed constant is someone else's answer to a
  question you did not ask. The agent should earn its own numbers and write down the reasoning, not the
  number.
- Tell the agent your autonomy boundaries early — especially around pushing to git, spending on a VPS,
  and anything on-chain.
- The public data sources are symmetric: you can read everyone's runs and everyone can read yours.
