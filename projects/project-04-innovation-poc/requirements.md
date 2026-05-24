# Requirements — Project 04: Technical Innovation POC

This document defines what the POC must do and must not do, the constraints, the assumptions, and the acceptance criteria a reviewer will check.

Requirements use **MoSCoW** prioritization: **M**ust, **S**hould, **C**ould, **W**on't.

---

## 1. Functional Requirements

### 1.1 Technique Selection & Charter (Must)

- **M-FR-1**: A **single emerging technique** is selected. "Emerging" means the bulk of literature is within the last 12–18 months. Candidates include but are not limited to FP4 training, MoE serving, EAGLE-2 speculative decoding, FP8 inference, disaggregated prefill/decode, Mamba / SSM hybrids, agentic ML pipelines, online RLHF, AWQ/GPTQ INT4.
- **M-FR-2**: A **charter** ≤ 1 page covers: technique name, hypothesis, why this technique now, the org-level question to be answered, the 80-hour time box, the named recommender (you).
- **M-FR-3**: A **technique-selection comparison** evaluates the chosen technique against ≥ 2 alternatives along: relevance to org, novelty, hardware feasibility, expected magnitude of win, productionization difficulty.
- **S-FR-4**: A **sponsor** is named — a real or hypothetical leader who will receive the recommendation and act on it.

### 1.2 Pre-Registered Success Criteria (Must)

- **M-FR-5**: A **go / no-go / not-yet criteria document** is committed *before* the first experiment runs. It defines:
  - **Go threshold** — concrete metric(s) and value(s) that, if met, support a "go" recommendation
  - **No-go threshold** — concrete metric(s) and value(s) that, if observed, support a "no-go" recommendation
  - **Not-yet conditions** — the gap between go and no-go, plus the trigger conditions that would convert "not-yet" to "go" later
- **M-FR-6**: The thresholds must be set with reference to a **baseline** that reflects your org's current state, not the paper's benchmark.
- **M-FR-7**: A **noise floor** is established for each measured metric before experiments begin.
- **S-FR-8**: A **decision matrix** is pre-defined for the case where some metrics meet "go" and others don't.

### 1.3 POC Implementation (Must)

- **M-FR-9**: A **runnable** end-to-end implementation of the technique. "Runnable" means: a reviewer clones the repo, follows the README, and reproduces the headline result within a documented time + hardware budget.
- **M-FR-10**: The implementation must be **honest** about what comes from upstream and what is your code. If you wrap an OSS implementation, say so; if you rewrote a kernel, show the diff.
- **M-FR-11**: The implementation must run at a **credible scale** — not a toy 100M-parameter model unless the technique is specifically about small models. For LLM techniques: ≥ 1B parameters; ideally 7B+. For inference techniques: realistic batch size and request distribution.
- **S-FR-12**: The implementation is **modular** — the technique can be enabled/disabled via a single flag, so baselines are comparable.

### 1.4 Eval Harness (Must)

- **M-FR-13**: An **automated eval harness** runs the technique against an agreed set of evals appropriate to the technique. Examples:
  - LLM accuracy techniques (quantization, distillation): MMLU, HellaSwag, HumanEval, a domain-specific eval
  - LLM serving techniques (speculative decoding, paged KV): TTFT, TBT, goodput, throughput at SLO
  - Training techniques (FP8, MoE): loss curve, eval-step metrics, convergence wall-clock
- **M-FR-14**: The harness produces a **structured report** (JSON + Markdown) that the POC report can ingest verbatim.
- **M-FR-15**: The harness reports **diff vs baseline**, not absolute numbers only.
- **S-FR-16**: The harness is **reusable** — another engineer can swap in a different model / technique with ≤ 1 day of work.

### 1.5 Experiment Campaign (Must)

- **M-FR-17**: **≥ 12 documented experiments**, each with:
  - hypothesis (one sentence)
  - intervention (code / config / data change)
  - measurement (before / after with CI where relevant)
  - decision (continue / pivot / kill)
- **M-FR-18**: At least **3 stress experiments** — conditions outside the paper's main regime, where the technique is expected to be challenged (long context, low SLO, large batch, distribution shift, hardware variation).
- **M-FR-19**: At least **2 negative experiments** documented — where the technique didn't deliver and the negative finding is kept, not deleted.

### 1.6 Productionization Gap Analysis (Must)

- **M-FR-20**: A **gap analysis** lists, per gap, the missing piece for production adoption. Categories at minimum:
  - Correctness checks (numerics, regression tests, eval)
  - Observability (metrics, traces, dashboards, alerts)
  - Rollback / kill switch
  - On-call surface (runbook, paging, escalation)
  - Compliance / security (model card, audit trail, data handling)
  - Capacity / cost (steady-state, peak, regional)
- **M-FR-21**: Each gap has: rough engineer-week estimate, owning team (real or hypothetical), priority, dependency on other gaps.
- **S-FR-22**: A **dependency graph** is drawn showing which gaps block which others.

### 1.7 Recommendation (Must)

- **M-FR-23**: A **recommendation** is stated unambiguously: `go`, `no-go`, or `not-yet`.
- **M-FR-24**: For **go**: a 6-month plan with milestones, owners, and the **first** decision point.
- **M-FR-25**: For **no-go**: the opportunity cost; the conditions under which you would re-open; pointers to alternative techniques.
- **M-FR-26**: For **not-yet**: explicit **trigger conditions** — what observable change would convert this to "go" (hardware release, paper improvement, dependency maturation, business priority shift).
- **M-FR-27**: Confidence is **stated and calibrated** ("70 % go") with the conditions that would change it.

### 1.8 Hand-Off (Must)

- **M-FR-28**: A **hand-off doc** names the team that owns the next step.
- **M-FR-29**: The first 30 days of that team's work is outlined.
- **M-FR-30**: The repo includes a `where-to-pick-up.md` guide for an engineer landing on the project cold.

### 1.9 Communication (Must)

- **M-FR-31**: A **POC report** (8–14 pages) opens with the recommendation, supports it with evidence, includes the risk register.
- **M-FR-32**: A **tech talk** (25–40 min, recorded) explains the technique, the campaign, the recommendation. Includes one "what surprised us" slide.
- **M-FR-33**: ≥ 5 ADRs covering: technique choice, measurement methodology, eval harness design, integration boundary, recommendation framing.

---

## 2. Non-Functional Requirements

### 2.1 Scientific Rigor (Must)

- **M-NFR-1**: Success criteria pre-registered (committed before experiments).
- **M-NFR-2**: Baselines on **your** hardware and **your** workload, not the paper's.
- **M-NFR-3**: Noise floor measured; minimum detectable effect documented.
- **M-NFR-4**: Each headline measurement has ≥ 95 % CI.
- **M-NFR-5**: Confounders addressed (hardware, time-of-day, workload distribution).

### 2.2 Reproducibility (Must)

- **M-NFR-6**: A reviewer can clone the repo and reproduce the headline result within 60 minutes (excluding model download time).
- **M-NFR-7**: Every experiment has a `repro/<exp-id>/` directory with the command, the config, the expected output range.
- **M-NFR-8**: Software versions pinned (PyTorch, CUDA, vLLM, etc.).

### 2.3 Honesty (Must)

- **M-NFR-9**: Negative experiments kept.
- **M-NFR-10**: Failed conditions documented, not hidden.
- **M-NFR-11**: Confidence in the recommendation is calibrated, not signaled (state probability or trigger conditions).
- **M-NFR-12**: Comparison vs alternatives is fair; the chosen technique is not strawmanned-against.

### 2.4 Maintainability (Should)

- **S-NFR-13**: Code is type-checked on the public POC API.
- **S-NFR-14**: No file > 800 LOC.
- **S-NFR-15**: Tests cover the eval harness and any numerics-sensitive paths.

---

## 3. Constraints

- **C-1**: 80-hour hard time box. Going over to "really nail it" is the failure mode this project teaches you to avoid. Document overruns in the hand-off doc.
- **C-2**: Pre-registered success criteria are not modified post-hoc. If they were wrong, that finding goes in the report as a methodology learning.
- **C-3**: Negative findings are kept. A POC that ends in "no-go" is as valuable as one that ends in "go" — sometimes more so.
- **C-4**: You may not say "go" without a productionization gap analysis. You may not say "no-go" without the opportunity-cost analysis.
- **C-5**: The recommendation is **yours** — not the paper authors', not the vendor's. Defend it on your evidence.
- **C-6**: The eval harness must be **automated**. A one-off notebook is not a harness.
- **C-7**: The technique must be implemented at credible scale. A 100M-parameter "we proved it works" is not credible for an LLM technique unless the technique is specifically about small models.

---

## 4. Assumptions

You may assume the following without further justification. If absent, document the substitute.

- **A-1**: Hardware appropriate to the technique is available (e.g., H100 for FP8 / FP4; A100 OK for most inference techniques).
- **A-2**: A reference model and reference dataset / request distribution are available.
- **A-3**: Tooling (profilers, eval harnesses, load generators, vLLM, TensorRT-LLM, NVIDIA TE) installable on your environment.
- **A-4**: At least one sponsor / reviewer who would act on the recommendation in a real org.
- **A-5**: Permission to install pre-release / nightly versions of frameworks where the technique requires.

---

## 5. Out of Scope (Won't)

To keep this project at 80 hours:

- **W-1**: You will not productionize the technique. You write the gap analysis; production is the next team's job.
- **W-2**: You will not write the paper. Reproduction quality is enough; novel research is out of scope.
- **W-3**: You will not benchmark every adjacent technique. One technique, one POC, one recommendation.
- **W-4**: You will not build new infrastructure (training framework, serving framework). Use existing.
- **W-5**: You will not solve the org's roadmap. You produce one input to the roadmap.
- **W-6**: You will not run multi-month experiments. If the technique fundamentally requires months of training to validate, justify the substitute (smaller scale, fewer steps, plot the learning curve to project).
- **W-7**: You will not skip the failed conditions. Boundary analysis is part of the deliverable, not a stretch.

---

## 6. Acceptance Criteria

A reviewer should be able to mechanically check these.

### A. Selection + charter
1. `docs/technique-selection.md` exists and compares ≥ 2 alternatives.
2. Charter ≤ 1 page in `docs/charter.md`.
3. Sponsor named (or hypothetical sponsor with role + organizational context).

### B. Pre-registration
4. `docs/success-criteria.md` committed before the first experiment (commit timestamps verifiable).
5. Go / no-go / not-yet thresholds are concrete (numbers + units, not adjectives).
6. Noise floor measured and reported.

### C. POC works
7. `make poc` (or equivalent) reproduces the headline result within a documented time + hardware budget.
8. The implementation runs at a credible scale per the technique.
9. The technique can be toggled via a flag to compare with baseline.

### D. Eval harness
10. `make eval` produces a structured report (JSON + Markdown).
11. Eval covers metrics appropriate to the technique.
12. Eval reports diff vs baseline.

### E. Experiments
13. ≥ 12 experiments in `experiments/experiment-log.md` with full structure.
14. ≥ 3 stress experiments outside the paper's main regime.
15. ≥ 2 negative experiments kept.

### F. Gap analysis
16. `docs/productionization-gaps.md` enumerates gaps with engineer-week estimates and owners.
17. Dependency graph (Mermaid or PNG) included.

### G. Recommendation
18. Recommendation in `docs/recommendation.md` is unambiguous (`go` / `no-go` / `not-yet`).
19. Confidence calibrated (probability or trigger conditions).
20. For `not-yet`, trigger conditions are observable and measurable.

### H. Hand-off
21. `docs/hand-off.md` names next team and first 30 days.
22. `docs/where-to-pick-up.md` lands a cold reader in ≤ 30 minutes.

### I. Communication
23. POC report 8–14 pages; opens with recommendation.
24. Tech talk 25–40 min recorded; slides committed.
25. ≥ 5 ADRs merged with full structure.

### J. Quality bar
26. No file > 800 LOC.
27. Type checks clean on POC public API.
28. Tests cover the eval harness.

---

## 7. Dependencies on Other Teams (For the Plan, Not the Code)

For the design doc and recommendation, explicitly identify dependencies on:

- **A sponsor** — the leader who will act on the recommendation
- **A productionization team** — the team that would own the next phase if `go`
- **Model owners** — for the reference model and the correctness contract
- **Infra / Platform team** — for hardware allocation if `go`
- **Security / Compliance** — for any new dependency, runtime, or data-handling implication
- **FinOps / Finance** — for steady-state cost projections in the gap analysis

Naming real people is not required; naming roles, decision rights, and dependencies is.

---

## 8. Glossary

- **POC** — Proof of Concept
- **De-risk** — convert ambiguity about a technique into a defensible go/no-go recommendation
- **Pre-registration** — committing analysis plan + success criteria before data collection
- **Trigger condition** — an observable event that would change a `not-yet` to a `go`
- **Calibrated confidence** — a probability estimate that, over many such estimates, matches the empirical frequency
- **Noise floor** — the minimum detectable difference given measurement variability
- **Gap analysis** — the inventory of missing capabilities between POC and production
- **Hand-off** — formal transfer of project ownership; not implicit
- **Stress test** — running the technique outside the paper's optimized regime
