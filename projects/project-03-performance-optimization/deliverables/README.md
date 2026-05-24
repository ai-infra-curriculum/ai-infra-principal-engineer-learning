# Deliverables — Project 03: Performance Optimization Initiative

This document defines exactly what to submit, in what format, and how it will be unpacked for review.

A complete submission is a single Git repository (public or accessible to your reviewer) following the structure below. Anything missing counts against you; anything mis-named delays review.

---

## Required Submission Inventory

| # | Artifact | Path | Format | Notes |
|---|---------|------|--------|-------|
| 1 | Target selection | `docs/target-selection.md` | Markdown, ≤ 1 page | Why inference *or* training, not both |
| 2 | Workload definition | `docs/workload.md` | Markdown | Model, request distribution, hardware, software stack, pinned versions |
| 3 | Methodology | `docs/methodology.md` | Markdown | Pre-registered; sample size + power calc; statistical test |
| 4 | Design Doc | `docs/design-doc.md` | Markdown, 12–20 pages | Problem, metric, methodology, campaign plan, narrative |
| 5 | ADRs | `adr/0001..0006-*.md` | Markdown, ≥ 1 page each | Metric choice, methodology, top 3 bets, regression prevention, durability, rollback |
| 6 | Baseline + final benchmarks | `benchmarks/baseline.md`, `benchmarks/final.md` | Markdown + raw data | Each with 95 % CI, N, noise floor |
| 7 | Experiment log | `experiments/experiment-log.md` | Markdown | ≥ 20 experiments; ≥ 5 negative |
| 8 | Per-experiment repro | `repro/<exp-id>/` | Shell scripts + config | At least 3 representative experiments runnable end-to-end |
| 9 | Profile traces | `profiles/{baseline,roundN,final}/` | `.nsys-rep` / `.ncu-rep` / PyTorch profiler JSON | Annotated screenshots committed alongside |
| 10 | Cross-layer narrative | `docs/cross-layer-narrative.md` | Markdown | At least one symptom-to-root-cause story across all 4 layers |
| 11 | Optimization code | `src/` + upstream PR links in `docs/upstream-prs.md` | Code | Patches landed or PR'd; never private branches only |
| 12 | Kernel code (if any) | `kernels/` | Triton or CUDA | Numerics tests + benchmark |
| 13 | Perf-tunables | `docs/perf-tunables.md` | Markdown | Every introduced flag / env var with default + recommended + rationale |
| 14 | Perf CI | `.github/workflows/perf-ci.yml` (or equivalent) + `ci/` | YAML + scripts | Nightly; statistical regression detection |
| 15 | Regression demo | `docs/regression-demo.md` + PR link | Markdown | Synthetic 5 % regression caught in one run |
| 16 | Perf runbook | `docs/perf-runbook.md` | Markdown | Repro / bisect / escalate |
| 17 | Durability assessment | `docs/durability.md` | Markdown | Per optimization: survives next release? next upgrade? next hardware? |
| 18 | Rollback runbooks | `docs/rollbacks/*.md` | Markdown | One per production-touching change |
| 19 | Canary plan | `docs/canary.md` | Markdown | 1/10/50/100; breaking metrics per stage |
| 20 | Reliability SLO check | `docs/reliability-slo.md` | Markdown | Pre / post / correctness diff |
| 21 | Executive summary | `docs/exec-summary.md` | Markdown, ≤ 1 page | Non-engineer readable in 90 s |
| 22 | Tech talk recording | `talks/tech-talk.mp4` or link in `talks/README.md` | Video, 25–40 min | Audio mandatory |
| 23 | Tech talk slides | `talks/slides.pdf` | PDF | Same content as recording |
| 24 | Self-assessment | `docs/self-assessment.md` | Markdown | Per-dimension scores per rubric |
| 25 | Top-level README | `README.md` | Markdown | Headline number, business win, quickstart, links |

---

## Repository Layout (Mandatory)

```
project-03-performance-optimization/
├── README.md
├── pyproject.toml
├── Makefile                          # `make baseline`, `make experiment EXP=014`, `make perf-ci`
├── docs/
│   ├── target-selection.md
│   ├── workload.md
│   ├── methodology.md
│   ├── design-doc.md
│   ├── cross-layer-narrative.md
│   ├── perf-tunables.md
│   ├── perf-runbook.md
│   ├── durability.md
│   ├── canary.md
│   ├── reliability-slo.md
│   ├── exec-summary.md
│   ├── upstream-prs.md
│   ├── regression-demo.md
│   ├── self-assessment.md
│   └── rollbacks/
│       └── *.md
├── adr/
│   ├── 0001-metric-choice.md
│   ├── 0002-measurement-methodology.md
│   ├── 0003-bet-kernel-FA3-FP8.md      # or your top kernel bet
│   ├── 0004-bet-framework-vllm.md      # or your top framework bet
│   ├── 0005-regression-prevention.md
│   └── 0006-rollback-canary-model.md
├── benchmarks/
│   ├── baseline.md
│   ├── final.md
│   └── raw/                            # CSVs or parquets
├── experiments/
│   ├── plan.md
│   └── experiment-log.md
├── repro/
│   ├── 003-fa3-enable/
│   ├── 011-paged-kv-tune/
│   └── 018-nccl-algo-tree/
├── profiles/
│   ├── baseline/
│   │   ├── trace.nsys-rep
│   │   └── annotated.md
│   ├── round1/
│   ├── round2/
│   └── final/
├── src/
│   ├── server/                        # if inference; or trainer/ if training
│   ├── adapters/
│   └── perf/                          # measurement utilities
├── kernels/                           # only if you authored or modified kernels
│   ├── triton/
│   └── tests/
├── ci/
│   └── perf/                          # scripts that the perf CI invokes
├── .github/workflows/perf-ci.yml      # or jenkins/buildkite equivalent
├── monitoring/
│   └── grafana/                       # dashboard JSON
└── talks/
    ├── README.md
    ├── slides.pdf
    └── tech-talk.mp4 (or .url)
```

---

## Naming Conventions

- **ADRs:** `NNNN-kebab-case-title.md`, sequential.
- **Experiments:** Three-digit ID; directory `repro/<NNN>-<short-title>/`.
- **Profiles:** Named by phase (`baseline`, `round1`, `round2`, `final`) + descriptor.
- **Diagrams:** Mermaid inline in Markdown wherever possible.

---

## Format Requirements

- All Markdown is GitHub-flavored.
- Profile files (`.nsys-rep`, `.ncu-rep`) committed if under a few hundred MB; otherwise stored externally with stable links in `profiles/<phase>/index.md`.
- Annotated screenshots: PNG at 1× and 2× retina.
- Code blocks always have a language tag.
- No file > 800 LOC. No `.md` doc > 1500 lines.
- Video: H.264 mp4 preferred; if hosted externally, `talks/README.md` provides the stable link.

---

## What You Will Be Asked at Review

A reviewer will sit with your repo for ~60 minutes and try to:

1. Read the README and quote the headline number and the business win.
2. Open `docs/methodology.md` and check the pre-registration is real (commit timestamps).
3. Pick an experiment ID at random from `experiment-log.md` and inspect the corresponding `repro/`.
4. Open a `.nsys-rep` (or screenshot) and follow the annotated bottleneck → fix → verification.
5. Look at the perf CI workflow and confirm it's nightly with statistical detection.
6. Find the regression-demo PR link and confirm a 5 % regression was caught.
7. Open `docs/durability.md` and find specific named follow-up actions, not generic ones.
8. Open `docs/rollbacks/` and pick one runbook — could they execute it?
9. Read `docs/exec-summary.md` aloud in 90 seconds.
10. Watch 5 minutes of the talk at random and follow the narrative.

If any of these fail, the corresponding dimension in `rubric.md` loses a level.

---

## Submission Checklist

Before declaring done:

- [ ] All 25 inventory items present at the documented paths
- [ ] `make baseline` reproduces baseline within stated CI
- [ ] `make experiment EXP=<id>` reproduces at least 3 experiments
- [ ] Perf CI green for last 7 nights (or documented why some failed)
- [ ] Regression demo PR linked
- [ ] Headline number in README matches benchmarks/final.md and the talk
- [ ] Reliability SLO maintained per `docs/reliability-slo.md`
- [ ] At least 3 named reviewers acknowledged in design doc front-matter (one perf-specialist, one model owner, one FinOps / product partner)
- [ ] Self-assessment committed with honest scores
