# Deliverables — Project 04: Technical Innovation POC

This document defines exactly what to submit, in what format, and how it will be unpacked for review.

A complete submission is a single Git repository (public or accessible to your reviewer) following the structure below. Anything missing counts against you; anything mis-named delays review.

---

## Required Submission Inventory

| # | Artifact | Path | Format | Notes |
|---|---------|------|--------|-------|
| 1 | Technique selection | `docs/technique-selection.md` | Markdown, 1–2 pages | Compares ≥ 2 alternatives |
| 2 | Charter | `docs/charter.md` | Markdown, ≤ 1 page | Org-level question, time box, recommender, sponsor |
| 3 | Pre-registered success criteria | `docs/success-criteria.md` (+ YAML) | Markdown + YAML | Tagged commit `pre-registration-v1` before implementation |
| 4 | Design Doc | `docs/design-doc.md` | Markdown, 10–18 pages | POC plan, harness, stress matrix, recommendation framing |
| 5 | ADRs | `adr/0001..0005-*.md` | Markdown, ≥ 1 page each | Technique choice, methodology, harness, integration, recommendation framing |
| 6 | Baseline + final benchmarks | `benchmarks/baseline.md`, `benchmarks/final.md` | Markdown + raw data | 95 % CI; noise floor |
| 7 | POC source | `src/poc/` | Python (typed) | Toggleable via flag; pip-installable |
| 8 | Evaluation harness | `src/harness/` + `Makefile` target `make harness` | Python | Two-arm; structured JSON + Markdown output |
| 9 | Experiment log | `experiments/experiment-log.md` | Markdown | ≥ 12 experiments with full structure |
| 10 | Per-experiment repro | `repro/<exp-id>/` | Shell + config | At least 3 reproducible end-to-end |
| 11 | Stress test report | `docs/stress-tests.md` | Markdown | ≥ 3 stress conditions; failure modes |
| 12 | Boundary analysis | `docs/boundary-analysis.md` | Markdown | Where the technique works / breaks |
| 13 | Productionization gaps | `docs/productionization-gaps.md` | Markdown + Mermaid | Each gap with estimate, owner, deps, risk |
| 14 | Confidence | `docs/confidence.md` | Markdown | Calibrated probability + what would change my mind |
| 15 | Recommendation | `docs/recommendation.md` | Markdown + YAML | Unambiguous go/no-go/not-yet + triggers |
| 16 | POC report | `docs/poc-report.md` | Markdown, 8–14 pages | Opens with recommendation |
| 17 | Hand-off | `docs/hand-off.md` | Markdown | Next team, first 30 days |
| 18 | Where to pick up | `docs/where-to-pick-up.md` | Markdown | Cold-start guide |
| 19 | Upstream PRs (if any) | `docs/upstream-prs.md` | Markdown | PR links + status |
| 20 | Tech talk recording | `talks/tech-talk.mp4` or link in `talks/README.md` | Video, 25–40 min | Audio mandatory |
| 21 | Tech talk slides | `talks/slides.pdf` | PDF | Same content as recording |
| 22 | Self-assessment | `docs/self-assessment.md` | Markdown | Per-rubric scores |
| 23 | Top-level README | `README.md` | Markdown | Recommendation in opening; quickstart; links |

---

## Repository Layout (Mandatory)

```
project-04-innovation-poc/
├── README.md
├── pyproject.toml
├── Makefile                          # `make poc`, `make harness`, `make repro EXP=003`
├── docs/
│   ├── technique-selection.md
│   ├── charter.md
│   ├── success-criteria.md
│   ├── success-criteria.yaml
│   ├── design-doc.md
│   ├── methodology.md
│   ├── stress-tests.md
│   ├── boundary-analysis.md
│   ├── productionization-gaps.md
│   ├── confidence.md
│   ├── recommendation.md
│   ├── recommendation.yaml
│   ├── poc-report.md
│   ├── hand-off.md
│   ├── where-to-pick-up.md
│   ├── upstream-prs.md
│   ├── current-bet.md                # mid-week snapshot
│   └── self-assessment.md
├── adr/
│   ├── 0001-technique-choice.md
│   ├── 0002-measurement-methodology.md
│   ├── 0003-harness-design.md
│   ├── 0004-integration-boundary.md
│   └── 0005-recommendation-framing.md
├── benchmarks/
│   ├── baseline.md
│   ├── final.md
│   └── raw/
├── experiments/
│   ├── plan.md
│   └── experiment-log.md
├── repro/
│   ├── 001-paper-regime/
│   ├── 006-stress-longcontext/
│   └── 011-scale-up/
├── src/
│   ├── poc/
│   │   ├── __init__.py
│   │   ├── technique.py
│   │   └── integration.py
│   └── harness/
│       ├── __init__.py
│       ├── runner.py
│       ├── stats.py
│       └── report.py
├── tests/
│   ├── poc/
│   └── harness/
├── monitoring/
│   └── grafana/
└── talks/
    ├── README.md
    ├── slides.pdf
    └── tech-talk.mp4 (or .url)
```

---

## Naming Conventions

- **ADRs:** `NNNN-kebab-case-title.md`, sequential.
- **Experiments:** Three-digit ID; directory `repro/<NNN>-<short-title>/`.
- **Pre-registration tag:** `pre-registration-v1` on the commit that introduces `docs/success-criteria.md`. If you revise criteria mid-project (don't), the new version is `pre-registration-v2` and the report calls out the change as a methodology lesson.
- **Diagrams:** Mermaid inline where rendering matters.

---

## Format Requirements

- All Markdown is GitHub-flavored.
- Code blocks always have a language tag.
- No file > 800 LOC.
- Video: H.264 mp4 preferred; if hosted externally, `talks/README.md` provides the stable link.
- Recommendation and success-criteria committed as **both** Markdown and YAML so machines and humans can both consume them.

---

## What You Will Be Asked at Review

A reviewer will sit with your repo for ~60 minutes and try to:

1. Read the README and quote the recommendation paragraph.
2. Verify pre-registration is real — check the `pre-registration-v1` commit timestamp.
3. Run `make poc` and see the headline reproduce.
4. Run `make harness` and see the structured JSON + Markdown report.
5. Pick an experiment ID from `experiment-log.md` and inspect the `repro/` directory.
6. Open `docs/stress-tests.md` and confirm at least 3 stress conditions with failure modes.
7. Open `docs/productionization-gaps.md` and find the dependency graph + engineer-week estimates.
8. Read `docs/confidence.md` and find the "what would change my mind" section.
9. Read the recommendation aloud and verify it's unambiguous.
10. Watch 5 minutes of the talk at random.
11. Read `docs/where-to-pick-up.md` and simulate picking the project up cold.

If any of these fail, the corresponding dimension in `rubric.md` loses a level.

---

## Submission Checklist

Before declaring done:

- [ ] All 23 inventory items present at the documented paths
- [ ] `make poc` reproduces the headline within stated budget
- [ ] `make harness` produces JSON + Markdown report
- [ ] Pre-registration commit tag exists; criteria unchanged
- [ ] ≥ 12 experiments in `experiment-log.md` with full structure
- [ ] ≥ 3 stress + ≥ 2 negative experiments documented
- [ ] Recommendation in `docs/recommendation.md` is unambiguous and calibrated
- [ ] POC report opens with the recommendation
- [ ] Hand-off + where-to-pick-up validated by a peer
- [ ] At least 3 named reviewers acknowledged in design doc front-matter (one technical peer, one sponsor, one productionization team representative)
- [ ] Self-assessment committed with honest scores
- [ ] Tech talk recorded; slides committed
