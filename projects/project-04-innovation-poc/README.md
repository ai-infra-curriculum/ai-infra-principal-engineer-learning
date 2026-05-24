# Project 04: Technical Innovation POC

> **Duration:** 80 hours (3-4 weeks full-time, 8 weeks part-time)
> **Scope:** End-to-end proof of concept of an emerging technique, de-risked for production adoption
> **Difficulty:** Principal / Expert
> **Track:** Individual Contributor (IC), Principal Engineer
> **Project Type:** Tier 4 — R&D-flavored, with explicit go/no-go criteria

---

## Overview

Every quarter a new technique arrives in the literature that *sounds* like it could change how your org trains, serves, or operates models. **FP4 training**. **MoE serving at the edge of feasibility**. **Speculative decoding with EAGLE-2**. **Diffusion LLMs**. **Agentic ML pipelines with self-healing loops**. **Continuous learning via online RLHF**. **State-space models replacing transformers for long context**.

Some are real. Some are mirages. Most are real for a narrow band of conditions you don't quite have. The principal-level question is **not** "is this technique impressive?" — it's "for our system, on our hardware, with our SLOs, at our scale, *will it work, when, and at what cost?*"

Your charter is to run a structured, time-boxed **proof of concept** of a single emerging technique, ending in a defensible **go / no-go / not-yet** recommendation backed by data, with the next 6 months of work named if "go" and the trigger conditions named if "not-yet."

This is not "build a demo." This is **principal-grade de-risking**: convert ambiguity into a written decision your CTO, VP-Eng, or distinguished-engineer peer would sign off on without asking you to "go deeper." The deliverable is a *recommendation*, supported by code and measurement.

---

## Why This Project Matters

At principal level you are the org's **filter** for emerging techniques. Every direction you bless costs the org months of engineering investment. Every direction you reject costs the org an opportunity it might not realize until a competitor ships first. Both errors are expensive.

Done well, an innovation POC:

1. **Resolves a strategic ambiguity** the org can't otherwise resolve in a meeting room
2. **Establishes the methodology** other engineers will use to evaluate the next technique
3. **Lands a recommendation** that survives technical review *and* product review *and* finance review
4. **Reduces the org's regret surface** — either "we picked this up early" or "we knew when to pass"
5. **Builds the principal engineer's credibility** as the one who turns "interesting paper" into "yes/no/when"

A principal engineer who can't run a POC is a senior engineer with extra title. A principal engineer who can repeatedly run POCs that land with credibility is who the CTO calls.

---

## Learning Outcomes

After completing this project, you will be able to:

### Technical depth
1. **Implement an emerging technique end-to-end** at a credible scale — not a notebook toy, but a runnable system someone else could pick up
2. **Adapt the technique** to your hardware, your model, your data, your latency budget — and understand which adaptations preserve the paper's claims and which break them
3. **Measure rigorously** — establish baselines that translate to *your* production, not the paper's benchmarks; quantify the technique's win, cost, and risk
4. **Bridge research and production** — identify what's missing for production-readiness (correctness checks, observability, rollback, eval harness, on-call surface)
5. **Apply the technique under stress** — measure not just the headline case but the cases the paper didn't optimize for

### Principal-level skill
6. **Time-box rigorously** — 80 hours, real go/no-go at the end, no "let me just try one more thing"
7. **Write a 10–18 page POC report** with explicit recommendation, evidence, risk register, and named follow-up actions
8. **Pre-register the success criteria** — what counts as "go", "no-go", "not-yet" must be written before the experiments run
9. **Defend the recommendation** to a skeptical reviewer — answering "what could change your mind?" with named, measurable triggers
10. **Hand the project off** — either to a productionization team (if go) or to a watch list (if not-yet), with the next reviewer named

---

## Key Questions This Project Answers

You must defend a clear answer to each. They appear in the rubric.

1. **Why this technique now?** Why is the org better off spending 80 hours on this than on three other candidates? What changed in the last 6 months that made it worth doing?
2. **What does "works" mean?** Not the paper's metric — *your* metric, on *your* workload, with *your* SLOs. Pre-register.
3. **Where does it break?** Every technique has a regime where it falls over. Find and document at least one.
4. **What's missing for production?** A working POC is not a production system. Name the gaps. Estimate the cost to close them.
5. **What's the alternative cost?** If you say "no-go", what's the opportunity cost? If you say "go", what does this displace in your roadmap?
6. **What's your confidence?** 90 %? 60 %? Confidence must be calibrated, not signaled. State the conditions that would change it.
7. **Who owns it after this project?** Hand-off is not implicit. Name the team and the next decision point.

---

## Candidate Techniques (Pick One)

You should pick a technique that is (a) emerging, (b) plausibly useful to your org, (c) reachable with 80 hours of work at your scale. The list below is illustrative; pick from it or propose a comparable one.

### Training-side
- **FP4 training with NVIDIA Transformer Engine** — 2× memory, ~1.5× compute on Blackwell-class hardware; numerics fragile
- **MoE training at moderate scale** (8–32 experts) — capacity, comms, routing, expert-balance loss
- **Hybrid 3D parallelism with FSDP + TP** for trillion-scale models
- **Continuous learning / online RLHF** — short-horizon update loops on production traffic
- **DPO / KTO / IPO** instead of full RLHF for alignment

### Inference-side
- **Speculative decoding with EAGLE-2 / Medusa-2** — adaptive draft tree, acceptance-aware
- **FP8 inference** with Transformer Engine at production SLO
- **AWQ / GPTQ INT4** weight-only quantization at near-original accuracy
- **Continuous batching at the edge of in-flight token budget** (vLLM tuning at extreme concurrency)
- **Disaggregated prefill/decode** — separate clusters per phase
- **MoE serving** — expert placement, cross-host routing, cold-start

### Architecture-side
- **Mamba / SSM hybrid** for long-context tasks the transformer struggles with
- **RWKV** or another linear-attention variant for cost-constrained deployment
- **Hybrid retrieval-augmented generation with learned retrievers**
- **Latent-space reasoning models** (e.g., Coconut-style)

### Pipeline / agentic
- **Agentic ML pipelines** — multi-agent orchestrations with retry / self-critique / verification
- **Self-healing inference pipelines** — degrade gracefully across model versions / fallback chains
- **Automated red-teaming pipelines** with adversarial generation + judge models
- **Eval automation harnesses** that catch silent regressions in production

### Constraint
Whatever you pick, it must be (1) emerging in the last 12–18 months (no "is RAG worth doing"), (2) something the org could plausibly adopt within 12 months if you say "go", (3) something you can credibly measure on your hardware. Document the choice in `docs/technique-selection.md` with a one-page comparison against 2 alternatives.

---

## Prerequisites

### Required experience
- **8+ years** total engineering, **3+ years** at staff scope
- Hands-on with PyTorch / JAX, plus the toolchain relevant to your chosen technique
- Comfort reading recent papers and reproducing core claims
- At least one production system you've operated end-to-end
- Written at least one design doc with a recommendation that survived executive review

### Required completion in this curriculum
- Module 501 (Technical Strategy)
- Module 505 (Long-term Technical Bets) — **especially relevant**
- Module 502 (Mentorship & Leadership)

### Infrastructure assumed available
Either real or credibly simulated:
- Hardware appropriate to the technique (e.g., H100 for FP8 / FP4; A100 / L40S OK for many inference techniques; check the paper's hardware before committing)
- A reference workload (model + data + load profile) from your org or a credible substitute
- Tooling for measurement (profilers, eval harnesses, load generators)
- Permission to install pre-release / nightly versions of frameworks where required
- Object storage for traces, eval outputs, and checkpoints

If you don't have hardware at that scale, justify the substitute and the extrapolation method. Hand-waving is not acceptable at principal level.

---

## Deliverables (Summary — see `deliverables/README.md` for full submission spec)

1. **`technique-selection.md`** — 1 page; why this technique, what alternatives were considered
2. **`design-doc.md`** (10–18 pages) — POC plan, success criteria pre-registered, risk register
3. **ADRs `adr/0001` through `adr/0005`** — at minimum 5 (technique choice, measurement methodology, evaluation harness, integration boundary, recommendation framing)
4. **Working POC code** under `src/poc/` — pip-installable; reproducible by a reviewer
5. **Eval harness** — automated; runs the technique against agreed evals; produces a structured report
6. **Experiment log** with **≥ 12 documented experiments**, each with hypothesis, change, measurement, decision
7. **Stress-test report** — at least 3 conditions where the technique is *expected* to break, tested and documented
8. **POC report** — the deliverable; 8–14 pages; opens with recommendation, supports with evidence
9. **Productionization gap analysis** — what's missing if you say "go"; estimated cost per gap
10. **Tech talk** — 25–40 min recorded talk + slides explaining the technique, the experiments, the recommendation
11. **Hand-off doc** — who owns next, what their first 30 days look like

---

## Week-by-Week Duration (80 hours total)

| Week | Hours | Focus |
|------|-------|-------|
| 1 | 16 h | Technique selection, success criteria, baseline, design doc draft 1 |
| 2 | 20 h | POC implementation; first experiments; eval harness |
| 3 | 22 h | Stress tests, scale-up experiments, productionization gap analysis |
| 4 | 22 h | Recommendation, POC report, ADRs, tech talk, hand-off |

Part-time at 10 h/week takes ~8 weeks; same phasing.

Day-by-day in [`STEP_BY_STEP.md`](./STEP_BY_STEP.md).

---

## Success Criteria

You have completed this project at a passing principal level when **all** of these are true:

### Technical
- POC code is **runnable** by a reviewer from `git clone` within 60 minutes; produces the headline result within a documented hardware/time budget
- **≥ 12 experiments** documented, each pre-registered against the success criteria
- At least **3 stress conditions** tested (paper's regime + at least 2 outside it)
- An **eval harness** runs automatically and produces a structured report; not a one-off notebook
- The headline result is reported with confidence intervals and the noise floor

### Recommendation
- A clear **go / no-go / not-yet** recommendation with **named, measurable** trigger conditions for "not-yet"
- Recommendation answers: "what's the next 6 months if go", "what's the opportunity cost if no-go", "what's the alternative if not-yet"
- **Confidence stated and calibrated** ("70 % confident go; 30 % chance the second-workload result reverses the call")
- **Productionization gap analysis** has dollar / engineer-week estimates per gap

### Communication
- POC report **opens with the recommendation** (not buried at the end)
- 30-minute talk recorded; tells the story (why this technique, what we tried, what we found, recommendation, what we didn't try and why)
- One slide on what surprised you — POCs that produce no surprises are usually shallow

### Stretch criteria (for higher scores)
- A blog post or talk submitted externally (MLSys workshop, NeurIPS workshop, internal tech all-hands)
- A reproduction commit upstreamed to the technique's original repo or to a community benchmark
- The recommendation includes a **negative result** that saves the org from a wrong bet (worth as much as a positive go)
- The eval harness is reused by a second team in the org for a related technique

---

## Related Lessons

| Lesson | How it feeds this project |
|--------|---------------------------|
| **Module 501 — Technical Strategy** | Framing the recommendation; tying technique to org roadmap |
| **Module 502 — Mentorship & Leadership** | Walking junior engineers through the POC; succession planning if go |
| **Module 505 — Long-term Technical Bets** | The whole project is one of these |
| **Module 503 — Cross-Org Initiative** | Hand-off to a productionization team |
| **Module 504 — Open Source / Community** | Upstreaming reproductions; community credibility |

---

## Rubric Summary

See [`rubric.md`](./rubric.md) for the full grading rubric. High-level:

| Dimension | Weight | What "Exceeds" looks like |
|-----------|--------|---------------------------|
| Technique implementation & faithfulness | 20 % | POC reproduces paper's headline within published tolerance and runs on your stack |
| Measurement methodology & evaluation | 20 % | Pre-registered success criteria; rigorous baselines; eval harness reusable |
| Stress testing & boundary analysis | 15 % | At least 3 conditions outside the paper's regime tested; failure modes named |
| Recommendation quality & calibration | 20 % | Recommendation is unambiguous, time-bounded, with named trigger conditions and calibrated confidence |
| Productionization gap analysis | 15 % | Each gap has owner, cost estimate, and dependency mapping |
| Communication & hand-off | 10 % | Talk + report land for a CTO-level audience; hand-off names next owner and next decision point |

Minimum **70 / 100** to pass. **85+** is portfolio-grade.

---

## How to Use This Project

This is structured for **self-paced principal-track learners** treating the POC as a real strategic-de-risking exercise, not an exploration project.

1. Read this README, [`requirements.md`](./requirements.md), and [`architecture.md`](./architecture.md) end to end before starting any implementation.
2. **Time-box rigorously.** 80 hours is a hard limit. Going to 120 to "really nail it" is the failure mode this project teaches you to avoid.
3. **Pre-register the success criteria.** A go/no-go decided after the experiments is hindsight rationalization; a go/no-go decided before is methodology. Commit the criteria to git before you run an experiment.
4. Build incrementally per [`STEP_BY_STEP.md`](./STEP_BY_STEP.md). Resist the urge to do "one more experiment" past the time box; instead, write that as a follow-up in the hand-off.
5. Have at least three reviewers on the final package: one technical (a peer who could redo the POC), one strategic (a leader who would act on the recommendation), one productionization (the team that would own it if go).
6. Treat the POC report as the deliverable — the code is supporting evidence, not the artifact.

Good luck. The principal engineer who can repeatedly turn ambiguous emerging techniques into defensible org-level recommendations is the one who shapes the technical direction of the company.
