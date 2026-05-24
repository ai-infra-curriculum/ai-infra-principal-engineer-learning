# Project 02: Cross-Team Platform Integration

> **Duration:** 80 hours (3-4 weeks full-time, 8 weeks part-time)
> **Scope:** Multi-team — unifies 3+ ML platforms across the engineering org
> **Difficulty:** Principal / Expert
> **Track:** Individual Contributor (IC), Principal Engineer
> **Project Type:** Tier 4 — Platform integration, lasting developer experience

---

## Overview

Your company has accreted three (or more) ML platforms over five years. The training team built a thing on Kubeflow. The applied-research team built a thing on Ray on bare-metal. The product-ML team uses a vendor SaaS (SageMaker / Vertex / Databricks). Each platform has its own auth, its own job spec, its own UI, its own billing dashboard, and its own opinions about what counts as a "model". An engineer who moves between teams loses two weeks every time. A VP who wants a single number for "what did we spend on training last quarter?" gets a four-way email thread and a guess.

Your charter is **not** to replace any of these platforms. That fight is unwinnable and not what good principal engineers do. Your charter is to build the **integration layer** that makes the three (or more) platforms feel like one product to a developer, one cost center to finance, one identity domain to security, and one source of truth to leadership.

Concretely you will deliver: a **Backstage-based developer portal** with a unified ML surface, **golden-path templates** for the top 3 workflows (train, serve, evaluate), a **federated identity** model that bridges OIDC across the three platforms, a **cost-attribution pipeline** that joins per-platform cost telemetry into one cube, and an **adapter SDK** that lets future platforms plug in without rewriting the portal.

This is a project about **glue done with taste**. Bad glue creates a fourth thing for teams to learn. Good glue dissolves into the workflow and the underlying platforms become an implementation detail. Tell the difference and you'll do principal-grade work.

---

## Why This Project Matters

At principal level you are paid to make the **whole** larger than the sum of its parts. The single largest source of avoidable cost at most ML orgs is not bad models or wasted GPUs; it is **engineering time lost to context switching between platforms** and **decisions deferred because nobody can see the whole picture**. A platform-integration project, done well:

1. **Cuts onboarding** for a new ML engineer from weeks to days
2. **Unifies the cost story** so capacity planning and chargeback are arguable, not guessable
3. **Removes the "which platform do I use?" decision** from every project kickoff
4. **Lets the three underlying platforms evolve independently** without breaking users
5. **Earns trust from three platform owners**, none of whom want their platform deprecated

It is also a project where **org skill matters as much as code**. You will negotiate with three platform teams that each believe their platform is the future. You will write a charter that explicitly says "we are not picking a winner." You will have to mean it.

---

## Learning Outcomes

After completing this project, you will be able to:

### Technical depth
1. **Design and ship a Backstage portal** with custom ML plugins (training jobs, model registry view, endpoints, costs) wired to live data from 3+ backend platforms
2. **Implement a federated identity bridge** — OIDC token exchange (RFC 8693) between an org IdP (Okta / Entra / Google) and per-platform identities (AWS IAM, GCP SA, Databricks workspace, K8s SA)
3. **Build a cost-attribution pipeline** that ingests per-platform billing/usage data, normalizes to a common schema, joins with team/project metadata, and writes a queryable cube (BigQuery / Snowflake / ClickHouse)
4. **Author golden-path templates** (Backstage software templates) that scaffold a new training job, a new serving endpoint, and a new offline eval — each working on at least 2 backends
5. **Define an adapter SDK** with a stable contract such that a fourth platform can be added in ≤ 1 engineer-week

### Principal-level skill
6. **Charter a multi-team initiative** with three platform owners as stakeholders — RACI, scope guardrails, escalation paths
7. **Write a 12–20 page integration design doc** that survives review by all three platform leads plus security and FinOps
8. **Run a cross-team rollout** that migrates ≥ 5 real (or realistic) developer workflows to the new portal with measurable adoption
9. **Argue the cost-attribution case** at a level a CFO or VP-Eng would sign off
10. **Design with reversibility** — every integration point must have a documented exit ramp; no fork-lift dependencies on the portal

---

## Key Questions This Project Answers

You must be able to defend a clear answer to each by the end. They will appear in the rubric.

1. **Why integration, not consolidation?** What is the cost — political and technical — of telling the three platform teams to converge on one? Why is the glue strategy correct *now*, and what conditions would flip the answer to "yes, consolidate"?
2. **What is the contract?** What is the minimum API each platform must expose for your adapter to work? What do you do when a platform refuses?
3. **What is the identity model?** Who is the user, what is the principal, what tokens flow where, who can revoke what?
4. **How do you bill?** What does a per-project cost number actually include (compute, storage, network egress, idle reservations, support cost)? Where are the known gaps and how do you communicate them?
5. **What is the portal *not*?** What surfaces stay on the underlying platforms and why?
6. **What is the rollback?** If the portal is wrong, how does a team get back to working directly against its platform within 5 minutes?
7. **What is the long-term shape?** In 24 months, do the underlying platforms still exist, or has the integration layer made consolidation easier? Either answer is fine — pick one and own it.

---

## Prerequisites

### Required experience
- **8+ years** total engineering, **3+ years** at staff scope
- Hands-on with at least **two** of: Kubeflow / Ray / SageMaker / Vertex AI / Databricks / Azure ML / Anyscale
- Production experience with **OIDC**, OAuth 2.0 token exchange, or workload identity federation
- Familiarity with **Backstage** (or equivalent IDP — Port, Cortex, Roadie). At minimum installed it once.
- Built and operated at least one **data pipeline** (Airflow / Dagster / dbt) you would defend on-call
- Written at least one design doc with **3+ teams as co-stakeholders**

### Required completion in this curriculum
- Module 501 (Technical Strategy)
- Module 503 (Cross-Org Initiative) — **strongly recommended**; this project leans on it heavily
- Module 502 (Mentorship & Leadership)

### Infrastructure assumed available
Either real or credibly simulated:
- Cloud accounts (or mocks with realistic billing exports) for **at least 2** of {AWS, GCP, Azure}, plus access to **one** Kubernetes-based ML platform
- An OIDC provider you can configure (Okta dev, Google, Auth0, Dex)
- A data warehouse (BigQuery free tier, Snowflake trial, ClickHouse Cloud, or self-hosted)
- A Backstage instance (you can run locally; for the rubric you must show it deployed)
- Permission to register OIDC apps in your IdP

If using mocks (e.g., synthetic billing CSVs), document why and what would change with real data. Hand-waving is not acceptable at principal level.

---

## Deliverables (Summary — see `deliverables/README.md` for full submission spec)

1. **`design-doc.md`** (12–20 pages) — integration architecture, charter, scope guardrails
2. **ADRs `adr/0001` through `adr/0006`** — minimum 6 (portal choice, identity model, cost schema, adapter contract, template strategy, rollback model)
3. **Working Backstage portal** under `portal/` — plugins for jobs, models, endpoints, costs; deployed (even if to a personal cluster)
4. **Adapter SDK** under `src/platform_sdk/` — typed, documented, with at least 2 implemented adapters
5. **Cost-attribution pipeline** under `pipelines/cost/` — ingests, normalizes, loads; produces a queryable cube and a `cost by team / by week / by platform` view
6. **Federated identity flow** — runnable demo of an OIDC token exchanged across 2+ platforms; sequence diagrams in design doc
7. **3 golden-path templates** — training, serving, eval; each runs on ≥ 2 backends
8. **Adoption evidence** — 5 (real or realistic) workflows migrated, with before/after metrics
9. **Tech talk** — 25–40 min recorded talk + slides
10. **Operating model doc** — who owns what after launch, SLA, on-call seam between portal and underlying platforms

---

## Week-by-Week Duration (80 hours total)

| Week | Hours | Focus |
|------|-------|-------|
| 1 | 16 h | Charter, stakeholder map, design doc draft 1, adapter contract spike |
| 2 | 20 h | Backstage portal + first 2 adapters + identity federation |
| 3 | 22 h | Cost pipeline, golden-path templates, third adapter |
| 4 | 22 h | Rollout to 5 workflows, ADRs, operating model, tech talk |

Part-time at 10 h/week takes ~8 weeks; same phasing.

Day-by-day breakdown in [`STEP_BY_STEP.md`](./STEP_BY_STEP.md).

---

## Success Criteria

You have completed this project at a passing principal level when **all** of these are true:

### Technical
- Portal is **deployed and reachable**, showing **live data** (not screenshots) from at least 3 backend platforms
- Adapter SDK has at least **2 fully implemented adapters** and a stub for a 3rd; adding a 4th would take ≤ 1 engineer-week per a documented checklist
- Cost cube returns a "spend by team by week by platform" query in ≤ 5 seconds at the scale you've loaded
- Federated identity flow works end-to-end: a user logs into the portal once and the portal invokes operations on at least 2 backend platforms on their behalf using short-lived tokens

### Adoption
- **5 workflows** demonstrably migrated (or piloted) with before/after metrics: time-to-first-job, number of UIs the developer touches, number of credentials they manage
- At least **one platform team lead** has signed off on the adapter contract for their platform

### Communication
- Tech talk recorded; includes **one slide** on what you got wrong about the org dynamics, not just the tech
- Operating model is unambiguous — a reader can answer "who pages who when the portal returns wrong cost data?" in under 30 seconds

### Stretch criteria (for higher scores)
- A **fourth** platform plugged in by someone other than you, using only the adapter SDK docs
- Cost cube includes a **sensitivity analysis** view (e.g., "what would last month's bill have been at 90 % spot mix?")
- Portal contributes a plugin upstream to Backstage or a similar OSS project
- Adoption number includes a team that initially opposed the project

---

## Related Lessons

| Lesson | How it feeds this project |
|--------|---------------------------|
| **Module 501 — Technical Strategy** | Charter, 12-month roadmap, integration-vs-consolidation argument |
| **Module 502 — Mentorship & Leadership** | Coaching adopting teams; running platform-lead reviews |
| **Module 503 — Cross-Org Initiative** | Stakeholder mapping; RACI; managing three platform owners |
| **Module 504 — Open Source / Community** | Backstage upstream contribution; community plugin ecosystem |
| **Module 505 — Long-term Technical Bets** | Portal as integration layer vs eventual consolidation |

---

## Rubric Summary

See [`rubric.md`](./rubric.md) for the full grading rubric. High-level:

| Dimension | Weight | What "Exceeds" looks like |
|-----------|--------|---------------------------|
| Integration architecture & adapter SDK | 25 % | SDK is small, opinionated, and a 4th adapter is shipped by an external contributor |
| Developer experience (portal + golden paths) | 20 % | Onboarding-to-first-job dropped from days to under an hour, demonstrated |
| Federated identity & security model | 15 % | Token exchange documented end-to-end; zero long-lived per-platform credentials in developer hands |
| Cost attribution & FinOps | 15 % | CFO-defensible numbers; sensitivity analysis; gaps explicitly named |
| Cross-team adoption | 15 % | 5 workflows live; ≥ 1 initially-skeptical team converted |
| Communication & operating model | 10 % | Talk is publishable; operating model resolves the on-call seam question |

Minimum **70 / 100** to pass. **85+** is portfolio-grade.

---

## How to Use This Project

This is structured for **self-paced principal-track learners** treating the work as a real cross-org initiative, not a homework exercise.

1. Read this README, [`requirements.md`](./requirements.md), and [`architecture.md`](./architecture.md) end to end before writing any code or any doc.
2. **Write the charter first.** The single most common failure mode here is starting on Backstage plugins before the three platform teams agree on what "integration" means.
3. Build incrementally per [`STEP_BY_STEP.md`](./STEP_BY_STEP.md). Resist the urge to skip stakeholder discovery; that's where most of the principal-level signal is.
4. Have at least three reviewers on the final package: one staff engineer from a platform team, one security partner, one FinOps / finance partner.
5. Treat the tech talk as the final integration test of your story — if you can't explain the *charter* in three minutes to a skeptical platform lead, the rest of the work doesn't land.

Good luck. The orgs that get this right pull two years of compounded productivity out of work that already exists. That is the leverage of principal-grade integration work.
