# Step-by-Step Build Guide — Project 03: Performance Optimization Initiative

This is a **100-hour, 5-week build plan** at full-time pace, or ~10 weeks at 10 h/week. Phases are sequential — do not skip Week 1's measurement work; that's where most of the principal-level signal is.

Each phase has: **goal**, **day-level breakdown**, **validation gate**, and **gotchas** drawn from real production scars.

---

## Phase 0 — Pre-Work (4 h, before Week 1)

### Goal
Confirm environment, tooling, and choose your target.

### Tasks
1. **Pick the reference target**: inference serving OR training. Pick **one**. The architectures differ enough that doing both within 100 hours is unrealistic. Document the choice in `docs/target-selection.md` with one paragraph each on alternatives considered.
2. Provision (or confirm) the hardware. Test profilers on it:
   - `nsys profile --stats=true python smoke.py` produces a `.nsys-rep`.
   - `ncu --set full -o smoke python smoke.py` produces a `.ncu-rep`.
   - `dcgmi dmon` shows live SM utilization, memory throughput.
3. Read (or skim) these references for whichever target you picked:
   - **Inference**: vLLM paper (PagedAttention), FlashAttention 2/3 papers, speculative decoding papers (Medusa, EAGLE), TensorRT-LLM docs, FP8 docs from NVIDIA Transformer Engine.
   - **Training**: FSDP paper, ZeRO paper, Megatron-LM paper, NCCL tuning guide, PaLM scaling paper.
4. Run a sanity baseline. For inference: serve a small model with vLLM, hit it with 100 requests, eyeball numbers. For training: torchtitan or your reference repo, 100 steps at 8 GPUs, eyeball MFU.
5. Identify a load generator (`locust` / `vegeta` / `wrk2` / domain) for inference, or a reference dataset / config for training.

### Gate
You can answer: "What is the theoretical peak FLOPS/s of one GPU on my SKU? What is the achievable memory bandwidth? What is the achievable NCCL all-reduce BW on my fabric? Given those, what is the upper-bound on my chosen metric?"

If you can't answer, do the math now — every later phase depends on this ceiling.

### Gotchas
- "I have access to GPUs" is not "I have permission to install `nsys`." Check before week 1.
- Profilers slow code down; the *unprofiled* run is your baseline, not the profiled one.
- A "smoke" workload that exercises only a 100M model will mislead you. Use the real workload shape from day one.

---

## Week 1 — Target Selection, Methodology, Baseline, Design Doc Draft 1 (18 h)

The single most common failure mode is starting optimization before defining the measurement. Don't.

### Day 1 (4 h) — Workload definition

Write `docs/workload.md` covering:
- Model (name, params, weight precision, tokenizer)
- Request distribution (for inference): prompt-length distribution, output-length distribution, RPS, concurrency, request mix
- Dataset (for training): tokenization, sequence length, packing, shuffling
- Hardware: SKU, count, interconnect, driver, CUDA version, container image
- Software: framework versions (PyTorch, vLLM, etc.) pinned

This is the *contract* for every experiment. Without it, your numbers are not reproducible.

### Day 2 (4 h) — Measurement methodology

Write `docs/methodology.md`:
- The named metric and its definition (units, percentile, condition)
- Warmup policy
- Sample size and the power calculation that justified it
- Statistical test (pre-registered; Welch's t-test on bootstrap or Bayesian estimation)
- Confounder management (same node, alternating arms, time-of-day)
- Noise floor measurement protocol (run two baselines back-to-back; the diff is the noise floor)

Pre-register the analysis in a Git commit before any experiment runs.

### Day 3 (4 h) — Baseline measurement

- Run the baseline workload N times (N from your power calc). Discard warmup.
- Compute mean, std, 95 % CI. **Report the noise floor** (the diff between two back-to-back baselines).
- Profile baseline with `nsys` (full trace) and `ncu` (one representative kernel of interest).
- Commit profiles to `profiles/baseline/`.

If the noise floor is bigger than your target effect, **stop and fix the noise** before optimizing. Common noise sources: shared host, DVFS, thermal throttling, mixed traffic, lack of warmup.

### Day 4 (3 h) — Design doc skeleton + decision menu

Design doc sections:
1. Problem (what perf gap matters, what's the business connection)
2. Target metric + baseline + target
3. Methodology
4. Optimization layers + knob inventory
5. Campaign plan (which knobs in what order)
6. Regression-prevention strategy
7. Rollback strategy
8. Executive narrative outline
9. Risks + open questions

For each of these, write a 2–4 sentence position; they become ADR stubs:
- Metric choice
- Methodology
- Top 3 optimization bets
- Regression-prevention strategy
- Durability strategy
- Rollback model

### Day 5 (3 h) — Peer review + experiment plan

Get the doc + workload + methodology in front of at least one perf-experienced engineer. Use feedback to refine. Then write `experiments/plan.md` listing the **first 8 experiments** by ID, hypothesis, predicted outcome.

### Validation gate
- [ ] `docs/workload.md` pins the workload contract
- [ ] `docs/methodology.md` pre-registers the analysis
- [ ] Baseline measured with 95 % CI and noise floor reported
- [ ] Baseline profiles committed
- [ ] Design doc draft 1 (≥ 8 pages) committed
- [ ] 6 ADR stubs
- [ ] First 8 experiments planned
- [ ] At least one perf-experienced reviewer has commented

### Gotchas
- Pre-registration feels overkill until you have to defend the headline number. Then it's a lifesaver.
- "I'll just measure once" is the most common methodological error. Always at least 3 runs; usually 10+.
- Profiler overhead can be enormous (50%+). Time *outside* the profiler.

---

## Week 2 — Optimization Round 1: Kernel + Framework (22 h)

### Goal
Get the first 5–8 experiments through the campaign. Most should be in kernel + framework layers because that's where the biggest single wins usually live.

### Day 6 (5 h) — Profile-driven candidate selection

Open the baseline profile. List the top 5 cost centers. For each, note:
- What kernel / op
- What % of time
- What's the theoretical floor (memory-bound, compute-bound, comms-bound?)

For inference: prefill vs decode mix; KV-cache pressure; attention vs MLP cost; tokenizer overhead.

For training: AllReduce vs compute overlap; AllGather waiting on prefetch; backward pass kernel mix; optimizer step cost.

Pick the first 5 experiments from this list.

### Day 7 (5 h) — Kernel layer experiment(s)

For **inference**:
- Enable FlashAttention-3 (H100) or FA-2 (Ampere). Measure.
- Try vLLM's CUDA-graph mode. Measure.
- Enable FP8 (NVIDIA Transformer Engine) on a copy of the model; validate logits diff < ε. Measure.

For **training**:
- Enable FA-3 in the attention path. Measure.
- `torch.compile` with fullgraph; debug recompiles. Measure.
- Selective activation checkpointing (only attention layers). Measure.

Document every change as an experiment row.

### Day 8 (4 h) — Framework layer experiment(s)

For **inference**:
- vLLM `--enable-chunked-prefill` if not already on. Measure.
- Speculative decoding with a small draft model. Measure correctness diff.
- Prefix caching if workload has shared prompts. Measure.

For **training**:
- Tune `forward_prefetch`, `backward_prefetch` for FSDP. Measure.
- Increase microbatch size to compute-bound; gradient accumulation if memory-tight. Measure.
- ZeRO stage 2 vs stage 3; memory vs comms trade. Measure.

### Day 9 (4 h) — Correctness validation

Don't ship a perf win that breaks correctness.

For **inference**:
- Run a held-out eval (MMLU, HumanEval, a domain-specific eval) against baseline + each optimization. Document accuracy diff.
- Run a logits-diff test on a fixed seed corpus; threshold ε per ADR.

For **training**:
- Run baseline + treatment for K steps at matched seed; compare loss curves; threshold per ADR.
- Convergence smoke: train to a milestone, compare eval metrics.

### Day 10 (4 h) — Write up + re-baseline

- Update `experiments/experiment-log.md` with each experiment.
- Re-profile the system in its current state; commit `profiles/round1/`.
- Identify the next 5 experiments based on the new profile (the bottleneck has moved by now).

### Validation gate
- [ ] ≥ 8 experiments documented this week
- [ ] At least 2 kernel-layer wins measurably contributing
- [ ] At least 2 framework-layer wins measurably contributing
- [ ] At least 1 negative experiment (tried, didn't help)
- [ ] Correctness diff documented for every optimization
- [ ] Re-profile shows the bottleneck has moved

### Gotchas
- The bottleneck moves. Re-profile after every meaningful change; otherwise you're optimizing yesterday's problem.
- `torch.compile` will recompile silently on shape changes; check for cache misses in the log.
- FA-3 has shape restrictions (sequence lengths, head dims) — read the docs before claiming it works for your model.

---

## Week 3 — Optimization Round 2: System + Policy + Campaign (22 h)

### Goal
Add the system-level and policy-level wins. By end of week, the experiment log has ≥ 20 rows and the headline number is at or near the target.

### Day 11 (5 h) — System layer experiments

Pick from the inventory based on what the profile says:
- **NCCL tuning**: try `NCCL_ALGO=Tree` vs `Ring`, `NCCL_PROTO=Simple`/`LL128`, adjust `NCCL_NTHREADS`. Measure with `nccl-tests` and end-to-end.
- **Topology pinning**: confirm NUMA-aware rank-to-GPU-to-NIC assignment. Use `numactl` and `nvidia-smi topo -m` to verify.
- **CUDA allocator**: `PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True,backend:cudaMallocAsync`. Measure tail latency.
- **GPU clock policy**: pin persistence mode; consider clock-locking if jitter is high.
- **Container/host**: CPU pinning for the inference engine; isolcpus if available.

### Day 12 (5 h) — Policy layer experiments

For **inference**:
- Routing by prompt-length bucket (short prompts get more concurrency).
- SLO-aware admission control: reject when in-flight tokens exceed budget.
- Autoscaling: track p99 vs target; pre-warm.
- Batch admission policy: cap max tokens per microbatch.

For **training**:
- Job-level: spot vs on-demand mix at the scheduler. (Mostly cost-side.)
- Within-job: dataloader concurrency, prefetch depth, packed sequences.

### Day 13 (4 h) — Negative experiments + write-ups

You will have tried things that didn't work. Don't delete them.

- Document each "tried, didn't help" with a hypothesis-of-why-not.
- These often reveal more than the wins (e.g., "FP4 not yet viable on H100 for this shape because…").
- At least 5 negative experiments by end of campaign.

### Day 14 (4 h) — Second workload validation

Pick a **second workload variation** (different model size, different prompt-length mix, different hardware count). Re-run the top 3 winning experiments. Document which survive and which don't.

This is the durability evidence in miniature.

### Day 15 (4 h) — Mid-campaign re-baseline + headline check

- Run the full benchmark with all kept optimizations applied. Measure.
- Compute headline result vs original baseline with 95 % CI.
- If you're at or near target: lock the configuration and move to Week 4.
- If not: pick the next 5 experiments from the current profile. Be honest about which knobs are out of reach.

### Validation gate
- [ ] ≥ 20 experiments documented (cumulative)
- [ ] At least 1 system-layer win and 1 policy-layer win measurably contributing
- [ ] At least 5 negative experiments documented
- [ ] At least 1 second-workload validation
- [ ] Headline result measured against original baseline with statistical significance

### Gotchas
- NCCL tuning often gives smaller wins than expected on modern fabrics; don't burn three days chasing 2 %.
- Policy-layer wins (routing, admission) look small in isolation but compound — measure goodput, not throughput in isolation.
- The temptation to claim every experiment helped is real. Insist on attribution; if you can't attribute a win to a specific change, it's noise.

---

## Week 4 — Regression CI, Durability, Rollback Runbooks (20 h)

### Goal
Lock the win in. A 2× win that regresses in 30 days is not a 2× win.

### Day 16 (5 h) — Perf CI implementation

- Build a CI job (GitHub Actions / Jenkins / Buildkite) that:
  - Runs nightly on a representative smoke benchmark
  - Pins hardware via a label (e.g., a specific runner with H100s)
  - Compares against a rolling 14-day baseline window with a statistical test
  - Posts result to a channel + writes a Markdown report
- Implement the comparison logic with proper statistics (don't use a fixed % threshold).

### Day 17 (4 h) — Regression demo

Open a PR that intentionally regresses the metric by ≥ 5 % (e.g., disable FA-3 in one config). Confirm CI catches it in one run. Capture screenshot for the report.

Write `docs/perf-runbook.md`:
- How to repro a CI alert locally
- How to rule out noise (rerun with N=200)
- How to bisect commits
- How to escalate

### Day 18 (4 h) — Durability assessment

`docs/durability.md`: per kept optimization, answer:
- Survives next model release? (Y/N + why)
- Survives next PyTorch / vLLM upgrade? (Y/N + why)
- Survives hardware refresh? (Y/N + why)
- Owner for re-validation when those triggers happen

For non-durable wins, name a follow-up action with an owner and a timeline.

### Day 19 (4 h) — Rollback runbooks + canary plan

For every production-touching change, write a rollback runbook:
- Title (e.g., "Roll back FP8 inference")
- Trigger conditions
- Steps (specific commands or flag flips)
- Verification (how do you know it worked)
- Communication template

Then write a canary plan for the headline change: 1 % → 10 % → 50 % → 100 %, the breaking metric for each gate, who watches.

### Day 20 (3 h) — Reliability SLO check

- Re-run the full benchmark with attention to **secondary metrics**: error rate, p99 latency (if throughput is primary), correctness diff.
- Confirm reliability SLO is maintained or improved.
- If a secondary metric regressed, decide whether to keep or roll back; document.

### Validation gate
- [ ] Perf CI runs nightly with statistical detection
- [ ] Synthetic 5 % regression caught in 1 run (PR + screenshot evidence)
- [ ] `docs/perf-runbook.md` covers repro / bisect / escalate
- [ ] `docs/durability.md` per-optimization survival analysis
- [ ] Rollback runbook per production-touching change
- [ ] Canary plan for headline change
- [ ] Reliability SLO maintained

### Gotchas
- A fixed-threshold CI ("alert if > 5 %") will fire on noise. Use a statistical test against a rolling baseline.
- Durability assessment is the easiest section to write generically and add no value. Be specific: name the upcoming model release, name the planned vLLM version, name the H200 transition if applicable.
- Canary plans that don't define the **breaking metric** are theater.

---

## Week 5 — Executive Narrative, Tech Talk, ADRs, Polish (18 h)

### Goal
The story is the deliverable. Make it landable for a VP and a model team and a FinOps partner.

### Day 21 (4 h) — Executive summary

1 page. Sections:
1. Headline (one sentence with the number)
2. Business win (what the headline means in $ or user impact)
3. How (one paragraph; no jargon)
4. Caveats (one paragraph; honest)
5. Next 6 months

Hand it to a non-engineer. If they can't quote it correctly in 90 seconds, rewrite.

### Day 22 (5 h) — Tech talk

30 min, recorded. Outline:
1. The problem (3 min) — the pain in the org, in numbers
2. Measurement (4 min) — why this metric, what the baseline is, what the noise floor is
3. Top 3 wins (12 min) — one slide each, including the profile screenshot and the attribution
4. Failed experiment (3 min) — what we tried, what it taught us, why we killed it
5. Durability + regression CI (4 min)
6. Next 6 months (2 min)
7. Q&A

Record, watch, re-record once.

### Day 23 (4 h) — ADRs polish

Polish the 6 ADRs from Week 1. Each contains: context, decision, alternatives, consequences, status, date, deciders. At least one ADR captures a decision you would reconsider, with named trigger conditions.

### Day 24 (3 h) — Polish docs + repo hygiene

- Top-level README: headline number, link to exec summary, link to design doc, link to experiment log, link to perf-runbook.
- `perf-tunables.md` lists every flag / env var introduced with default + recommended value + rationale.
- `repro/` directory has at least 3 representative experiments runnable end-to-end.
- Final lint pass: file size, function size, type checks.

### Day 25 (2 h) — Self-assessment + reviewer hand-off

- `docs/self-assessment.md`: score yourself against the rubric. Be honest.
- Identify the dimension where you'd lose the most points and write a paragraph defending why you made the trade.
- Hand the package to at least three reviewers: a perf-specialist staff engineer, a model owner, a FinOps / product partner. Address their comments before final submission.

### Validation gate
- [ ] 1-page executive summary committed
- [ ] Tech talk recorded; slides + executive summary committed
- [ ] 6 ADRs merged with full structure
- [ ] README, perf-tunables, repro guide all in place
- [ ] Self-assessment committed
- [ ] At least 3 reviewers acknowledged

---

## Final Checklist Before Submitting

Tick every box. Each maps to an acceptance criterion in `requirements.md`.

- [ ] Workload pinned in `docs/workload.md`
- [ ] Methodology pre-registered in `docs/methodology.md`
- [ ] Baseline measured with 95 % CI and noise floor reported
- [ ] ≥ 3 profiles committed (baseline, mid, final) with annotated screenshots
- [ ] ≥ 20 experiments in `experiments/experiment-log.md` with full structure
- [ ] ≥ 1 win in each layer (kernel, framework, system, policy)
- [ ] ≥ 5 negative experiments documented
- [ ] Headline result: ≥ 1.5× improvement (or ≥ 30 % reduction) with p < 0.01
- [ ] Reliability SLO maintained; correctness diff within tolerance
- [ ] Perf CI runs nightly; statistical regression detection
- [ ] Synthetic 5 % regression caught in 1 run (PR evidence)
- [ ] `docs/perf-runbook.md`, `docs/durability.md`, rollback runbooks all present
- [ ] Canary plan for headline change
- [ ] 1-page executive summary
- [ ] Tech talk recorded; slides committed
- [ ] 6 ADRs merged
- [ ] No file > 800 LOC; type checks clean

---

## Common Failure Modes — Read Before You Start

1. **Started optimizing before defining the metric.** A 2× win on the wrong metric is worse than a 1.2× win on the right one.
2. **Measured once.** First number was great; second run regressed 30 %. No noise floor → no credibility.
3. **Only used one profiler.** PyTorch profiler said one thing; `nsys` told a different story. The fix was in the layer the missing profiler covered.
4. **Skipped negative experiments.** Repo is full of wins; reviewer asks "what did you try that didn't work?" — silence.
5. **No correctness check.** Inference output diverged; only caught by a customer two weeks post-deploy. Project credibility tanks.
6. **Fixed-threshold CI.** Daily noise of 8 %; threshold at 5 %; alert fires constantly; everyone mutes; the real regression slips through.
7. **Durability section is generic.** "These optimizations should be stable" with no specifics. A reviewer scores it 1.
8. **No rollback runbook for a production change.** First on-call event after launch turns into a 4-hour scramble. Reputation loss.
9. **Tech talk is a tool tour.** Forty minutes of "and we used nsys here, and ncu there, and FA-3 next…" with no story. The VP doesn't quote any of it.
10. **Took on training **and** inference.** Spread too thin; no headline number for either. One project, one target, one metric.

---

## What "Good" Looks Like at the End

A reviewer with 1 hour should be able to:
1. Read the README and quote the headline number and the business win.
2. Open `docs/methodology.md` and validate the statistical methodology.
3. Open `experiments/experiment-log.md` and pick any row to inspect.
4. Open a `.nsys-rep` and follow the annotated bottleneck → fix → verification.
5. Trigger a synthetic regression in CI and watch it caught.
6. Open `docs/durability.md` and find specific named follow-up actions.
7. Watch 5 minutes of the talk and understand the story arc.

If a reviewer can do all 7 — you've passed. If they can do them and say "I would put this on a VP's desk" — you've hit 85+.
