# Project 01: Company-Wide Distributed Training Framework

> **Duration:** 100 hours (4-5 weeks full-time, 8-10 weeks part-time)
> **Scope:** Company-wide platform — used by every ML team
> **Difficulty:** Principal / Expert
> **Track:** Individual Contributor (IC), Principal Engineer
> **Project Type:** Tier 4 — Platform, lasting infrastructure

---

## Overview

You are the principal engineer responsible for **how every ML team in the company trains models**. The current state is fragmented: Team A hand-rolls `torchrun` scripts, Team B uses Ray Train, Team C copy-pasted a DeepSpeed config from a blog post in 2023 and nobody understands it. Three different teams have independently re-discovered the same NCCL bug. Two teams burned a combined $1.4M of cloud credits last quarter on jobs that crashed at hour 47 of 72 with no checkpoint.

Your charter is to design, build, and roll out **one** distributed training framework that all teams converge on. It must support models from **1B to 1T parameters**, span **100–1,000 GPUs** per job, survive **spot interruptions** and full-node failures, integrate with **at least two cluster managers** (Kubernetes + Slurm at minimum), and be observable enough that an on-call engineer can diagnose a stalled training run from a Grafana dashboard at 3am.

This is not a research toy. This is the kind of project that defines a principal engineer's reputation inside a company for the next five years. The framework you build will outlive the person who built it, which means the architectural decisions you defend in your design doc are at least as important as the code.

---

## Why This Project Matters

At principal level, you are no longer paid to write a clever training loop. You are paid to:

1. **Eliminate entire classes of failure** across the org, not just one team's bug
2. **Pick the right level of abstraction** — too low and teams reinvent it, too high and they fight it
3. **Earn trust from skeptical staff engineers** who already have working setups and don't want yours
4. **Make a multi-million-dollar bet** on a foundation (FSDP vs DeepSpeed vs Megatron vs custom) and own the consequences
5. **Communicate** the design upward (executives, capacity planning) and outward (users, on-call, security)

The technical work is real — sharding, comms, checkpoint topology, failure modes — but the **leverage** comes from convergence across the org. If three teams adopt your framework and shut down their own, you saved ~3 engineer-years and a lot of cloud spend. That is the unit of impact for principal IC work.

---

## Learning Outcomes

After completing this project, you will be able to:

### Technical depth
1. **Architect and implement** a distributed training framework integrating FSDP (PyTorch), ZeRO-3 (DeepSpeed), and (optionally) Megatron-style tensor parallelism behind a single user-facing API
2. **Design fault-tolerant** checkpointing with asynchronous, sharded, deduplicated state — surviving spot preemption with <5 min of wasted work per interruption at 256 GPUs
3. **Implement multi-cluster orchestration** spanning at least two regions or two cluster managers (e.g., EKS + on-prem Slurm) with a unified job submission API
4. **Instrument** training jobs with per-rank metrics (NCCL collective time, GPU SM utilization via DCGM, step time, gradient norm, optimizer state size) wired into Prometheus + Grafana + alerting
5. **Optimize** end-to-end so a reference 70B-parameter LLaMA-style model reaches ≥45% MFU on H100s (or the equivalent on your hardware) — and explain every percentage point of the gap
6. **Implement cost controls**: spot/on-demand mixing, gang scheduling, queue priority, per-team budget enforcement, automatic suspension at budget limit

### Principal-level skill
7. **Write a 15–25 page design doc** that survives review by 3+ staff engineers, a director, and a security partner — and shipped without major rewrites
8. **Author 4–6 ADRs** documenting the load-bearing technical bets (sharding strategy, checkpoint format, cluster mgr abstraction, comms backend, observability schema, security boundary)
9. **Plan a phased rollout** that migrates 3+ real teams from their existing setup with zero unplanned training failures attributable to the migration
10. **Run a tech talk** to the wider engineering org explaining the framework, with at least one slide on what you got wrong

---

## Key Questions This Project Answers

You must be able to defend a clear answer to each of these by the end. They will show up in the rubric.

1. **Why this layer?** Why is the framework at the level of "training job runner" instead of (a) a Python library teams import, or (b) a fully managed SaaS like SageMaker / Vertex Training?
2. **Why this sharding stack?** FSDP vs DeepSpeed ZeRO-3 vs Megatron-LM 3D parallelism — for which model sizes and which workloads do you pick which, and what is your fallback when the user's model doesn't fit your defaults?
3. **What is the failure model?** Define exactly which failures you survive transparently, which you survive with restart, which you escalate. What is your RTO for a stalled run? Your RPO (max work lost)?
4. **What does the developer experience look like?** Concretely: from `git push` to "model is training on 256 GPUs across two regions" — how many steps, how many config files, what does failure look like, who do they page?
5. **How do you prove it works?** What is the acceptance test suite that gates every PR to the framework? What is the chaos test schedule? What metric do you put in front of the VP to prove convergence?
6. **What is the migration story?** How does a team running 100-GPU jobs on their hand-rolled `torchrun` cluster move over without a multi-week freeze on their roadmap?
7. **What is the org cost of getting this wrong?** If your sharding choice locks out a future workload (e.g., MoE, FP4) — what's the exit?

---

## Prerequisites

### Required experience
- **8+ years** total engineering, **3+ years** at staff level or equivalent IC scope
- Shipped at least one **distributed training job ≥ 64 GPUs** end-to-end in production
- Hands-on with **PyTorch DDP**, plus at least one of FSDP / DeepSpeed / Megatron-LM
- Deep familiarity with **NCCL** (you have at minimum read NCCL logs in anger)
- Production-quality **Kubernetes** experience (operators, CRDs, scheduling)
- Written at least one **design doc that 5+ engineers reviewed**

### Required completion in this curriculum
- Module 501 (Technical Strategy) — for the design-doc and roadmap framing
- Module 503 (Cross-Org Initiative) — for migration and stakeholder mechanics
- Module 502 (Mentorship & Leadership) — useful but not blocking
- Recommended: read or skim **Megatron-LM**, **ZeRO**, **GPipe**, **PaLM**, and **GSPMD** papers before starting

### Infrastructure assumed available
Either a real environment or a credible simulation:
- ≥ 16 GPUs you control (8× A100 / H100 minimum, ideally 32+ for credible scale claims)
- A Kubernetes cluster with NVIDIA GPU operator + at least one InfiniBand or RoCE-capable interconnect, *or* clear documentation of what you would do with one
- Object storage (S3-compatible) for checkpoints
- Prometheus + Grafana stack
- Budget for ≥ 200 GPU-hours of experimentation (smaller scale on H100 is fine, larger scale on T4/L4 is fine; document the choice)

If you do not have hardware at that scale, you must explicitly justify your simulation strategy (e.g., emulate 256 ranks with 8 GPUs + delay injection on NCCL collectives) in your design doc. Hand-waving is not acceptable at principal level.

---

## Deliverables (Summary — see `deliverables/README.md` for full submission spec)

1. **`design-doc.md`** (15–25 pages) — the load-bearing artifact; will be reviewed line-by-line
2. **ADRs `adr/0001` through `adr/0006`** — minimum 6, one per major irreversible decision
3. **Working framework code** under `src/training_framework/` — pip-installable, type-checked, ≥80% test coverage on the core scheduling and checkpointing modules
4. **Reference workload**: a 7B-parameter model that trains end-to-end on at least 32 GPUs using your framework, with reproducible numbers
5. **Fault-tolerance evidence**: chaos test report — 10 induced failures (preemption, node loss, NCCL hang, OOM, disk full, bad checkpoint, slow node, network partition, host reboot, container OOM-kill) with recovery behavior documented
6. **Migration plan** for 3 real (or realistic mock) teams — written as a runbook, not a brochure
7. **Tech talk**: 30–40 min recorded presentation + slide deck
8. **Cost model spreadsheet** — per-job cost decomposition, plus annual savings projection
9. **Postmortem** of one real failure you induced or observed during the project, written in real postmortem format

---

## Week-by-Week Duration (100 hours total)

| Week | Hours | Focus |
|------|-------|-------|
| 1 | 18 h | Discovery + design doc draft 1 |
| 2 | 22 h | Core framework: scheduler, job spec, FSDP integration |
| 3 | 22 h | Checkpoint topology, fault tolerance, observability |
| 4 | 20 h | Reference workload to convergence, chaos tests, ADRs |
| 5 | 18 h | Migration plan, tech talk, cost model, polish |

Part-time (10 h/week) takes ~10 weeks; the phasing is the same, just spread.

Detailed day-by-day breakdown lives in [`STEP_BY_STEP.md`](./STEP_BY_STEP.md).

---

## Success Criteria

You have completed this project at a passing principal level when **all** of these are true:

### Technical
- A reference 7B (minimum) model trains end-to-end on your framework, ≥ 32 GPUs, ≥ 40% MFU on H100 (or hardware-adjusted equivalent), reproducible by a reviewer in under 1 hour from `git clone`.
- Checkpoint resume after induced node loss completes with **less than 5 minutes** of lost work at 32 GPUs (extrapolated cost target documented for 256 GPUs).
- Framework passes **at least 10/10** chaos scenarios listed in the evidence pack.
- All ADRs explicitly state the alternatives considered and the consequences accepted.

### Design / leadership
- Design doc has been reviewed by **at least 3 senior or staff engineers** with their comments addressed (or explicitly punted with rationale).
- Migration plan walks through **3 concrete teams** with their current setup, the migration steps, the rollback plan, and the success metric.
- Cost model shows a defensible 12-month savings or productivity number — even if conservative.

### Communication
- Tech talk is **recorded** (audio + slides, screen optional) and is comprehensible to a staff engineer outside your direct team.
- At least one ADR explicitly captures a decision you would do differently in hindsight — i.e., honest learning.

### Stretch criteria (for higher scores)
- Adds Megatron-style tensor parallelism *or* MoE-aware routing on top of FSDP and demonstrates a workload that requires it
- Supports a second cluster manager (Slurm) end-to-end, not just architecturally
- Tech talk accepted by an external venue (MLSys, MLOps World, internal company tech all-hands)
- Open-source release with a public README, examples, and at least one external user

---

## Related Lessons

| Lesson | How it feeds this project |
|--------|---------------------------|
| **Module 501 — Technical Strategy** | Framing the design doc, the org-level argument for convergence, the 12-month roadmap |
| **Module 502 — Mentorship & Leadership** | Running design reviews; coaching adopting teams through migration |
| **Module 503 — Cross-Org Initiative** | Stakeholder mapping; aligning Platform, ML Research, Infra, Security, Finance |
| **Module 504 — Open Source / Community** | If you choose to open-source (stretch); learning from FSDP / DeepSpeed / Megatron-LM communities |
| **Module 505 — Long-term Technical Bets** | Picking sharding stack, comms backend, checkpoint format — these are 3–5 year bets |

---

## Rubric Summary

See [`rubric.md`](./rubric.md) for the full grading rubric. High-level dimensions and weights:

| Dimension | Weight | What "Exceeds" looks like |
|-----------|--------|---------------------------|
| Technical depth & correctness | 25 % | Framework demonstrably trains a 7B+ model at scale with reproducible MFU numbers; sharding choices defensible across model sizes |
| Fault tolerance & operations | 20 % | All 10 chaos cases pass; on-call runbook is real; checkpoint topology has a written failure-mode analysis |
| Design doc & ADRs | 20 % | Doc is publishable as an industry tech blog; ADRs honestly document trade-offs and accepted consequences |
| Cross-team adoption / migration | 15 % | 3 teams onboarded (or credible plan to); migration runbook works on first try |
| Cost / business framing | 10 % | Cost model is defensible to a CFO, not just an engineer |
| Communication (tech talk + writing) | 10 % | Talk is well-structured, calibrated to audience, and includes "what I'd do differently" |

Minimum 70 / 100 to pass. 85+ qualifies as portfolio-grade for principal job interviews.

---

## How to Use This Project

This is structured for **self-paced principal-track learners**. The expectation is that you treat it like a real promotion-packet artifact, not a homework assignment.

1. Read this README, [`requirements.md`](./requirements.md), and [`architecture.md`](./architecture.md) end to end before writing any code or any doc.
2. Draft your design doc **first**. Have at least one peer review it before you commit to the implementation.
3. Build incrementally per [`STEP_BY_STEP.md`](./STEP_BY_STEP.md). Resist the urge to skip the chaos testing phase — that's where most of the principal-level signal is.
4. Have at least three people review the final package: one staff engineer, one platform/infra engineer outside your team, and one non-IC stakeholder (manager, PM, finance partner).
5. Treat the tech talk as the final integration test of your understanding — if you can't explain it clearly in 30 minutes, the design is probably not done yet.

Good luck. This is one of the projects that, done well, will end up on the front page of your principal-engineer promotion document.
