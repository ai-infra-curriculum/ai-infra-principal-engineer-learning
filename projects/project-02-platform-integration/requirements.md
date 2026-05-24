# Requirements — Project 02: Cross-Team Platform Integration

This document defines what the integration platform must do and must not do, the constraints you accept, the assumptions you may make, and the acceptance criteria a reviewer will check.

Requirements use **MoSCoW** prioritization: **M**ust, **S**hould, **C**ould, **W**on't.

---

## 1. Functional Requirements

### 1.1 Developer Portal (Must)

- **M-FR-1**: A Backstage (or equivalent IDP — Port, Cortex, Roadie) instance is deployed and reachable, showing live data from at least 3 backend ML platforms.
- **M-FR-2**: The portal has at minimum these views, each backed by live adapter calls (not screenshots, not seeded data):
  - **Jobs** — list training jobs across platforms, filterable by team / status / cluster
  - **Models** — registered models across platforms with version, owner, last evaluation
  - **Endpoints** — live serving endpoints with health, traffic, p50/p99 latency, $/req
  - **Costs** — per-team spend over time, broken down by platform and resource type
- **M-FR-3**: Each view links **back** to the underlying platform's native UI for the specific resource (deep link), so a developer can always "drop down" to the source of truth.
- **S-FR-4**: A single global search returns matching jobs / models / endpoints across all platforms.
- **C-FR-5**: A "diff" view shows model lineage across promotions (training run → eval → registered → deployed).

### 1.2 Adapter SDK (Must)

- **M-FR-6**: A typed adapter contract is defined (Python, with mypy strict). At minimum it covers:
  - `list_jobs(filter, page)`, `get_job(id)`, `cancel_job(id)`
  - `list_models()`, `get_model(id, version)`
  - `list_endpoints()`, `get_endpoint(id)`
  - `cost_records(time_range)` — yields normalized cost records
  - `exchange_token(user_token)` — see §1.3
- **M-FR-7**: At least **2 adapters** are fully implemented end-to-end. A 3rd is stubbed with TODOs no larger than one work-day each.
- **M-FR-8**: Adapter conformance tests live in the SDK; any new adapter runs the same suite. ≥ 30 contract tests.
- **S-FR-9**: Adapter SDK includes a `MockAdapter` that the portal can run against in local development, returning realistic fixtures.

### 1.3 Federated Identity (Must)

- **M-FR-10**: A single user logs into the portal via OIDC (org IdP). The portal does **not** ask for per-platform credentials.
- **M-FR-11**: When the portal calls a backend platform, it exchanges the user's ID token for a short-lived per-platform credential via:
  - AWS: STS `AssumeRoleWithWebIdentity` (or IAM Identity Center)
  - GCP: Workload Identity Federation
  - Azure: Workload Identity / federated credentials
  - Kubernetes: TokenRequest + service account binding
  - SaaS (Databricks etc.): platform-native OAuth 2.0 token exchange (RFC 8693) if supported, otherwise documented fallback
- **M-FR-12**: All per-platform credentials are **short-lived** (≤ 1 hour). No long-lived per-user secrets are stored in the portal.
- **M-FR-13**: Auditable: every cross-platform action logs `actor`, `principal_on_platform`, `action`, `resource`, `request_id`, `ts`.
- **S-FR-14**: Role mapping is declarative — a YAML or Rego policy maps an org group to a per-platform role per platform.

### 1.4 Cost Attribution (Must)

- **M-FR-15**: A pipeline ingests cost / usage data from each platform on at least a daily cadence. Sources include:
  - AWS Cost and Usage Report (CUR) or Cost Explorer API
  - GCP Billing Export to BigQuery
  - Azure Cost Management Export
  - Databricks usage logs / Vertex billing / SageMaker usage
  - Kubernetes cluster cost (OpenCost or kubecost-equivalent)
- **M-FR-16**: A normalized cost record schema is used across sources, at minimum:
  - `ts`, `platform`, `account`, `team`, `project`, `resource_type`, `resource_id`, `quantity`, `unit`, `cost_usd`, `currency`, `region`, `is_spot`, `tags{}`
- **M-FR-17**: The cube is queryable: `cost_by_team_by_week_by_platform` returns in ≤ 5 s at the loaded scale.
- **M-FR-18**: Coverage gaps are explicit. A `cost_coverage.md` doc states for each platform: what is captured, what is missing, what the % attribution rate is.
- **S-FR-19**: A `forecast` view projects month-end cost based on month-to-date trends, per team and total.
- **C-FR-20**: A "what-if" view re-runs the cost cube under hypothetical assumptions (spot mix, region change, GPU type change).

### 1.5 Golden-Path Templates (Must)

- **M-FR-21**: At least **3 templates** are shipped as Backstage software templates:
  - **`train-model`** — scaffolds a new training job repo, wires CI, registers a stub model
  - **`serve-model`** — scaffolds a serving endpoint from a registered model
  - **`evaluate-offline`** — scaffolds an offline eval job
- **M-FR-22**: Each template runs end-to-end on **at least 2 backends** (e.g., `train-model` works against both Kubeflow and SageMaker).
- **M-FR-23**: Templates emit telemetry: which template, which user, which backend, success/fail. Used to measure adoption.
- **S-FR-24**: Templates pin the developer's choice of backend at scaffold time but can be re-platformed later by changing one file.

### 1.6 Operating Model (Must)

- **M-FR-25**: An `operating-model.md` doc defines:
  - Who owns the portal code
  - Who owns each adapter
  - Who pages whom when (portal down vs adapter wrong vs underlying platform down)
  - SLA for the portal (e.g., 99.5 % availability, p95 page load ≤ 2 s)
  - Change-management process for adapter contract changes
- **M-FR-26**: A rollback path is documented: if the portal returns wrong data, a developer can go directly to the underlying platform within 5 minutes; no operations are blocked by portal availability.

---

## 2. Non-Functional Requirements

### 2.1 Performance (Must / Should)

| ID | Requirement | Target |
|----|-------------|--------|
| M-NFR-1 | Portal page load (p95) | ≤ 2 s for any single-resource view |
| M-NFR-2 | Cost cube query: spend-by-team-by-week | ≤ 5 s at the loaded scale |
| S-NFR-3 | Adapter call timeout | ≤ 3 s per call; fail open with a "stale" badge on cached data |
| S-NFR-4 | Global search (across platforms) | ≤ 2 s p95 over the indexed set |

### 2.2 Reliability (Must)

- **M-NFR-5**: Portal degrades gracefully if one adapter is down — other views remain functional; a banner names the affected backend.
- **M-NFR-6**: Cost pipeline is idempotent — re-running yesterday's ingest does not double-count.
- **M-NFR-7**: Identity broker has no single-region single point of failure.

### 2.3 Security (Must)

- **M-NFR-8**: All inter-component calls are mTLS or HTTPS with verified certificates.
- **M-NFR-9**: Per-platform credentials are ≤ 1 hour TTL. No long-lived per-user secrets stored anywhere.
- **M-NFR-10**: Audit log is append-only, retained ≥ 1 year, queryable by `request_id`.
- **M-NFR-11**: No PII or model contents flow through the portal beyond what the user sees in a view.

### 2.4 Maintainability (Must)

- **M-NFR-12**: ≥ 80 % unit test coverage on the adapter SDK and the cost-normalization layer.
- **M-NFR-13**: All public adapter contract types are typed (mypy strict). Adapter implementations may relax strictness with documented rationale.
- **M-NFR-14**: No file > 800 LOC. Functions ≤ 50 LOC. Modules organized by feature.
- **M-NFR-15**: ADRs cover the major decisions (portal choice, identity model, cost schema, adapter contract, template strategy, rollback model).

### 2.5 Usability (Should)

- **S-NFR-16**: A new developer can submit their first training job via a golden-path template in ≤ 30 minutes from "logged into portal" to "job is running".
- **S-NFR-17**: Error messages name the affected platform, the affected resource, and a next step (link or runbook), not just a stack trace.

---

## 3. Constraints

- **C-1**: You may **not** replace any of the underlying platforms. Integration only. If you find yourself rewriting an existing platform, reset.
- **C-2**: You may not require platform owners to vendor any of your code. The adapter for their platform is yours to operate; they expose a stable API.
- **C-3**: Your portal must not become a single point of failure for any business-critical workflow. If the portal is down, developers must be able to bypass it.
- **C-4**: No long-lived per-user credentials. All cross-platform authentication is via short-lived tokens.
- **C-5**: Cost data is normalized but **not** fabricated. If a platform doesn't expose a number, the cube shows `null` and the coverage doc lists the gap.
- **C-6**: You may not require a specific cloud or vendor. The integration story must be vendor-agnostic at the adapter contract layer.
- **C-7**: Backstage (or equivalent IDP) plugin code must be open-sourceable — no proprietary dependencies in the portal source.

---

## 4. Assumptions

You may assume the following without further justification. Substitute and document if absent.

- **A-1**: Three backend ML platforms exist and you have read-and-act-on-behalf-of-user API access to each (real or simulated).
- **A-2**: An org-wide OIDC IdP (Okta, Entra, Google, Auth0) is available and you can register apps there.
- **A-3**: A data warehouse (BigQuery / Snowflake / ClickHouse / Postgres) is available for the cost cube.
- **A-4**: A team membership source of truth exists (LDAP, SCIM-synced group, Backstage Catalog). If not, document the substitute.
- **A-5**: You have permission to deploy Backstage somewhere reachable to your reviewers.
- **A-6**: Each platform has at least *some* form of usage / cost export — even if it's a CSV.

---

## 5. Out of Scope (Won't)

To keep this project at 80 hours:

- **W-1**: You will not build new ML platforms. Integration only.
- **W-2**: You will not build a model registry. Existing platform registries are surfaced through the portal.
- **W-3**: You will not build a billing system. The cost cube enables reporting; actual chargeback / invoicing is out of scope.
- **W-4**: You will not solve cross-cloud data movement or replication.
- **W-5**: You will not implement vendor-specific deep features beyond what the adapter contract exposes (e.g., SageMaker-only autoscaling tuning).
- **W-6**: You will not build a custom IdP. Federate to an existing one.
- **W-7**: You will not solve cross-team approval workflows (model promotion, deploy gates). The portal *displays* them, doesn't *implement* them.
- **W-8**: You will not implement RBAC inside the portal beyond view-level — fine-grained per-resource RBAC is delegated to the underlying platforms.

---

## 6. Acceptance Criteria

A reviewer should be able to mechanically check these.

### A. Portal works end-to-end
1. From a fresh checkout, following docs, a reviewer can deploy the portal in ≤ 60 minutes and log in via OIDC.
2. The Jobs / Models / Endpoints / Costs views all return data from at least 3 backend platforms within 5 seconds of page load.
3. Deep links from a portal view to the underlying platform's native UI all work and land on the correct resource.

### B. Adapter SDK is real
4. The adapter SDK has documented types and a conformance test suite.
5. ≥ 2 adapters pass the conformance suite.
6. A 3rd adapter stub exists with TODOs each ≤ 1 day of work.
7. The SDK README contains a "build a 4th adapter in a week" checklist.

### C. Identity federation works
8. A reviewer logs in once and triggers an action on at least 2 backends, observing in audit logs that short-lived per-platform credentials were minted.
9. No long-lived per-user secrets exist anywhere in the system.
10. Audit log query by `request_id` returns the full cross-platform trace.

### D. Cost attribution is defensible
11. The cost cube returns `spend by team by week by platform` in ≤ 5 s.
12. `cost_coverage.md` lists every platform with attribution rate, known gaps, and the impact of each gap on the total.
13. A non-engineer (or a manager) can read the executive summary and understand the top 3 cost drivers in under 5 minutes.

### E. Golden paths work
14. The `train-model` template scaffolds a new repo that runs a training job on at least 2 backends.
15. The `serve-model` and `evaluate-offline` templates run on at least 2 backends each.
16. Template usage telemetry exists and at least 5 workflow migrations are documented with before/after metrics.

### F. Operating model + docs
17. `operating-model.md` answers "who pages whom when X" for at least 5 incident classes.
18. Rollback path is documented and a reviewer can execute it (or read through it) in under 5 minutes.
19. Tech talk recorded, slides committed.

### G. Quality bar
20. Unit test coverage ≥ 80 % on adapter SDK + cost normalization.
21. `mypy --strict` clean on adapter contract.
22. No file > 800 LOC.

---

## 7. Dependencies on Other Teams (For the Plan, Not the Code)

For the charter, design doc, and operating model, you must explicitly identify dependencies on:

- **Each platform team** (3+) — for API stability, on-call seam, adapter contract sign-off
- **Security / IAM team** — for OIDC app registration, identity broker review, audit log policy
- **FinOps / Finance team** — for the price book, normalized schema, chargeback policy alignment
- **Platform / Infra team** — for hosting the portal, the identity broker, and the cost pipeline
- **3+ adopting teams** — for the 5 workflow migrations

Naming real people / orgs is not required for the rubric. Naming real *roles* and *responsibilities* is.

---

## 8. Glossary

- **IDP / Developer Portal** — Internal Developer Platform (e.g., Backstage, Port, Cortex, Roadie). Not to be confused with IdP.
- **IdP** — Identity Provider (Okta, Entra, Google, Auth0)
- **OIDC** — OpenID Connect
- **Token Exchange (RFC 8693)** — OAuth 2.0 mechanism to swap one token for another with narrower scope or different audience
- **CUR / Billing Export** — cloud-provider-specific raw cost data
- **Cost Cube** — normalized warehouse table queryable across dimensions
- **Adapter** — code implementing the platform-integration contract for one backend
- **Golden Path** — a paved, opinionated, scaffolded workflow that "just works"
- **Coverage Rate** — share of true cost captured by the attribution pipeline
- **Operating Model** — who owns what, who pages whom, change management
