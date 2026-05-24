# Rubric — Project 04: Technical Innovation POC

Total: **100 points**. Minimum to pass: **70**. Portfolio-grade for principal interviews: **85+**.

Scoring is on a 5-level scale per dimension. A dimension's contribution = (dimension level / 4) × dimension weight.

Levels:
- **0 — Missing or unacceptable**
- **1 — Below bar** (would not pass senior-engineer level)
- **2 — Meets senior-engineer bar** (works but doesn't reflect principal-level thinking)
- **3 — Meets principal bar** (defensible, complete, ready to ship the recommendation)
- **4 — Exceeds principal bar** (portfolio-grade; could appear on a promo packet or external talk)

---

## Dimension 1 — Technique Implementation & Faithfulness (20 pts)

Did you actually build a credible POC, and is it faithful to the technique?

| Level | Evidence |
|-------|----------|
| 0 | No working POC, or only a notebook. |
| 1 | A demo on a toy model; not reproducible. |
| 2 | Runs end-to-end but at non-credible scale; or diverges from the technique in undocumented ways. |
| 3 | POC runs end-to-end at credible scale (≥ 1B params for LLM techniques; realistic batch/request distribution for inference techniques). Faithful to the published technique, with clearly noted adaptations. Toggle flag enables clean baseline vs treatment comparison. |
| 4 | POC reproduces the published headline within paper's tolerance on your stack. Implementation is open-sourceable; a reviewer can clone and reproduce within 60 minutes. Adaptations to your hardware/model are explicit and justified. |

**Sample evidence accepted:** `src/poc/`, `make poc` works, README quickstart, headline reproduction note.

---

## Dimension 2 — Measurement Methodology & Evaluation (20 pts)

Is the methodology defensible? Is the evaluation reusable?

| Level | Evidence |
|-------|----------|
| 0 | No pre-registration; baselines hand-waved; no statistical analysis. |
| 1 | Single-run "before/after" with no CI; post-hoc success criteria. |
| 2 | Baselines measured; CIs reported on the headline; harness is a script, not reusable. |
| 3 | **Pre-registered** success criteria committed *before* implementation (commit timestamps verifiable). Baselines on your hardware + your workload with 95 % CI. Noise floor measured. Automated harness produces structured JSON + Markdown reports. Stats include paired comparison and significance. |
| 4 | Harness reusable for a different technique with ≤ 1 day of work. Methodology section publishable as a methodology blog. Confounders explicitly addressed (time, hardware variability, shared resources). |

**Sample evidence accepted:** `docs/success-criteria.md` (pre-registered with commit tag), `docs/methodology.md`, harness source, `make harness` works, JSON + Markdown reports.

---

## Dimension 3 — Stress Testing & Boundary Analysis (15 pts)

Did you find where the technique breaks?

| Level | Evidence |
|-------|----------|
| 0 | Only ran paper's regime. |
| 1 | One out-of-regime experiment; no failure mode analysis. |
| 2 | 1–2 stress conditions tested; no root cause analysis. |
| 3 | **≥ 3 stress conditions** tested, at least 2 outside the paper's optimal regime. Failure modes characterized (numeric divergence at long sequence, throughput collapse at saturation, accuracy drop in OOD, etc.). At least 2 negative experiments kept with hypothesis-of-why-not. |
| 4 | Boundary analysis includes a generalization map ("works at X to Y, breaks beyond Z"). Stress findings change the recommendation (the recommendation would have been different if stress weren't tested). |

**Sample evidence accepted:** stress experiments in `experiment-log.md`, `docs/boundary-analysis.md`, negative-results section in POC report.

---

## Dimension 4 — Recommendation Quality & Calibration (20 pts)

Could a sponsor act on this recommendation today?

| Level | Evidence |
|-------|----------|
| 0 | No recommendation, or "needs more work." |
| 1 | Recommendation present but ambiguous; no confidence; no triggers. |
| 2 | Recommendation present; confidence as "high/med/low"; no observable triggers. |
| 3 | Unambiguous `go` / `no-go` / `not-yet`. **Calibrated probability** stated (e.g., 0.7). For `not-yet`: explicit, observable trigger conditions. For `go`: 6-month plan with first decision point. For `no-go`: opportunity cost + when to re-open. Owner-after-this-project named. Confidence rationale documented. |
| 4 | Recommendation includes a "what would change my mind" section grounded in the data. If the recommendation is `go`, a productionization commitment from the next team is in hand (or credibly simulated). If `not-yet`, the triggers are monitorable in production telemetry. |

**Sample evidence accepted:** `docs/recommendation.md`, `docs/confidence.md`, opening section of POC report, sponsor sign-off if applicable.

---

## Dimension 5 — Productionization Gap Analysis (15 pts)

If `go` was the recommendation, do we know what it costs?

| Level | Evidence |
|-------|----------|
| 0 | No gap analysis. |
| 1 | High-level "needs hardening" list. |
| 2 | Gap list present; no estimates, no owners. |
| 3 | Each gap has: description, engineer-week estimate, owning team, priority, dependency on other gaps, risk if not closed. Dependency graph included. Categories cover at minimum: correctness, observability, rollback, on-call, compliance, capacity. |
| 4 | Gap analysis is integrated with org reality (named teams, named tools, named blockers). The next team has reviewed it. Critical-path is identified and named. The gap analysis is the basis for a roadmap commitment. |

**Sample evidence accepted:** `docs/productionization-gaps.md`, dependency graph in Mermaid, next-team sign-off, integration with org tooling.

---

## Dimension 6 — Communication & Hand-Off (10 pts)

Can the next person pick this up? Can the sponsor act on it?

| Level | Evidence |
|-------|----------|
| 0 | No report; no talk; no hand-off. |
| 1 | Report exists but buries the recommendation; no hand-off doc. |
| 2 | Report opens with recommendation; talk recorded; hand-off ambiguous. |
| 3 | POC report 8–14 pages opening with recommendation, structured per architecture spec. Tech talk 25–40 min recorded with story arc and a "what surprised us" slide. `docs/hand-off.md` names next team and first 30 days. `docs/where-to-pick-up.md` lands a cold reader in ≤ 30 min. |
| 4 | Talk is publishable internally (or accepted externally). Hand-off has been validated by a peer who simulated picking it up cold. POC report's recommendation paragraph can be quoted verbatim to a VP. |

**Sample evidence accepted:** `docs/poc-report.md`, `talks/tech-talk.mp4`, `talks/slides.pdf`, `docs/hand-off.md`, `docs/where-to-pick-up.md`, peer validation note.

---

## Scoring Worksheet

```
Dimension                                         Weight   Level (0–4)   Subtotal
Technique implementation & faithfulness            20         ___         ___ × 20/4 = ___
Measurement methodology & evaluation               20         ___         ___ × 20/4 = ___
Stress testing & boundary analysis                 15         ___         ___ × 15/4 = ___
Recommendation quality & calibration               20         ___         ___ × 20/4 = ___
Productionization gap analysis                     15         ___         ___ × 15/4 = ___
Communication & hand-off                           10         ___         ___ × 10/4 = ___
                                                                          ─────────────
                                                              TOTAL:      ___ / 100
```

---

## Calibration Notes for Reviewers

- A "Level 3" project is **publishable internally** at a real company; the sponsor could make a decision on it.
- A "Level 4" project should feel like an artifact the org's CTO or VP-Eng could share with peers at other companies as an example of how to evaluate emerging techniques.
- A POC that ends in `no-go` is **as valuable** as one that ends in `go`. Sometimes more so — avoided bets pay back over years.
- Pre-registration is the single most important signal. If the criteria were modified post-hoc, no other dimension can be `Level 4`.
- Calibrated confidence beats false certainty. A `0.65 not-yet` with named triggers is `Level 4`; a `"high-confidence go"` without triggers is `Level 2`.
- A common error: scoring `Level 4` because the POC reproduced the paper. Reproduction is `Level 3` if it's clean. `Level 4` requires the stress, the calibration, and the hand-off.
- A common error in the other direction: penalizing a `no-go` because "the technique didn't work." A well-evidenced `no-go` is excellent principal-level work.

---

## Self-Assessment Before Submission

Before handing in for review, score yourself. If your honest self-score is below 70 in any single dimension, fix that dimension before submitting. The two dimensions most often under-scored on the first pass are **Recommendation Calibration** (because "high confidence" feels safer than a number) and **Stress Testing** (because the time box runs out before the stress matrix gets exercised). Be honest with yourself first.
