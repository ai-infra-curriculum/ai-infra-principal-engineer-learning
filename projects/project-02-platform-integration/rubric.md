# Rubric — Project 02: Cross-Team Platform Integration

Total: **100 points**. Minimum to pass: **70**. Portfolio-grade for principal interviews: **85+**.

Scoring is on a 5-level scale per dimension. A dimension's contribution = (dimension level / 4) × dimension weight.

Levels:
- **0 — Missing or unacceptable**
- **1 — Below bar** (would not pass senior-engineer level)
- **2 — Meets senior-engineer bar** (works but doesn't reflect principal-level thinking)
- **3 — Meets principal bar** (defensible, complete, ready to ship)
- **4 — Exceeds principal bar** (portfolio-grade; could appear on a promo packet or external talk)

---

## Dimension 1 — Integration Architecture & Adapter SDK (25 pts)

Is the integration done with taste? Is the adapter contract small, opinionated, and stable?

| Level | Evidence |
|-------|----------|
| 0 | No adapter SDK or only one backend integrated. |
| 1 | One adapter; no real contract; portal hard-codes backend specifics. |
| 2 | Adapter contract exists but is leaky (platform-specific fields surface in the portal). 2 adapters partially implemented. |
| 3 | Typed adapter contract, 2 full adapters + 1 stub. Conformance test suite covers ≥ 30 cases. SDK README contains a "build a 4th adapter in a week" checklist. ADR `0001-adapter-contract` defends the contract scope and the read/write boundary. |
| 4 | A 4th adapter built **by someone other than the project author** using only the SDK docs, in ≤ 1 week, with no contract changes needed. Contract has an explicit versioning policy with deprecation cadence. |

**Sample evidence accepted:** `src/platform_sdk/`, conformance test report, 4th-adapter retro if applicable, ADR `0001-adapter-contract.md`.

---

## Dimension 2 — Developer Experience (Portal + Golden Paths) (20 pts)

Did the portal + templates make a real difference in how developers work?

| Level | Evidence |
|-------|----------|
| 0 | No portal, or portal is a static dashboard. |
| 1 | Portal renders but only with seeded data; templates are README files, not scaffolds. |
| 2 | Portal renders live data from 1–2 backends. 1–2 templates exist; each runs on one backend. |
| 3 | Portal renders live data from **3+ backends**. **3 templates** run on **≥ 2 backends** each. A new developer can go from "logged in" to "first job running" in ≤ 30 min following docs. |
| 4 | Onboarding-to-first-job demonstrably dropped (numbers from the adoption pilot, not estimates). Templates self-deprecate by writing a runbook the team must fill in before first run. At least one template re-platforms (chosen at scaffold time, switch later by editing one file). |

**Sample evidence accepted:** deployed portal URL or screencast, `docs/adoption-pilot.md`, `docs/getting-started.md`, template source.

---

## Dimension 3 — Federated Identity & Security (15 pts)

Are tokens short-lived, scoped, audited, and free of long-lived per-user secrets?

| Level | Evidence |
|-------|----------|
| 0 | Long-lived per-user credentials; no federation. |
| 1 | A shared service-principal does everything; user identity is lost between portal and backend. |
| 2 | OIDC login to portal; per-backend SP credentials in the broker; no real per-call token exchange. |
| 3 | Token exchange end-to-end for ≥ 2 backends (e.g., AWS STS `AssumeRoleWithWebIdentity` and GCP WIF). Credentials ≤ 1 h TTL. Role mapping is data (Rego / YAML), not code. Audit log captures `actor`, `target_platform`, `target_role`, `request_id`. Documented fallback for SP-only platforms with auditing at app layer. |
| 4 | Token exchange end-to-end for 3+ backends, including at least one non-cloud SaaS. Audit log meets a named compliance control (SOC 2 CC6.x or equivalent). Role decisions auditable independent of adapter code. Failure-mode analysis includes broker outage. |

**Sample evidence accepted:** `src/identity_broker/`, role policy in `policies/`, audit log query examples, identity sequence diagram in design doc.

---

## Dimension 4 — Cost Attribution & FinOps (15 pts)

Could this argument survive a CFO?

| Level | Evidence |
|-------|----------|
| 0 | No cost pipeline. |
| 1 | Hand-rolled spreadsheet; no normalization. |
| 2 | Cost cube exists for 1–2 platforms; team attribution missing or hard-coded. |
| 3 | Cost cube ingests from **3+ platforms**, normalized to one schema, joined with team catalog. `cost_coverage.md` names every platform's attribution rate and known gaps. `spend_by_team_by_week_by_platform` query returns ≤ 5 s. Executive summary ≤ 1 page that a non-engineer can read. |
| 4 | Sensitivity analysis ("what if spot mix went to 90 %"). Forecast view projects month-end. Coverage gaps are tracked over time and a closure plan exists. Cost cube is operated by a real on-call (named in operating model). |

**Sample evidence accepted:** `pipelines/cost/`, dbt models, warehouse query screenshot, `docs/cost-coverage.md`, executive summary.

---

## Dimension 5 — Cross-Team Adoption (15 pts)

Did the project create *organizational convergence* on a developer experience?

| Level | Evidence |
|-------|----------|
| 0 | No adoption, no pilot. |
| 1 | The author is the only user. |
| 2 | 1–2 workflows migrated; no before/after metrics. |
| 3 | **5 workflows** migrated, each with documented before/after metrics (time-to-first-job, UIs touched, credentials managed). At least one platform team lead has signed off on the adapter contract for their platform. |
| 4 | One initially-skeptical team converted, with a written retro from them. Adoption telemetry (template instantiations / portal DAU) is on a dashboard the team reviews weekly. |

**Sample evidence accepted:** `docs/adoption-pilot.md`, sign-off comments from platform leads, adoption dashboard in `monitoring/`, retro from converted team.

---

## Dimension 6 — Communication & Operating Model (10 pts)

Can you explain the project, and is it operable after launch?

| Level | Evidence |
|-------|----------|
| 0 | No talk, no operating model, no docs beyond a stub README. |
| 1 | Talk exists but reads off slides; operating model is hand-wavy. |
| 2 | Talk 20–40 min, recorded. Operating model exists but punts on the on-call seam. |
| 3 | Talk 25–40 min, structured (problem / build / two deep dives / adoption / lessons / Q&A). Operating model resolves on-call seams for ≥ 7 scenarios. Executive summary ≤ 1 page that a non-engineer can read. README + getting-started + troubleshooting docs in place. |
| 4 | Talk is publishable internally (or externally accepted). Includes a "what I'd do differently — on org dynamics, not only tech" slide grounded in observed events. Operating model includes named owners and SLAs. |

**Sample evidence accepted:** `talks/tech-talk.mp4`, `talks/slides.pdf`, `docs/operating-model.md`, exec summary, README, getting-started, troubleshooting.

---

## Scoring Worksheet

```
Dimension                                         Weight   Level (0–4)   Subtotal
Integration architecture & adapter SDK             25         ___         ___ × 25/4 = ___
Developer experience (portal + golden paths)       20         ___         ___ × 20/4 = ___
Federated identity & security                      15         ___         ___ × 15/4 = ___
Cost attribution & FinOps                          15         ___         ___ × 15/4 = ___
Cross-team adoption                                15         ___         ___ × 15/4 = ___
Communication & operating model                    10         ___         ___ × 10/4 = ___
                                                                          ─────────────
                                                              TOTAL:      ___ / 100
```

---

## Calibration Notes for Reviewers

- A "Level 3" project is **publishable internally** at a real company. It should not feel like a learning exercise.
- A "Level 4" project should feel like the artifact a principal engineer would link from their promo packet **and** that the three platform leads on the integrated backends would endorse.
- Integration projects fail more often on **org dynamics** than tech. Adoption + operating model are weighted higher than usual for that reason.
- A common error: scoring "Level 4" because the *portal looks pretty*. A pretty portal with leaky abstractions and no coverage doc is Level 2.
- A common error in the other direction: penalizing absence of polish on the UI when the substance (contract, identity, cost) is principal-grade. Polish matters under Dimension 6; don't double-count under Dimension 2.
- Cost-coverage honesty **adds** points. A pipeline that admits "we attribute 78 % of GCP spend, here's why" is more credible than one that claims 100 % silently.

---

## Self-Assessment Before Submission

Before handing in for review, score yourself. If your honest self-score is below 70 in any single dimension, fix that dimension before submitting — don't let a reviewer be the one who tells you. The two dimensions most often under-scored on the first pass are **Federated Identity** (because token exchange is fiddly) and **Adoption** (because pilots get punted to "next month"). Be honest with yourself first.
