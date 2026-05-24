# Deliverables — Project 01: Distributed Training Framework

This document defines exactly what to submit, in what format, and how it will be unpacked for review.

A complete submission is a single Git repository (public or accessible to your reviewer) following the structure below. Anything missing counts against you; anything mis-named delays review.

---

## Required Submission Inventory

| # | Artifact | Path | Format | Notes |
|---|---------|------|--------|-------|
| 1 | Design Doc | `docs/design-doc.md` | Markdown, 15–25 pages | Must have a section per `STEP_BY_STEP.md` Week 1 outline |
| 2 | Architecture Decision Records | `adr/0001..0006-*.md` | Markdown, ≥ 1 page each | Six minimum; use any standard ADR template |
| 3 | Framework source code | `src/training_framework/` | Python, type-checked | Pip-installable; tests + mypy CI configured |
| 4 | Test suite | `tests/` | pytest | ≥ 80 % coverage on `control/`, `runtime/checkpoint/`, `shared/` |
| 5 | Reference workload | `examples/reference-7b/` | YAML job spec + model code | Must run end-to-end on ≥ 32 GPUs |
| 6 | Benchmark report | `benchmarks/reference-7b.md` | Markdown + figures | MFU number, loss curve, hardware, command to reproduce |
| 7 | Optimization log | `benchmarks/optimization-log.md` | Markdown table | ≥ 5 rows: change, before, after, attribution |
| 8 | Chaos test suite | `scripts/chaos/` + `docs/chaos-report.md` | Shell + Markdown | 10 scenarios, results recorded |
| 9 | Migration plan | `docs/migration-plan.md` | Markdown | 3 teams, each with steps + rollback + metric + owner |
| 10 | Cost model | `docs/cost-model.xlsx` *or* `notebooks/cost-model.ipynb` | Spreadsheet or notebook | Assumptions separated from outputs |
| 11 | Tech talk recording | `talks/tech-talk.mp4` or link in `talks/README.md` | Video, 25–45 min | Audio mandatory; screen/face optional |
| 12 | Tech talk slides | `talks/slides.pdf` (and source if available) | PDF | Same content as recording |
| 13 | On-call runbook | `docs/oncall.md` | Markdown | ≥ 8 incident playbooks |
| 14 | Failure postmortem | `docs/postmortems/0001-*.md` | Markdown | Real or induced failure from this project |
| 15 | Stakeholder interview notes | `docs/stakeholder-interviews.md` | Markdown | ≥ 3 teams |
| 16 | Getting Started guide | `docs/getting-started.md` | Markdown | A stranger can submit a tiny job in ≤ 30 min |
| 17 | Troubleshooting guide | `docs/troubleshooting.md` | Markdown | ≥ 10 entries |
| 18 | Top-level README | `README.md` | Markdown | What it is, what it isn't, quickstart, links to all of the above |

---

## Repository Layout (Mandatory)

```
project-01-distributed-training/
├── README.md
├── pyproject.toml
├── requirements.txt
├── Makefile                          # `make demo`, `make chaos`, `make test`
├── docs/
│   ├── design-doc.md
│   ├── migration-plan.md
│   ├── stakeholder-interviews.md
│   ├── getting-started.md
│   ├── troubleshooting.md
│   ├── oncall.md
│   ├── cost-model.xlsx (or notebooks/cost-model.ipynb)
│   ├── chaos-report.md
│   └── postmortems/
│       └── 0001-<title>.md
├── adr/
│   ├── 0001-sharding-strategy.md
│   ├── 0002-checkpoint-format.md
│   ├── 0003-cluster-mgr-abstraction.md
│   ├── 0004-comms-backend.md
│   ├── 0005-observability-schema.md
│   └── 0006-security-boundary.md
├── src/
│   └── training_framework/
│       ├── api/
│       ├── cli/
│       ├── control/
│       ├── runtime/
│       ├── backends/k8s/
│       ├── backends/slurm/
│       └── shared/
├── tests/
├── examples/
│   ├── tiny-gpt/
│   └── reference-7b/
├── benchmarks/
│   ├── reference-7b.md
│   └── optimization-log.md
├── scripts/
│   └── chaos/
│       ├── run_all.sh
│       └── *.sh
├── monitoring/
│   └── grafana/                      # exported dashboard JSON
└── talks/
    ├── README.md                     # link to video if hosted externally
    ├── slides.pdf
    └── tech-talk.mp4 (or .url)
```

If you genuinely cannot include a deliverable (e.g., real recording vs link), include a `*.url` text file pointing to it.

---

## Naming Conventions

- **ADRs:** `NNNN-kebab-case-title.md`, sequential starting `0001`.
- **Postmortems:** `NNNN-kebab-case-title.md`, sequential, dated in front-matter.
- **Benchmarks:** `<workload>-<scale>-<date>.md` (e.g., `reference-7b-32gpu-2026-05-14.md`).
- **Diagrams:** Mermaid inline in Markdown wherever possible; if PNG, `docs/img/<name>.png`.

---

## Format Requirements

- All Markdown uses GitHub-flavored Markdown.
- Diagrams in Mermaid where rendering matters; otherwise `.png` 1× and `.png@2x` for retina.
- Code blocks always have a language tag.
- No file > 800 LOC. No `.md` doc > 1500 lines (split if longer).
- Cost spreadsheet: assumptions and outputs on separate sheets, named.
- Video: H.264 mp4 preferred; if hosted externally (Vimeo, internal Drive, Loom), `talks/README.md` provides a stable link.

---

## What You Will Be Asked at Review

A reviewer will sit with your repo for ~60 minutes and try to:

1. Read the top-level README and understand the project in 2 minutes.
2. Reproduce the demo workflow (`make demo`) without intervention.
3. Find your failure model in the design doc within 30 seconds.
4. Open ADR 0001 and explain why you didn't pick the alternative.
5. Watch 5 minutes of your talk at random.
6. Pick one chaos scenario and trace it from the script through the runtime to the recovery.
7. Open the migration plan and pick one team — could they actually follow this?

If any of these fail, the corresponding dimension in `rubric.md` loses a level.

---

## Submission Checklist

Before declaring done:

- [ ] All 18 inventory items present at the documented paths
- [ ] `make test` passes
- [ ] `make chaos` passes (or documents which scenarios are skipped and why)
- [ ] `make demo` runs end-to-end on a small GPU box
- [ ] Tech talk recording present (or link)
- [ ] At least 3 named reviewers acknowledged in design doc front-matter
- [ ] Self-assessment in `docs/self-assessment.md` with your scores per rubric dimension
- [ ] Last commit message contains the SHA and date you consider "final"
