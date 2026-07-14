# Playbook — VPS operations (install · smoke · real train · real eval · monitor)

Goal: reproduce the validator's pipeline **1:1** on a GPU box, cheaply. Golden rule: **CPU-first** — do all
data-gen + format/coverage asserts locally before you spend a GPU-second. Fill `<VPS_IP>` / paths from memory.

## 0. Connect + inventory (read-only)
```bash
ssh root@<VPS_IP> 'nvidia-smi --query-gpu=index,utilization.gpu,memory.used,memory.total --format=csv,noheader; \
                   docker ps; df -h /; free -g'
```
- **Never power-off / reboot** the VPS — that's the user's call. If a box is unreachable, report it; don't try to start it.
- Note the GPU count. Env tasks are often **forced to a fixed N×GPU regardless of model size** — read the
  validator's gpu-requirement constants and match it (train on the same N so DDP/batch dynamics match eval).

## 1. Match the validator container
- Find the exact training + eval **container image** the validator uses (read `gradients-ai/G.O.D` build/run code).
  Pull/build the SAME image. Do NOT hand-roll a venv that drifts from it.
- **Pin dependency versions** to the image. Mismatched `sglang`/`pydantic`/`transformers`/`vllm` are a classic
  crash source; match what the eval container ships.

## 2. Stage the base model (once)
```bash
python3 -c "from huggingface_hub import snapshot_download; \
  snapshot_download('<BASE_MODEL>', local_dir='/cache/models/<BASE_MODEL_DIR>')"
```
- Some servers **HF-validate `--model-path`** and reject local dirs → serve the base by its **repo-id** with
  `HF_HOME` pointing at the cache, instead of a local path.

## 3. OFFLINE smoke (CPU — do this BEFORE any GPU run)
- Run the miner's data-gen for a few games/rows and **assert**: correct output/tool-call envelope, format
  **byte-matches** the eval prompt, labels are what you intend, no crashes, coverage as expected.
- If gen is game-based, run under `--network none` to prove it needs no external server at train time.
- This catches ~most bugs (wrong format, dead code path, starved labels) for zero GPU cost.

## 4. Real train (mirror the validator)
- Run the miner's gen → trainer **exactly as the validator would** (same entrypoint, same flags, same N×GPU).
- **Turn ON an eval split + best-checkpoint** so you get a real held-out `eval_loss` (the learnability gate)
  — never ship a blind final checkpoint. Keep epochs small (**1–2**); over-training hurts head-to-head play.
- Run long jobs detached (`screen`/`tmux` or background) and log to a file you can tail.

## 5. Real eval (deterministic, validator-faithful)
- Use the validator's **own eval harness** (its `evaluation/` code) or the official standalone re-eval script,
  so the **LoRA merge/serve rule** and scoring match the tournament exactly. Don't hand-roll scoring.
- Eval is typically **greedy (temperature 0) + fixed seed** — reproducible. Compare candidates on the SAME seed.
- For head-to-head games, eval your trained model **vs the real peer field / vs base**, not just self-play.

## 6. A/B multiple variants in one session (avoid GPU idle)
- Because you prepared variants offline, pin **one variant per GPU** and train/eval them concurrently.
- Select the winner by the **measured** metric (eval_loss and/or head-to-head win-rate), never by an offline
  proxy alone.

## 7. Monitor (cheap, event-driven)
```bash
# de-ansi a training/eval log tail
ssh root@<VPS_IP> "sed -r 's/\x1b\[[0-9;]*m//g' <logfile> | tail -40"
```
- Watch for: `Traceback`, `CUDA out of memory`, a server stuck "waiting…" > a few min (it died — rerun with
  verbose), eval_loss trajectory. Prefer **/loop** with a sane interval over manual babysitting.

## 8. Teardown
```bash
ssh root@<VPS_IP> 'docker rm -f $(docker ps -aq) 2>/dev/null; nvidia-smi'   # free GPUs
```
- Clean containers + scratch when done so the box is idle-free. **Do not power it off** — tell the user it's free.

## Gotchas that actually bit (general classes)
- **Don't mount managed paths the eval wipes.** Some eval code `rmtree`s well-known paths; if you bind-mount your
  host workspace onto one, it deletes your host files. Mount ONLY a scratch/cache dir.
- **`python -m` uses CWD, not PYTHONPATH** for its package — run from the right dir or bake your patches into
  the image so the container's copy (not a stale one) runs.
- **Merge LoRA only as the validator does** (e.g. only when the adapter ships added tokens), else serve as an
  adapter — otherwise your local numbers won't reproduce the tournament.
- macOS lacks GNU `timeout`/`ls --time-style`; datacenter-bandwidth clones beat home-uplink — clone big repos
  on the VPS, not locally.
