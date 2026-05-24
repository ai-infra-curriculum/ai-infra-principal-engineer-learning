# Project 03: Performance Optimization Initiative

> **Duration:** 100 hours (4-5 weeks full-time, 10 weeks part-time)
> **Scope:** Critical-path performance work on a real production system; measurable business win
> **Difficulty:** Principal / Expert
> **Track:** Individual Contributor (IC), Principal Engineer
> **Project Type:** Tier 4 — Performance, with executive narrative and post-launch measurement

---

## Overview

You are the principal engineer the org calls when a system that "just needs to be 2× faster" turns out to be a six-month rabbit hole touching CUDA kernels, scheduler policies, batching strategy, KV-cache layout, network topology, and the model team's pre-tokenization pipeline. The fix isn't a flag flip. The fix is a coordinated, instrumented, multi-layer attack that ends with a number on a slide a VP wants to share at the all-hands.

Your charter is to pick a **single critical-path workload** — typically large-model **training** or large-model **inference serving** — and make it measurably, defensibly, and durably faster, cheaper, or lower-latency. You will choose the target metric *and the business reason*, then run a structured optimization campaign across the full stack: profiling at scale, kernel-level fixes (FlashAttention-3, fused MLPs, custom CUDA / Triton), framework-level fixes (FSDP comms overlap, vLLM continuous batching, speculative decoding, paged-KV), system-level fixes (NCCL tuning, NVLink/IB topology, NUMA pinning), and policy-level fixes (autoscaling, request routing, queueing).

At the end you will not only have produced a 1.5–3× win on your chosen metric; you will have produced the **executive narrative**, the **regression-prevention infrastructure**, and the **rollout plan** that turn a one-time win into a durable shift in the cost or latency curve.

This is the project that, done well, turns into a 30-minute talk you give to the company and a slide in your principal-engineer promotion document. Done poorly, it becomes a benchmark microbenchmark in isolation that doesn't move a single production metric.

---

## Why This Project Matters

At principal level you are paid to:

1. **Move a needle the business actually feels** — $/token, p99 latency, GPU-hours per training run, time-to-first-token, throughput at SLO — not a microbenchmark
2. **See across layers** — the bottleneck is rarely where the first profile points; it's two layers up or one layer down
3. **Defend the methodology** — a 2× number with hand-wavy methodology gets challenged and lost; a 1.5× number with rigorous methodology gets adopted
4. **Build the regression backstop** — performance work is undone by the next model release if you don't lock the win in CI
5. **Tell the story upward** — the same work, told as "we optimized attention" vs "we cut inference cost 32 % at the same SLO", lands very differently

A performance project is also where principal engineers reveal whether they have **taste**. There are always more knobs than time. You have to pick the ones that move the metric *and* are durable *and* are explainable. That's the work.

---

## Learning Outcomes

After completing this project, you will be able to:

### Technical depth
1. **Profile at scale** with `nsys`, `nsys-rep`, `ncu`, the PyTorch profiler, `py-spy`, `perf`, `bpftrace`, and `dcgm-exporter` — and correlate kernel-level traces with system-level metrics
2. **Apply kernel-level optimizations** — FlashAttention-2/3 (or your own), fused MLP / GEMM-bias-GELU, fused RMSNorm, Triton-authored kernels, tensor-core utilization analysis, FP8 with NVIDIA Transformer Engine, INT4/INT8 weight-only quantization (AWQ / GPTQ / SmoothQuant)
3. **Apply framework-level optimizations** for the chosen workload:
   - Training: FSDP comms overlap, gradient accumulation, selective activation checkpointing, microbatch sizing, ZeRO-3 vs FSDP picking, `torch.compile`
   - Inference: vLLM / TGI / TensorRT-LLM, continuous batching, paged-KV cache, speculative decoding (Medusa / EAGLE), chunked-prefill, prefix caching
4. **Apply system-level optimizations** — NCCL `NCCL_ALGO` / `NCCL_PROTO` / `NCCL_NTHREADS`, NVLink vs IB / RoCE topology awareness, NUMA pinning, GPU clock and power policy, container CPU pinning, hugepages
5. **Apply policy-level optimizations** — request routing by sequence-length bucket, autoscaling with predictive headroom, batch admission policy, SLO-aware shedding
6. **Lock in the win** — performance CI with statistical regression detection (Welch's t-test or Bayesian), nightly perf job, runbook for "perf has regressed"

### Principal-level skill
7. **Write a 12–20 page performance design doc** that defines the target metric, the methodology, the experiments, the predicted outcome, the rollback, and the executive narrative
8. **Run a structured experiment campaign** with at least **20 documented experiments**, each with hypothesis, change, before/after, attribution
9. **Build the regression backstop** — perf CI catches a ≥ 5 % regression at p95 within one PR
10. **Tell the story** — a 30-minute talk and a 1-page exec summary that survive translation to a VP and to a model team and to the FinOps partner

---

## Key Questions This Project Answers

You must defend a clear answer to each. They appear in the rubric.

1. **Why this metric?** Why $/token (or p99 TTFT, or MFU, or QPS-at-SLO) and not the other obvious one? What does the business win look like in dollars or user impact?
2. **How will you know?** What is the measurement methodology, what's the experimental control, what's the noise floor, what's the minimum detectable effect?
3. **Where is the bottleneck?** Before doing any optimization, what does the profile tell you? Quantify the cost of each layer.
4. **What's the order?** With 20 candidate optimizations and 100 hours, which 10 do you do, in what order, and why?
5. **What's the durability?** When the model team ships v2 of the model in three months, what % of your wins survive without re-optimization?
6. **What's the rollback?** Every optimization must have a documented rollback. What's the runbook if the optimization breaks correctness in production?
7. **What did you turn down?** A principal engineer says no to good ideas in service of the great ones. What did you punt, and why?

---

## Prerequisites

### Required experience
- **8+ years** total engineering, **3+ years** at staff scope
- Deep familiarity with **PyTorch** (or JAX) and at least one of {FSDP, DeepSpeed, vLLM, TensorRT-LLM}
- Hands-on with **NVIDIA Nsight Systems** and **Nsight Compute**, or the AMD / Intel equivalent for your hardware
- Comfortable reading **CUDA kernels** (you don't need to write production CUDA from scratch, but you need to understand the source of FlashAttention)
- At least one production system you've operated where p99 latency or $/unit mattered
- Written at least one design doc reviewed by an org perf lead or distinguished engineer

### Required completion in this curriculum
- Module 501 (Technical Strategy)
- Module 502 (Mentorship & Leadership)
- Module 503 (Cross-Org Initiative) — important: optimizing a system you don't own is a stakeholder problem first
- Recommended: skim the FlashAttention, FlashAttention-2/3, vLLM, PagedAttention, Speculative-Decoding, Medusa, FP8 (NVIDIA TE), and Continuous Batching (Orca / TGI) papers before starting

### Infrastructure assumed available
Either real or credibly simulated, but **the rubric strongly favors real**:
- A reference workload you can hit hard without breaking production — either a staging clone, a canary fleet, or a load generator pointed at a dedicated test fleet
- ≥ 8 GPUs you control (H100 strongly preferred for training optimization; A100 / L40S / L4 acceptable for inference optimization; document your hardware)
- Permission to install profilers (`nsys`, `ncu`, `dcgm-exporter`, `py-spy`) on the target nodes
- Object storage for traces (`.nsys-rep` files get big — budget 100s of GB)
- A way to run a load test reproducibly (`locust`, `vegeta`, `wrk2`, or a domain-specific generator)
- For inference: a corpus of representative requests with realistic prompt-length distribution
- For training: a non-toy reference model and dataset (LLaMA-3 8B / 70B class is ideal)

If you don't have hardware at that scale, justify the substitute (e.g., A10 24GB instead of H100 80GB; halve batch sizes; document the extrapolation method). Hand-waving is not acceptable at principal level.

---

## Deliverables (Summary — see `deliverables/README.md` for full submission spec)

1. **`design-doc.md`** (12–20 pages) — target metric, methodology, experiment campaign plan, executive narrative
2. **ADRs `adr/0001` through `adr/0006`** — at minimum 6 (metric choice, methodology, top 3 optimization bets, regression-prevention strategy)
3. **Experiment log** with **≥ 20 documented experiments**, each with hypothesis, change, before/after with confidence interval, attribution
4. **Profiles** — at least 3 `nsys` / `ncu` traces with annotated screenshots showing the bottleneck and the fix
5. **Optimization code** — kernels, framework patches, config changes, infra changes, all in version control with PR descriptions explaining the *why*
6. **Performance CI** — nightly perf job with statistical regression detection; catches a synthetic ≥ 5 % regression in a smoke test
7. **Before/after report** — the headline number with full methodology section; defensible to a peer at NVIDIA or Meta
8. **Executive narrative** — 1-page summary plus a 30-minute talk for an internal audience
9. **Rollback runbook** — for each optimization that touches production, the rollback steps and the verification
10. **Durability assessment** — how many of these wins survive the next model release; named action items for the next 6 months

---

## Week-by-Week Duration (100 hours total)

| Week | Hours | Focus |
|------|-------|-------|
| 1 | 18 h | Target selection, methodology, baseline, design doc draft 1 |
| 2 | 22 h | Profile-driven optimization round 1 (kernel + framework) |
| 3 | 22 h | Optimization round 2 (system + policy); experiment campaign in earnest |
| 4 | 20 h | Regression CI, durability tests, rollback runbooks |
| 5 | 18 h | Executive narrative, tech talk, ADRs, polish |

Part-time at 10 h/week takes ~10 weeks; same phasing.

Day-by-day in [`STEP_BY_STEP.md`](./STEP_BY_STEP.md).

---

## Success Criteria

You have completed this project at a passing principal level when **all** of these are true:

### Technical
- A **single named metric** moved by **≥ 1.5×** (or **≥ 30 %** if the metric is a cost / latency reduction), with full methodology and confidence intervals
- **≥ 20 documented experiments**, with at least 10 producing measurable wins and the rest documented as "tried, didn't help, here's why"
- At least one **kernel-level** optimization measurably contributing to the headline number (FlashAttention-3, custom Triton kernel, FP8 enablement, fused op, etc.)
- At least one **system-level** optimization measurably contributing (NCCL tuning, topology, NUMA, paging, network)
- At least one **policy-level** optimization measurably contributing (batching, routing, autoscaling, admission control)
- **Performance CI** catches a synthetic ≥ 5 % regression on a representative smoke test

### Methodology
- Measurement methodology is documented, including: workload definition, control vs treatment, noise floor, sample size, confidence interval, blinding (or honest "could not blind because X")
- Profiles support the before/after claims — a reviewer can open a `.nsys-rep` and see what changed

### Communication
- 1-page executive summary that a non-engineer can read in 90 seconds and quote correctly
- 30-minute talk recorded; tells the story (problem → measurement → top 3 wins → durability → what we'd do next), not a tool tour
- One slide on a failed optimization, what it taught you, and why you killed it

### Durability
- Regression CI runs at least nightly with statistical detection
- A durability section names which wins survive the next model release and which don't
- Rollback runbook exists for every production-touching change

### Stretch criteria (for higher scores)
- Multiple metrics moved simultaneously (e.g., $/token down 30 % **and** p99 latency down 20 % at the same SLO)
- A kernel you authored or substantially modified, with the PR open against the upstream project
- A blog post or external talk submitted (MLSys, GTC, internal tech all-hands)
- An adopted convention (e.g., "all new inference services use vLLM with these flags") that came out of this work

---

## Related Lessons

| Lesson | How it feeds this project |
|--------|---------------------------|
| **Module 501 — Technical Strategy** | Picking the right metric; tying perf wins to business outcomes |
| **Module 502 — Mentorship & Leadership** | Building the next perf engineer through the campaign |
| **Module 503 — Cross-Org Initiative** | Coordinating with model, infra, and product teams whose systems you're touching |
| **Module 505 — Long-term Technical Bets** | Choosing FP8 / FP4, MoE serving, speculative decoding as durable directions |

---

## Rubric Summary

See [`rubric.md`](./rubric.md) for the full grading rubric. High-level:

| Dimension | Weight | What "Exceeds" looks like |
|-----------|--------|---------------------------|
| Headline result & methodology | 25 % | ≥ 2× win with publishable-quality methodology; profiles support every claim |
| Profiling depth & cross-layer reach | 20 % | Kernel + framework + system + policy all measurably contribute; profiles correlate across layers |
| Regression prevention & durability | 15 % | Perf CI catches a 5 % regression at p95; durability section names which wins survive a model bump |
| Optimization campaign rigor | 15 % | ≥ 20 experiments with hypothesis, statistical analysis, and the negatives kept |
| Executive narrative | 15 % | 1-page exec summary + 30-min talk land for a VP and a model team and a FinOps partner |
| Rollback & operational safety | 10 % | Every production-touching change has a tested rollback; perf SLO + reliability SLO both defended |

Minimum **70 / 100** to pass. **85+** is portfolio-grade.

---

## How to Use This Project

This is structured for **self-paced principal-track learners** treating the work as a real critical-path optimization initiative.

1. Read this README, [`requirements.md`](./requirements.md), and [`architecture.md`](./architecture.md) end to end before touching a profiler.
2. **Pick the metric and the baseline first.** The single most common failure mode is starting to optimize before defining a defensible measurement. If you can't say "p99 TTFT at QPS=128 on prompt-length-256 with sample N=10 000, baseline 245 ms ± 12 ms", you have no business changing a kernel yet.
3. Build incrementally per [`STEP_BY_STEP.md`](./STEP_BY_STEP.md). Resist the urge to skip Week 1's measurement work; that's where most of the principal-level signal is.
4. Have at least three reviewers on the final package: one perf-specialist staff engineer, one model owner, one FinOps / product partner.
5. Treat the executive narrative as a first-class deliverable, not an afterthought. Most performance work loses its impact in the translation; the translation is the job.

Good luck. The orgs that get this right turn a quarter of perf engineering into a year of margin or a year of headroom. That's the leverage of principal-grade perf work.
