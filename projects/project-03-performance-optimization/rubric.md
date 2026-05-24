# Rubric — Project 03: Performance Optimization Initiative

Total: **100 points**. Minimum to pass: **70**. Portfolio-grade for principal interviews: **85+**.

Scoring is on a 5-level scale per dimension. A dimension's contribution = (dimension level / 4) × dimension weight.

Levels:
- **0 — Missing or unacceptable**
- **1 — Below bar** (would not pass senior-engineer level)
- **2 — Meets senior-engineer bar** (works but doesn't reflect principal-level thinking)
- **3 — Meets principal bar** (defensible, complete, ready to ship)
- **4 — Exceeds principal bar** (portfolio-grade; could appear on a promo packet or external talk)

---

## Dimension 1 — Headline Result & Methodology (25 pts)

Did the metric move, and is the measurement defensible?

| Level | Evidence |
|-------|----------|
| 0 | No headline number; no defined metric. |
| 1 | A claim of "faster" with no methodology; single-run "before/after"; no CI. |
| 2 | Headline metric improved with mean numbers; no CI; methodology informal. |
| 3 | Headline metric improved ≥ 1.5× (or ≥ 30 % reduction). Methodology pre-registered; baseline + treatment with 95 % CIs; statistical significance documented (p < 0.01); noise floor measured; workload pinned. |
| 4 | Improvement ≥ 2× (or ≥ 40 % reduction) with publishable-quality methodology. Multiple metrics moved (or guarded). Methodology section could be reviewed by a peer at NVIDIA or Meta without revision. Profiles support every claim. |

**Sample evidence accepted:** `docs/methodology.md`, `docs/workload.md`, baseline + final benchmarks, statistical analysis, profile screenshots, headline in README.

---

## Dimension 2 — Profiling Depth & Cross-Layer Reach (20 pts)

Did the campaign reach across kernel, framework, system, and policy layers?

| Level | Evidence |
|-------|----------|
| 0 | No profiling beyond a few timer prints. |
| 1 | One profiler used (PyTorch profiler or nsys); single-layer analysis. |
| 2 | Two profilers; wins in two layers (e.g., kernel + framework only). |
| 3 | At least one measurable win in **each** of {kernel, framework, system, policy}. ≥ 3 profiles committed with annotated screenshots. At least one cross-layer correlation written up (a policy symptom traced through layers to a kernel root cause). |
| 4 | All four layers contribute meaningfully (each ≥ 10 % of the headline). At least one custom kernel (Triton or CUDA) authored or substantively modified. Cross-layer narrative is the spine of the tech talk. |

**Sample evidence accepted:** `profiles/`, annotated screenshots, layer attribution in `experiment-log.md`, kernel code under `kernels/`, narrative section of design doc.

---

## Dimension 3 — Regression Prevention & Durability (15 pts)

Does the win stay won?

| Level | Evidence |
|-------|----------|
| 0 | No CI; no durability discussion. |
| 1 | Manual benchmark run occasionally; no automation. |
| 2 | CI exists; uses a fixed % threshold; no durability analysis. |
| 3 | Perf CI runs nightly with statistical regression detection. Synthetic ≥ 5 % regression caught in one CI run (proven via PR). `docs/durability.md` per-optimization survival analysis exists, with named follow-up owners for non-durable wins. |
| 4 | CI tracks long-term drift charts per metric; second-workload validation documented for ≥ 3 optimizations; durability section names specific upcoming triggers (model release, vLLM version, hardware refresh) and the planned re-validation cadence. |

**Sample evidence accepted:** `.github/workflows/perf-ci.yml` or equivalent, regression PR demo, `docs/durability.md`, second-workload report.

---

## Dimension 4 — Optimization Campaign Rigor (15 pts)

Was the campaign structured, honest, and complete?

| Level | Evidence |
|-------|----------|
| 0 | Fewer than 5 experiments documented. |
| 1 | 5–10 experiments; no statistical analysis; negatives discarded. |
| 2 | 10–15 experiments; some negatives kept; attribution informal. |
| 3 | **≥ 20 experiments**, each with hypothesis, change, before/after with CI, attribution, decision. **≥ 5 negative** experiments kept. Attribution sums sensibly to the headline. |
| 4 | ≥ 30 experiments; analysis includes interaction effects (which optimizations compound, which conflict); the experiment plan was updated based on profiles, not just executed blindly; experiments pre-registered (commit before run). |

**Sample evidence accepted:** `experiments/experiment-log.md`, `experiments/plan.md`, per-experiment `repro/` directories.

---

## Dimension 5 — Executive Narrative (15 pts)

Does the story land for a VP, a model team, and a FinOps partner?

| Level | Evidence |
|-------|----------|
| 0 | No exec summary, no talk. |
| 1 | A README with the number; no narrative. |
| 2 | Talk exists but is a feature tour ("here's all the things we tried"); exec summary too technical. |
| 3 | 1-page exec summary readable in 90 seconds by a non-engineer; 30-min talk with story arc (problem → measurement → top 3 wins → durability → next); one failed-experiment slide; slides + recording in repo. |
| 4 | Talk publishable internally (or accepted externally — MLSys / GTC / company all-hands). Exec summary translates the technical win into business outcome with credible $$ or user-impact framing. Multiple audiences in mind — there's a paragraph for the VP, a paragraph for the model team, a paragraph for FinOps. |

**Sample evidence accepted:** `docs/exec-summary.md`, `talks/tech-talk.mp4`, `talks/slides.pdf`, ADRs that name the business framing.

---

## Dimension 6 — Rollback & Operational Safety (10 pts)

Could this ship to production safely?

| Level | Evidence |
|-------|----------|
| 0 | No rollback; no canary; no reliability-SLO check. |
| 1 | Optimizations enabled directly via code change; no flags. |
| 2 | Some changes flagged; no canary plan; reliability SLO not measured. |
| 3 | Every production-touching change is feature-flagged with documented rollback. Canary plan for headline change with breaking-metric gates per stage. Reliability SLO maintained; correctness diff within named tolerance. |
| 4 | At least one optimization actually rolled out via canary (in this exercise or real). Postmortem-rule defined for any rollback. Reliability SLO + perf SLO both reported on the same dashboard. |

**Sample evidence accepted:** rollback runbooks in `docs/rollbacks/`, canary plan in `docs/canary.md`, feature flag config in `config/`, dashboard JSON in `monitoring/grafana/`.

---

## Scoring Worksheet

```
Dimension                                         Weight   Level (0–4)   Subtotal
Headline result & methodology                      25         ___         ___ × 25/4 = ___
Profiling depth & cross-layer reach                20         ___         ___ × 20/4 = ___
Regression prevention & durability                 15         ___         ___ × 15/4 = ___
Optimization campaign rigor                        15         ___         ___ × 15/4 = ___
Executive narrative                                15         ___         ___ × 15/4 = ___
Rollback & operational safety                      10         ___         ___ × 10/4 = ___
                                                                          ─────────────
                                                              TOTAL:      ___ / 100
```

---

## Calibration Notes for Reviewers

- A "Level 3" project is **publishable internally** at a real company. It should not feel like a lab exercise.
- A "Level 4" project should feel like the artifact a principal engineer would link from their promo packet **and** that the model team would point to when their next model launches.
- Performance projects most often fail on **methodology**, not on optimization choice. Score Dimension 1 strictly: a great win with bad methodology is Level 2, not Level 4.
- A common error: scoring "Level 4" because the headline number is big. If the methodology won't survive a peer at NVIDIA, the headline doesn't matter.
- A common error in the other direction: penalizing a "smaller" win (e.g., 1.6×) when the methodology is rigorous and the campaign is honest. Methodology + honesty often beats raw numbers at the principal level.
- Negative experiments **add** points, not subtract. A project with 20 positives and 0 negatives is suspicious.
- A perf project that broke reliability is **not** Level 3, regardless of the perf number.

---

## Self-Assessment Before Submission

Before handing in for review, score yourself. If your honest self-score is below 70 in any single dimension, fix that dimension before submitting — don't let a reviewer be the one who tells you. The two dimensions most often under-scored on the first pass are **Methodology** (because pre-registration feels overkill until challenged) and **Durability** (because "it works today" is the easy slide and the hard one). Be honest with yourself first.
