# Requirements — Project 03: Performance Optimization Initiative

This document defines what the optimization initiative must do and must not do, the constraints, the assumptions, and the acceptance criteria a reviewer will check.

Requirements use **MoSCoW** prioritization: **M**ust, **S**hould, **C**ould, **W**on't.

---

## 1. Functional Requirements

### 1.1 Target Selection & Baseline (Must)

- **M-FR-1**: A **single named metric** is selected as the headline outcome. It must be a metric the business cares about. Examples (pick one, not all):
  - Inference: `$/M tokens served`, `p99 TTFT @ QPS=N`, `p99 TBT (time-between-tokens) @ QPS=N`, `goodput @ SLO`, `tokens/sec/GPU @ batch=B`
  - Training: `MFU on workload W at scale S`, `tokens/sec/GPU at world_size=N`, `wall-clock to convergence on benchmark dataset D`, `$/epoch`
- **M-FR-2**: The metric is defined precisely enough that a peer at NVIDIA or Meta could reproduce: workload, model, dataset / request distribution, hardware, software stack, measurement window, units.
- **M-FR-3**: A **baseline** measurement exists with a confidence interval (≥ 95 % CI) and a documented noise floor.
- **M-FR-4**: A **target** is set with rationale, not pulled from the air. E.g., "30 % reduction in $/M tokens" must connect to either the business goal or a defensible analytical ceiling (memory bandwidth, FLOPS, comms BW).
- **S-FR-5**: A **secondary metric** is tracked as a guard against regressing what you didn't measure (e.g., if optimizing throughput, track p99 latency as guard).

### 1.2 Profiling (Must)

- **M-FR-6**: At least **3 profiles** captured: one at baseline, one mid-campaign, one at final state. Each is committed (or linked) as `.nsys-rep` / `.ncu-rep` / PyTorch profiler traces.
- **M-FR-7**: For each profile, an annotated screenshot or markdown writeup identifies the top 3 cost centers.
- **M-FR-8**: Cross-layer correlation: at least one profile correlates a kernel-level finding (Nsight Compute) with a system-level signal (DCGM SM utilization, NCCL bandwidth, NUMA cross-socket traffic) and a framework-level signal (PyTorch profiler op-level).
- **S-FR-9**: A "what does idle look like" profile — bubbles in the timeline are identified and explained (host stall, comms wait, kernel launch latency, allocator stall, page fault).

### 1.3 Optimization Campaign (Must)

- **M-FR-10**: **≥ 20 documented experiments**, each with:
  - hypothesis (one sentence)
  - intervention (the change, in code or config)
  - measurement (before / after with CI)
  - attribution (this change → this much of the delta)
  - decision (keep / revert / further investigate)
- **M-FR-11**: At least one optimization in **each** of these layers measurably contributes to the headline number:
  - **Kernel** (FlashAttention, fused op, Triton, FP8/INT4, custom CUDA)
  - **Framework** (FSDP comms overlap, vLLM continuous batching, paged-KV, speculative decoding, `torch.compile`, ZeRO config, microbatch sizing)
  - **System** (NCCL / RCCL tuning, network topology, NUMA, hugepages, kernel sysctls, GPU clock policy)
  - **Policy** (request routing, batching policy, autoscaling, admission control, queueing)
- **S-FR-12**: At least one experiment is **negative** — the hypothesis was wrong, the change reverted. Negative results are kept and counted.
- **C-FR-13**: A custom kernel (Triton or CUDA) authored or substantially modified.

### 1.4 Methodology & Statistics (Must)

- **M-FR-14**: Each headline measurement has at minimum: sample size, mean, std, 95 % CI; a t-test (or Bayesian equivalent) comparing baseline vs treatment with p-value or posterior interval; statement of effect size.
- **M-FR-15**: Confounders are addressed: experiments run on the same hardware, same software, same workload distribution, same time-of-day (or randomized), with warmup excluded.
- **M-FR-16**: The minimum detectable effect (MDE) for the chosen sample size is documented.
- **S-FR-17**: Where possible, experiments are **blinded** (the load generator does not know which version of the server it's hitting). Where impossible, the lack of blinding is acknowledged.

### 1.5 Regression Prevention (Must)

- **M-FR-18**: A **performance CI** job runs on at least a nightly schedule against a representative smoke workload.
- **M-FR-19**: The CI detects a synthetic ≥ 5 % regression at p95 (introduced for the demo) within a single CI run.
- **M-FR-20**: When the CI detects a regression it: posts to a channel (Slack / Teams / email), opens an issue with a link to the failing run, and records the offending commit range.
- **M-FR-21**: A **perf runbook** exists: what to do when CI is red. Includes: how to repro locally, how to rule out noise, how to bisect, how to escalate.
- **S-FR-22**: The CI tracks the metric over time (chart per commit / per day) so drift is visible even when no single CI run trips the threshold.

### 1.6 Durability (Must)

- **M-FR-23**: A **durability assessment** documents, per optimization, what survives the next model release / dependency upgrade / hardware refresh, and what doesn't.
- **M-FR-24**: For non-durable wins, a follow-up action with an owner and a timeline is named.
- **S-FR-25**: At least one optimization is tested against a **second** workload (different model size, different request distribution, different hardware) and the result documented.

### 1.7 Operational Safety (Must)

- **M-FR-26**: Each production-touching change has a documented **rollback** (specific commands or config) with a verification step.
- **M-FR-27**: A **canary** plan is defined for at least one of the headline optimizations: how it rolls out (1 % → 10 % → 50 % → 100 %), what metric breaks the canary, who watches.
- **M-FR-28**: A **reliability SLO** is named alongside the perf SLO. A perf win that breaks the reliability SLO is not a win.
- **S-FR-29**: For each optimization, the worst-case failure mode is named (e.g., "FP8 enablement: numeric divergence on long sequences; mitigation: per-tensor scaling + sanity-check on first deploy").

### 1.8 Communication (Must)

- **M-FR-30**: A **1-page executive summary** that a non-engineer can read in 90 seconds, with the headline number, the business win, the caveats.
- **M-FR-31**: A **30-minute talk** for an internal audience. Story arc: problem → measurement → top 3 wins → durability → what's next. Includes one failed-experiment slide.
- **M-FR-32**: ≥ 6 ADRs covering: metric choice, measurement methodology, top 3 optimization bets, regression-prevention strategy, durability strategy, rollback model.

---

## 2. Non-Functional Requirements

### 2.1 Performance (the metric itself) (Must)

| ID | Requirement | Target |
|----|-------------|--------|
| M-NFR-1 | Headline improvement on the named metric | ≥ 1.5× (or ≥ 30 % reduction if metric is cost / latency) |
| M-NFR-2 | Statistical significance | p < 0.01 on a paired test or 99 % CI excludes baseline |
| S-NFR-3 | Improvement is durable across at least one workload variation | ≥ 70 % of headline gain preserved |
| C-NFR-4 | Multiple metrics improved (no regressions) | e.g., throughput up AND p99 latency down |

### 2.2 Reliability (Must)

- **M-NFR-5**: Reliability SLO maintained — the optimization does not increase error rate or correctness divergence beyond a named threshold.
- **M-NFR-6**: For inference optimizations: numeric correctness validated against a reference (e.g., logits diff < ε on a held-out set; downstream task accuracy diff < δ on a held-out eval).
- **M-NFR-7**: For training optimizations: convergence behavior preserved — loss curve within a documented tolerance of baseline at matched step.

### 2.3 Reproducibility (Must)

- **M-NFR-8**: Every benchmark in the experiment log has a `repro/<exp-id>/` directory with the exact command, the exact image/commit, the workload spec, and the expected output range.
- **M-NFR-9**: A reviewer can rerun at least 3 representative experiments end-to-end on a fresh machine following the README.
- **M-NFR-10**: Profiler traces are committed (or linked with a stable URL) and openable with the named tool version.

### 2.4 Maintainability (Must)

- **M-NFR-11**: Optimization patches are landed (or PR'd) against the upstream they belong to (your codebase or the OSS project), not held in a private branch.
- **M-NFR-12**: Kernel code (if any) has a unit test for numerics and a benchmark.
- **M-NFR-13**: All config flags / env vars introduced for performance are documented in a single `perf-tunables.md`.
- **M-NFR-14**: No file > 800 LOC. Functions ≤ 50 LOC.

---

## 3. Constraints

- **C-1**: You may not select a metric that is not on someone's roadmap or scorecard. "Made this microbenchmark go brrrr" is not a principal-grade target.
- **C-2**: You may not optimize correctness away. Reference parity must be maintained within a documented tolerance.
- **C-3**: You may not measure once and call it a win. Every headline number has CIs.
- **C-4**: You may not roll an optimization to production without a documented rollback.
- **C-5**: You may not blame the model team / infra team / OSS upstream for a perf gap without proposing (and ideally landing) a fix.
- **C-6**: You may not use a proprietary, non-reproducible benchmark for the headline. The workload must be reproducible by a reviewer.
- **C-7**: You may not skip the negative experiments. Killed ideas count toward the experiment log.

---

## 4. Assumptions

You may assume the following without further justification. If absent, document the substitute.

- **A-1**: A reference workload exists (real production traffic shape, or a credible synthetic).
- **A-2**: Hardware access is sufficient for at least 3 simultaneous experiment configurations to run independently.
- **A-3**: A staging or canary environment exists where production-touching changes can be validated before full rollout.
- **A-4**: Profilers (`nsys`, `ncu`, `dcgm-exporter`, `py-spy`) can be installed on target nodes.
- **A-5**: At least 100s of GB of storage available for traces.
- **A-6**: For inference work: a load generator (`locust` / `vegeta` / `wrk2` / domain-specific) and a corpus of representative requests.
- **A-7**: For training work: a non-toy reference model (LLaMA-style, OPT-style, BERT-large, T5-large class minimum).

---

## 5. Out of Scope (Won't)

To keep this project at 100 hours and not 1,000:

- **W-1**: You will not optimize **two** systems simultaneously. Pick one. Inference *or* training.
- **W-2**: You will not build a new ML framework. Patches and configurations only.
- **W-3**: You will not redesign the org's capacity planning. Cost wins are reported, not implemented as chargeback.
- **W-4**: You will not solve cross-cloud bin-packing. Single-cloud / single-cluster focus.
- **W-5**: You will not change the model. Quantization and decoding are model-adjacent; pretraining changes (architecture, data) are out.
- **W-6**: You will not build a new profiler. Use the existing stack.
- **W-7**: You will not solve cold-start at scale unless it is the headline metric.
- **W-8**: You will not promise improvements that depend on hardware you don't have ("on H200 this would be ___").

---

## 6. Acceptance Criteria

A reviewer should be able to mechanically check these.

### A. Methodology is sound
1. The README states the metric, the workload, the hardware, the software stack, the measurement window, the sample size.
2. `docs/methodology.md` describes control, treatment, randomization, blinding (or honest non-blinding), and the noise floor.
3. Baseline measurement has a 95 % CI; CI is non-degenerate.

### B. The campaign happened
4. `experiments/experiment-log.md` lists ≥ 20 experiments with hypothesis / change / measurement / attribution / decision.
5. At least 5 experiments are negative (tried, didn't help, kept the data).
6. At least one optimization in each of {kernel, framework, system, policy} layers measurably contributes to the headline.

### C. Profiles support the claims
7. ≥ 3 profiles committed or linked. Annotated screenshots in `profiles/`.
8. At least one cross-layer correlation written up.
9. A reviewer can open at least one profile and reproduce the bottleneck identification.

### D. Headline result
10. The headline metric moved by ≥ 1.5× (or ≥ 30 % reduction).
11. Statistical significance documented (p < 0.01 or equivalent).
12. Reliability SLO maintained; correctness within documented tolerance.

### E. Regression prevention
13. Perf CI runs at least nightly.
14. A synthetic ≥ 5 % regression is detected within one CI run (proven via a demo PR).
15. Perf runbook exists with bisection guidance.

### F. Durability
16. `docs/durability.md` per-optimization survival analysis exists.
17. At least one optimization has been tested against a second workload variation.
18. Follow-up actions named with owners for non-durable wins.

### G. Operational safety
19. Rollback runbook per production-touching change.
20. Canary plan for ≥ 1 optimization, with breaking metric named.

### H. Communication
21. 1-page executive summary in `docs/exec-summary.md`.
22. 30-min talk recorded; slides committed.
23. ≥ 6 ADRs merged with full structure.

### I. Quality bar
24. `mypy` clean on any new public API.
25. Kernel numerics tested.
26. No file > 800 LOC.

---

## 7. Dependencies on Other Teams (For the Plan, Not the Code)

For the design doc and campaign plan, explicitly identify dependencies on:

- **Model owners** — for the reference model, the correctness threshold, the eval bar
- **Infra / Platform team** — for hardware allocation, profiler privileges, network changes
- **SRE / Production engineering** — for canary infra, alerting, rollback infra
- **FinOps / Finance** — for the $-per-unit price book; for translating tokens-per-second-per-GPU into dollars
- **Security** — if your optimization introduces a new dependency, kernel module, or runtime
- **Product** — for the SLO that frames "win at SLO" vs "win in a vacuum"

Naming real people is not required; naming roles, responsibilities, and decision rights is.

---

## 8. Glossary

- **MFU** — Model FLOP Utilization
- **HFU** — Hardware FLOP Utilization (often confused with MFU; clarify which you mean)
- **TTFT** — Time To First Token
- **TBT** — Time Between Tokens
- **Goodput** — useful throughput meeting SLO
- **Paged KV** — paged attention KV-cache, pioneered by vLLM
- **Continuous batching** — request-level batching where new requests join an in-flight batch
- **Speculative decoding** — draft + verifier model, accept-on-match
- **Medusa / EAGLE** — speculative-decoding variants
- **FlashAttention 2/3** — IO-aware exact attention kernel families
- **NCCL** — NVIDIA Collective Communications Library
- **DCGM** — NVIDIA Data Center GPU Manager
- **NUMA** — Non-Uniform Memory Access
- **CI confidence interval** — distinct from CI continuous integration; both used here, context-sensitive
- **MDE** — Minimum Detectable Effect
