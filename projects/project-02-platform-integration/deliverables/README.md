# Deliverables — Project 02: Cross-Team Platform Integration

This document defines exactly what to submit, in what format, and how it will be unpacked for review.

A complete submission is a single Git repository (public or accessible to your reviewer) following the structure below. Anything missing counts against you; anything mis-named delays review.

---

## Required Submission Inventory

| # | Artifact | Path | Format | Notes |
|---|---------|------|--------|-------|
| 1 | Charter | `docs/charter.md` | Markdown, ≤ 1 page | What this is and isn't; decision rights |
| 2 | Design Doc | `docs/design-doc.md` | Markdown, 12–20 pages | Architecture, identity, cost, adoption |
| 3 | ADRs | `adr/0001..0006-*.md` | Markdown, ≥ 1 page each | Min 6: adapter-contract, portal-choice, identity-model, cost-schema, template-strategy, rollback-model |
| 4 | Deployed portal | `portal/` + URL in README | Backstage app | Live data from ≥ 3 backends |
| 5 | Adapter SDK | `src/platform_sdk/` | Typed Python | Contract + conformance tests + ≥ 2 adapters + 1 stub |
| 6 | Conformance test report | `tests/conformance/report.md` | Markdown | Per-adapter pass/fail per test |
| 7 | Identity broker | `src/identity_broker/` | Python service | OIDC + token exchange for ≥ 2 backends |
| 8 | Role policy | `policies/role-mapping.{rego,yaml}` | Rego or YAML | Org group → per-platform role |
| 9 | Audit log query examples | `docs/audit-examples.md` | Markdown | Sample queries by `request_id`, by actor, by platform |
| 10 | Cost pipeline | `pipelines/cost/` | dbt + Dagster/Airflow | Ingest, normalize, cube, freshness checks |
| 11 | Cost coverage doc | `docs/cost-coverage.md` | Markdown | Per-platform attribution %, gaps, impact |
| 12 | Golden-path templates | `templates/{train-model,serve-model,evaluate-offline}/` | Backstage software templates | Each runs on ≥ 2 backends |
| 13 | Adoption pilot | `docs/adoption-pilot.md` | Markdown | 5 workflows, before/after metrics |
| 14 | Operating model | `docs/operating-model.md` | Markdown | On-call seams, SLAs, ownership |
| 15 | Stakeholder interviews | `docs/stakeholder-interviews.md` | Markdown | ≥ 3 platforms + security + FinOps |
| 16 | Executive summary | `docs/exec-summary.md` | Markdown, ≤ 1 page | For a non-engineer reader |
| 17 | Tech talk recording | `talks/tech-talk.mp4` or link in `talks/README.md` | Video, 25–45 min | Audio mandatory |
| 18 | Tech talk slides | `talks/slides.pdf` (and source if available) | PDF | Same content as recording |
| 19 | Getting Started | `docs/getting-started.md` | Markdown | Logged-in to first job in ≤ 30 min |
| 20 | Troubleshooting | `docs/troubleshooting.md` | Markdown | ≥ 10 entries |
| 21 | Top-level README | `README.md` | Markdown | What it is, what it isn't, quickstart, links |

---

## Repository Layout (Mandatory)

```
project-02-platform-integration/
├── README.md
├── pyproject.toml                    # adapter SDK + identity broker + ingesters
├── package.json                      # portal workspace root (if not in portal/)
├── Makefile                          # `make portal-up`, `make ingest`, `make conformance`
├── docs/
│   ├── charter.md
│   ├── design-doc.md
│   ├── stakeholder-interviews.md
│   ├── adoption-pilot.md
│   ├── operating-model.md
│   ├── cost-coverage.md
│   ├── exec-summary.md
│   ├── getting-started.md
│   ├── troubleshooting.md
│   └── audit-examples.md
├── adr/
│   ├── 0001-adapter-contract.md
│   ├── 0002-portal-choice.md
│   ├── 0003-identity-model.md
│   ├── 0004-cost-schema.md
│   ├── 0005-template-strategy.md
│   └── 0006-rollback-model.md
├── portal/                           # Backstage app
│   ├── app-config.yaml
│   ├── packages/app/
│   ├── packages/backend/
│   └── plugins/
│       ├── ml-jobs/
│       ├── ml-models/
│       ├── ml-endpoints/
│       └── ml-costs/
├── src/
│   ├── platform_sdk/                 # adapter contract + conformance tests
│   │   ├── contract.py
│   │   ├── conformance/
│   │   └── adapters/
│   │       ├── kubeflow/
│   │       ├── sagemaker/            # or vertex/databricks — pick your 2 full + 1 stub
│   │       └── databricks/           # stub
│   └── identity_broker/
│       ├── server.py
│       ├── exchanges/
│       │   ├── aws_sts.py
│       │   ├── gcp_wif.py
│       │   └── k8s_token.py
│       └── policy.py                 # loads Rego or YAML
├── policies/
│   └── role-mapping.rego (or .yaml)
├── pipelines/
│   └── cost/
│       ├── dagster_project/ (or airflow/dags/)
│       ├── dbt_project/
│       │   ├── models/
│       │   │   ├── staging/
│       │   │   ├── normalize/
│       │   │   └── aggregates/
│       │   └── tests/
│       └── ingesters/
│           ├── aws_cur.py
│           ├── gcp_billing.py
│           ├── azure_cm.py
│           └── opencost.py
├── templates/
│   ├── train-model/
│   ├── serve-model/
│   └── evaluate-offline/
├── tests/
│   ├── sdk/
│   ├── conformance/
│   │   └── report.md
│   ├── broker/
│   └── pipelines/
├── monitoring/
│   ├── grafana/                      # dashboard JSON
│   └── otel/
└── talks/
    ├── README.md                     # link to video if hosted externally
    ├── slides.pdf
    └── tech-talk.mp4 (or .url)
```

---

## Naming Conventions

- **ADRs:** `NNNN-kebab-case-title.md`, sequential starting `0001`.
- **Templates:** kebab-case directory names; each contains a `template.yaml` (Backstage standard).
- **Diagrams:** Mermaid inline in Markdown wherever possible; if PNG, `docs/img/<name>.png`.

---

## Format Requirements

- All Markdown is GitHub-flavored.
- Diagrams in Mermaid where rendering matters; otherwise `.png` (1×) and `.png@2x` (retina).
- Code blocks always have a language tag.
- No file > 800 LOC. No `.md` doc > 1500 lines (split if longer).
- Video: H.264 mp4 preferred; if hosted externally (Vimeo, internal Drive, Loom), `talks/README.md` provides a stable link.
- Portal URL: if reviewer can't reach a live instance, include a 5–10 min screencast under `talks/portal-walkthrough.mp4`.

---

## What You Will Be Asked at Review

A reviewer will sit with your repo for ~60 minutes and try to:

1. Read the top-level README and understand the project in 2 minutes.
2. Reach the portal (live or via screencast) and see live data from at least 3 backends.
3. Trigger an action that requires identity federation and observe in the audit log that a short-lived cred was minted.
4. Run `make conformance` and see the adapter test report.
5. Open `docs/cost-coverage.md` and answer "what % of last month's spend on platform X is attributed to a team?".
6. Pick one template, scaffold it, and confirm it works on both backends it claims.
7. Open `docs/operating-model.md` and answer "who pages whom when the portal returns wrong cost data?"
8. Open ADR `0001-adapter-contract.md` and explain why you didn't pick the alternative.
9. Watch 5 minutes of your talk at random and follow the narrative.

If any of these fail, the corresponding dimension in `rubric.md` loses a level.

---

## Submission Checklist

Before declaring done:

- [ ] All 21 inventory items present at the documented paths
- [ ] `make portal-up` brings up the portal (or screencast included)
- [ ] `make conformance` passes for ≥ 2 adapters
- [ ] `make ingest` runs the cost pipeline end-to-end on sample data
- [ ] OIDC login works against your IdP; audit log shows token exchange events
- [ ] Cost cube returns `spend_by_team_by_week_by_platform` in ≤ 5 s
- [ ] 3 templates instantiated end-to-end on ≥ 2 backends each
- [ ] 5 workflow migrations documented in `docs/adoption-pilot.md` with before/after metrics
- [ ] 6 ADRs merged with full structure (context, decision, alternatives, consequences)
- [ ] Tech talk recording present (or link); slides + executive summary committed
- [ ] At least 3 named reviewers acknowledged in design doc front-matter, one from each platform team
- [ ] Self-assessment in `docs/self-assessment.md` with your scores per rubric dimension
