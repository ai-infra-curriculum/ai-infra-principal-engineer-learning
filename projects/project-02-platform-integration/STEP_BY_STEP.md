# Step-by-Step Build Guide — Project 02: Cross-Team Platform Integration

This is an **80-hour, 4-week build plan** at full-time pace, or ~8 weeks at 10 h/week. Phases are sequential — do not parallelize until you've hit the gate at the end of the prior phase.

Each phase has: **goal**, **day-level breakdown**, **validation gate**, and **gotchas** drawn from real production scars on platform-integration work.

---

## Phase 0 — Pre-Work (3 h, before Week 1)

### Goal
Confirm the environment, validate the three target backends, and reread the org problem you are solving.

### Tasks
1. Pick the **three target backends** and confirm hands-on access (real or simulated). At least one Kubernetes-based, at least one cloud SaaS. Suggested defaults: Kubeflow on EKS + SageMaker + Vertex; or Ray on K8s + Databricks + SageMaker.
2. Stand up a local Backstage instance (`npx @backstage/create-app`); confirm it boots, you can log in, and the default catalog renders.
3. Register OIDC dev apps in your IdP for: portal, identity broker, each backend that supports OIDC federation. Capture client IDs in `secrets/` (use a `.env.example` template; never commit real secrets).
4. Pull a 30-day sample of cost data per backend. Eyeball it. You will be normalizing this in week 3 — knowing what it looks like now saves a week of surprises later.
5. Pre-read: Backstage plugin architecture; RFC 8693; AWS `AssumeRoleWithWebIdentity` docs; GCP Workload Identity Federation; the cost-export schema for each cloud you'll touch.

### Gate
You can answer, in one paragraph each: "What does a job in backend A look like over the API? What does a cost record from backend A look like? What is backend A's identity federation story?"

### Gotchas
- Backstage moves fast; pin a version (`@backstage/cli@1.x`) and document it.
- `npx create-app` produces a *lot* of code. Resist the urge to rewrite it on day 1. Live with the defaults until you can defend a change.
- "I have access" usually means "I have access to the console." Confirm **programmatic** access via the SDK / API.

---

## Week 1 — Charter, Stakeholder Map, Design Doc Draft 1, Adapter Contract Spike (16 h)

The single most common failure mode on integration projects is **building before agreeing**. Don't.

### Day 1 (4 h) — Stakeholder map + charter

Interview (or, for personal projects, write user stories for) each of the **three platform owners** plus FinOps and Security. For each platform owner capture:
- Their current users (which teams)
- Their north star (where they want their platform in 24 months)
- Their nightmare ("if you do X we'll veto")
- Their on-call surface today

Charter is **one page** and answers:
- What this project is (integration layer)
- What it is **not** (replacement; consolidation; control plane)
- Who is in / out of scope
- Decision rights (who decides adapter contract changes; who decides portal UX)
- Escalation path

Output: `docs/charter.md`, `docs/stakeholder-interviews.md`.

### Day 2 (4 h) — Design doc skeleton

Use a real design-doc template. Sections at minimum:
1. Problem statement (org pain today, in numbers if you have any)
2. Goals + non-goals
3. Integration architecture (high level, with diagram)
4. Adapter contract sketch
5. Identity model
6. Cost-attribution approach
7. Golden-path templates
8. Operating model + rollout plan
9. Open questions
10. Appendix (charter, stakeholder notes)

Write **problem statement + goals first**. If you can't write one paragraph that the three platform leads would all sign, you don't yet know what you're building.

### Day 3 (4 h) — Decision menu

For each, write a 2–4 sentence position; each becomes an ADR stub:
- Portal framework (Backstage / Port / Cortex / custom)
- Adapter contract style (typed Python Protocol / gRPC / OpenAPI)
- Identity model (broker / per-adapter / SP-only fallback policy)
- Cost cube store (BigQuery / Snowflake / ClickHouse / Postgres)
- Template format (Backstage software templates / cookiecutter / both)
- Rollback model (always-direct deep-links / portal-as-source-of-truth)

Write the stubs into `adr/000N-*.md`. Status `proposed`.

### Day 4 (2 h) — Adapter contract spike

Write the contract as **typed Python** (`Protocol` + `Pydantic` models). One source file. No implementation yet. ≤ 200 LOC.

Get one platform owner to read it and tell you what's missing or wrong. Iterate before you implement.

### Day 5 (2 h) — Peer review of design doc draft 1

Get the doc in front of at least one experienced engineer outside the platform teams. Use their feedback to refactor before any UI code is written.

### Validation gate
- [ ] `docs/charter.md` (≤ 1 page) exists
- [ ] `docs/stakeholder-interviews.md` with ≥ 3 platforms + security + FinOps
- [ ] Design doc draft 1 (≥ 8 pages) committed
- [ ] 6 ADR stubs
- [ ] Adapter contract spike committed and reviewed by 1 platform owner
- [ ] At least one external reviewer has commented on the design doc

### Gotchas
- **Don't pick a winner among platforms.** The moment your charter implies one platform is "the future", you've lost the other two.
- The temptation to write your own portal because Backstage is "too heavy" is real. Resist for at least 2 weeks; you'll thank yourself.
- Adapter contracts grow forever if you let them. Stay minimal: read-heavy, almost no writes.

---

## Week 2 — Backstage Portal + First 2 Adapters + Identity Federation (20 h)

### Goal
The portal renders live data from two backends, with identity federated through OIDC.

### Day 6 (4 h) — Portal scaffolding + auth

- Clone the Backstage scaffold from Phase 0.
- Configure the OIDC auth provider against your IdP. Verify login + group claims.
- Set up the catalog with a Team kind sourced from your group catalog (SCIM, CSV, or hand-written for the spike).
- Add a placeholder for the four custom views (Jobs / Models / Endpoints / Costs).

### Day 7 (4 h) — Identity broker MVP

A small FastAPI (or Backstage backend plugin) service:
- Validates the user's ID token on every call
- Exposes `POST /exchange` with body `{target_platform, requested_role}`; returns short-lived creds
- Loads role mapping from a YAML or Rego file
- Logs every mint to a `audit_events` table (Postgres for now; ClickHouse later)

Implement two exchanges:
- AWS `AssumeRoleWithWebIdentity`
- Kubernetes TokenRequest API (or, if your K8s-ML platform is on a cloud, the cloud's WIF flavor)

Wire it behind a feature flag; default-deny until verified.

### Day 8 (5 h) — First adapter (Kubeflow / Ray / pick your K8s-ML)

- Implement `list_jobs`, `get_job`, `cancel_job`, `list_models`, `list_endpoints`, `cost_records`, `deep_link` against your chosen K8s-ML platform.
- Use the identity broker for every call.
- Write ≥ 10 conformance tests now; you'll generalize them in week 3.

### Day 9 (4 h) — Second adapter (SageMaker / Vertex / Databricks — pick one cloud SaaS)

- Same contract. Different platform.
- Note differences in the contract behavior (e.g., SageMaker job pagination quirks); capture in adapter docstrings.
- Update the identity broker if needed for this platform's federation flavor.

### Day 10 (3 h) — Portal views wired to adapters

- Jobs view: fan-out to both adapters in parallel; merge; render with platform badge.
- Models view: same.
- Endpoints view: same.
- Costs view: still placeholder (real data lands in Week 3).
- Confirm p95 page load ≤ 2 s on the test data set (or document the deviation).

### Validation gate
- [ ] OIDC login works end-to-end against your IdP
- [ ] Identity broker mints short-lived creds for at least 2 backends, logged to audit table
- [ ] Two adapters implemented; conformance tests passing
- [ ] Jobs / Models / Endpoints views show live data from both backends
- [ ] No long-lived per-user secrets exist anywhere

### Gotchas
- Backstage's auth plugins are easier to misconfigure than the docs suggest. Verify `email_verified`, group claims, and session expiry explicitly.
- Per-platform federation has a quirk per platform. Budget at least a half-day per new federation flavor.
- "Fan-out and merge" looks easy until the first adapter is slow. Set per-adapter timeouts (3 s) and degrade with a stale badge.

---

## Week 3 — Cost Pipeline, Golden-Path Templates, Third Adapter (22 h)

### Goal
The cost cube returns honest, queryable data; three golden-path templates ship; a third adapter is stubbed with a clear path to completion.

### Day 11 (5 h) — Cost ingesters

For each platform you're integrating:
- Implement an ingester that reads cost / usage from the platform's export (CUR, GCP billing, Azure CM, Databricks usage, OpenCost).
- Land raw rows in a `staging_<platform>_raw` table in the warehouse.
- Schedule via Dagster or Airflow.
- Idempotency: re-running a window does not duplicate.

### Day 12 (5 h) — Normalizer (dbt) + cube

- dbt models that transform `staging_*_raw` → `fact_cost_records` per the normalized schema in `architecture.md` §2.4.
- Join with the team catalog and the price book at the TAG step.
- Materialize `agg_team_week_platform` for the Costs view.
- Validate freshness: latest record per platform within 24 h of source export.
- Write the **coverage doc** `docs/cost-coverage.md`: per platform, what is captured, what is missing, what % of source spend is attributed.

### Day 13 (3 h) — Costs view wired to cube

- Backstage plugin that hits the warehouse (or a thin BFF on top).
- Spend by team by week by platform, with a freshness indicator and a coverage indicator.
- Drill-down per team into per-resource records.
- Performance: p95 query ≤ 5 s at loaded scale.

### Day 14 (5 h) — Golden-path templates

- `train-model`: scaffolds a Git repo with a job spec for the chosen backend, CI to lint + dry-run the spec, model card stub, registry entry stub.
- `serve-model`: scaffolds an endpoint definition, autoscaling policy, alert.
- `evaluate-offline`: scaffolds an eval job + a notebook.
- Each template asks for the backend at scaffold time and writes that into a single re-platformable file.
- Telemetry: log every instantiation (template, backend, user, result) to the audit table.

### Day 15 (4 h) — Third adapter (stub)

- Pick the third platform (the one you haven't done yet).
- Implement to the contract, but only fully implement `list_jobs`, `list_models`, `deep_link`, `cost_records`.
- The rest: typed stubs with `NotImplementedError` and a TODO each ≤ 1 day of work.
- Run the conformance suite; expected fails are listed in the stub adapter's README.

### Validation gate
- [ ] Cost cube returns `spend_by_team_by_week_by_platform` in ≤ 5 s
- [ ] `docs/cost-coverage.md` exists with per-platform coverage % and gaps
- [ ] Costs view renders live data with freshness + coverage indicators
- [ ] 3 templates instantiate end-to-end on at least 2 backends each
- [ ] Third adapter stub committed with documented TODOs
- [ ] Telemetry on template instantiations flows to the audit table

### Gotchas
- Cost data is messier than any docs admit. Plan for at least one "tag schema is inconsistent across accounts" surprise.
- dbt freshness tests are your friend; turn them on early.
- Templates that "work on my laptop" usually break on a fresh user. Test by handing the template to a real human.

---

## Week 4 — Rollout, ADRs, Operating Model, Tech Talk (22 h)

### Goal
Five workflows migrated, the load-bearing decisions written up, the operating model unambiguous, and the talk recorded.

### Day 16 (4 h) — Pilot 5 workflows

Pick 5 real (or realistic) workflows across the org. For each:
- Document the **before** state: which UIs, which credentials, how many minutes to first job
- Migrate (or write the migration playbook) to use the portal + a golden-path template
- Document the **after** state with the same metrics

Output: `docs/adoption-pilot.md`. Numbers, not adjectives.

### Day 17 (4 h) — ADRs polish

Polish the 6 ADRs from Week 1. Each must contain:
- Context (what problem; what alternatives we had)
- Decision (one sentence; what we chose)
- Alternatives considered (≥ 2 named with why-not)
- Consequences accepted (operational, organizational, technical debt)
- Status + date + deciders

Minimum 6: `adapter-contract`, `portal-choice`, `identity-model`, `cost-schema`, `template-strategy`, `rollback-model`. Add more if you've made more load-bearing decisions.

### Day 18 (4 h) — Operating model

`docs/operating-model.md` answers, for at least these scenarios:
1. Portal returns wrong cost data
2. Adapter throwing 5xx for one backend
3. Identity broker down
4. Backend platform deprecating an API field the adapter uses
5. New adapter contract version proposed
6. New team wants to onboard
7. Old platform deprecated; adapter needs sunset

For each: who owns, who pages whom, SLA, rollback.

### Day 19 (5 h) — Adoption mechanics + polish

- README at the top level that a new user lands on
- `docs/getting-started.md`: a 30-minute path from "logged into portal" to "first job submitted via template"
- `docs/troubleshooting.md`: top 10 issues with cause + fix
- A short loom-style walkthrough video for new users (separate from the tech talk)

### Day 20 (5 h) — Tech talk + executive summary

25–40 min recorded. Outline:
1. The problem (3 min) — three platforms, one developer's day
2. What we built (5 min) — portal, SDK, broker, cost cube, templates
3. Two deep dives (12 min) — pick **two**, not all five (suggested: identity model + cost coverage)
4. Adoption results (4 min) — the 5 workflows, before/after numbers
5. What we got wrong (3 min) — at least one org-dynamics lesson, not only tech
6. Roadmap (2 min)
7. Q&A

Record. Watch. Re-record once. Then write a **1-page executive summary** (no jargon) that an exec could read in 90 seconds.

### Validation gate
- [ ] 5 workflows pilot documented with before/after numbers
- [ ] 6 ADRs merged with full structure
- [ ] `docs/operating-model.md` resolves on-call seams for ≥ 7 scenarios
- [ ] README + getting-started + troubleshooting docs in place
- [ ] Tech talk recorded + slides committed
- [ ] Executive summary (≤ 1 page) committed

---

## Final Checklist Before Submitting

Tick every box. Each maps to an acceptance criterion in `requirements.md`.

- [ ] Portal deployed and reachable (URL or screenshot in README)
- [ ] OIDC login works; identity broker mints short-lived creds for ≥ 2 backends
- [ ] 2 fully implemented adapters; 1 stub adapter; conformance suite passes
- [ ] Cost cube returns `spend_by_team_by_week_by_platform` in ≤ 5 s
- [ ] `docs/cost-coverage.md` is honest (gaps named)
- [ ] 3 golden-path templates run on ≥ 2 backends each
- [ ] 5 workflow migrations documented with before/after metrics
- [ ] 6 ADRs merged
- [ ] `docs/operating-model.md` resolves on-call seams
- [ ] Tech talk recorded; slides + executive summary committed
- [ ] No long-lived per-user secrets anywhere
- [ ] No file > 800 LOC; ≥ 80 % test coverage on adapter SDK + normalizer
- [ ] `mypy --strict` clean on adapter contract

---

## Common Failure Modes — Read Before You Start

These are how previous principal-track learners lose points on integration projects.

1. **Built the portal first, talked to platform owners last.** You shipped a beautiful UI that the platform leads quietly veto in week 4 because the adapter contract assumes things their platform can't do.
2. **Tried to consolidate the platforms.** The charter said "integrate." You quietly drifted to "replace." The three platform teams stop returning your messages.
3. **Picked a custom portal over Backstage to "go faster."** Spent two weeks rebuilding catalog + auth + plugins. Should have spent those weeks on identity + cost.
4. **Long-lived per-platform credentials in the portal.** Security partner rejects the project; you redo identity in week 5. Don't.
5. **Cost numbers without a coverage doc.** A reviewer asks "what % of spend is in this number?" and the answer is "umm." This is the single biggest credibility loss.
6. **Adapter contract that grows to 80 methods.** The portal becomes a generic platform-control-plane. You can't keep adapters in sync. Stay minimal.
7. **No rollback path.** The portal goes down. Developers have no idea how to reach the underlying platform. The org loses trust in 24 hours.
8. **Templates that only work on one backend.** "Multi-backend" was a slogan. A reviewer tries the other backend and it crashes.
9. **Operating model that punts on the on-call seam.** "We'll figure that out post-launch." A real on-call event in week 6 burns the project's credibility.
10. **Tech talk that's a feature tour.** Forty minutes of "and look, you can also do this!" with no narrative. Bores the room. The story is: org pain → integration thesis → identity + cost story → adoption results → what we got wrong.

---

## What "Good" Looks Like at the End

A reviewer with 1 hour should be able to:
1. Log into your portal and immediately see live data from 3 backends
2. Open the cost view and find a `cost_coverage.md` link explaining the gaps
3. Trigger an action that requires identity federation and observe in audit logs that a short-lived cred was minted
4. Pick a golden-path template, scaffold a repo, and see CI run
5. Open one ADR and explain why you didn't pick the alternative
6. Read the operating model and answer "who pages whom when the portal returns wrong cost data?" in 30 seconds
7. Watch 5 minutes of your tech talk and understand the integration thesis

If a reviewer can do all 7 — you've passed. If the **platform leads on the three backends** would all say "yes, that integration story is fair to my platform" — you've hit 85+.
