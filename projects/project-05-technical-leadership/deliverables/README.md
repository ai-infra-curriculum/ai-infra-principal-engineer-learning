# Deliverables — Project 05: Technical Leadership Capstone

This document defines exactly what to submit, in what format, and how it will be unpacked for review.

A complete submission is a single Git repository (public or accessible to your reviewer) following the structure below. Anything missing counts against you; anything mis-named delays review.

---

## Required Submission Inventory

| # | Artifact | Path | Format | Notes |
|---|---------|------|--------|-------|
| 1 | Worldview statement | `docs/worldview.md` | Markdown, 3–5 pages | ≥ 3 falsifiable claims, ≥ 2 non-claims, named blind spots |
| 2 | Design doc | `docs/design-doc.md` | Markdown, 20–35 pages | All required sections; ≥ 5 diagrams; ≥ 3 reviewers |
| 3 | ADR series | `adr/0001..0010-*.md` | Markdown, ≥ 1 page each | 6–10 ADRs on one coherent domain |
| 4 | ADR index | `adr/INDEX.md` | Markdown | Dependency graph + worldview-connection note per ADR |
| 5 | Mentorship plan | `docs/mentorship-plan.md` | Markdown | ≥ 2 mentees with 12-month axes |
| 6 | Coaching artifacts | `coaching/session-NN-*.{md,mp4,mp3}` | Markdown + audio/video | ≥ 3 sessions with reflections |
| 7 | Mentorship reflection | `docs/mentorship-reflection.md` | Markdown, 5–10 pages | Honest; includes "what I do poorly" |
| 8 | Mentee feedback | `docs/mentee-feedback.md` | Markdown | Solicited + incorporated |
| 9 | Tech talk recording | `talks/tech-talk.mp4` or link in `talks/README.md` | Video, 30–45 min | Audio mandatory |
| 10 | Tech talk slides | `talks/slides.pdf` (and source if available) | PDF | Sparse; supporting |
| 11 | Speaker notes | `talks/speaker-notes.md` | Markdown | Such that another principal could give a close version |
| 12 | Talk abstract | `talks/abstract.md` | Markdown, 200–400 words | Conference-submission ready |
| 13 | Lightning version | `talks/lightning/` | Slides + notes, 5 min | Stretch artifact (counts under Dimension 4 level 4) |
| 14 | Conference proposal | `proposal/` | Markdown components | Title, abstract, description, audience, learning outcomes, bio, AV |
| 15 | Proposal cover letter | `proposal/cover-letter.md` | Markdown, 200–400 words | Addressed to program committee |
| 16 | Venue rationale | `docs/proposal-venue-rationale.md` | Markdown | Why this venue for this talk |
| 17 | Submission status | `docs/submission-status.md` | Markdown | Submitted? Accepted? Pending? Captured confirmation |
| 18 | Connective synthesis | `docs/connective-synthesis.md` | Markdown, 2–4 pages | Worldview → each artifact mapping + diagram |
| 19 | Peer review thread | `docs/peer-reviews.md` (or PR comments) | Markdown | ≥ 3 reviewers on design + ADRs; ≥ 1 on talk; ≥ 1 on proposal |
| 20 | Self-assessment | `docs/self-assessment.md` | Markdown | Per-dimension scores + rationale + weakest-dimension paragraph |
| 21 | Top-level README | `README.md` | Markdown | One paragraph per artifact; links to each |

---

## Repository Layout (Mandatory)

```
project-05-technical-leadership/
├── README.md
├── docs/
│   ├── worldview.md
│   ├── design-doc.md
│   ├── design-doc-outline.md
│   ├── mentorship-plan.md
│   ├── mentorship-reflection.md
│   ├── mentee-feedback.md
│   ├── proposal-venue-rationale.md
│   ├── submission-status.md
│   ├── connective-synthesis.md
│   ├── peer-reviews.md
│   └── self-assessment.md
├── adr/
│   ├── INDEX.md
│   ├── 0001-<title>.md
│   ├── 0002-<title>.md
│   ├── 0003-<title>.md
│   ├── 0004-<title>.md
│   ├── 0005-<title>.md
│   ├── 0006-<title>.md
│   └── (optional 0007–0010)
├── coaching/
│   ├── session-01-<topic>/
│   │   ├── session.md
│   │   ├── recording.mp4 (or .mp3 or .url)
│   │   └── reflection.md
│   ├── session-02-<topic>/
│   └── session-03-<topic>/
├── talks/
│   ├── README.md
│   ├── tech-talk.mp4 (or .url)
│   ├── slides.pdf
│   ├── slides-source.{key,pptx,sketch}     # optional
│   ├── speaker-notes.md
│   ├── abstract.md
│   └── lightning/
│       ├── slides.pdf
│       └── speaker-notes.md
└── proposal/
    ├── title.md
    ├── abstract.md
    ├── description.md
    ├── audience.md
    ├── learning-outcomes.md
    ├── bio.md
    ├── av-requirements.md
    └── cover-letter.md
```

---

## Naming Conventions

- **ADRs:** `NNNN-kebab-case-title.md`, sequential starting `0001`.
- **Coaching sessions:** `session-NN-<topic>/`, NN sequential starting `01`.
- **Mentees in plan:** use pseudonyms or initials if real; first paragraph notes which.
- **Diagrams:** Mermaid inline in Markdown wherever possible; if PNG, `docs/img/<name>.png`.

---

## Format Requirements

- All Markdown is GitHub-flavored.
- Diagrams in Mermaid where rendering matters; otherwise PNG with retina variant.
- Code blocks (where used in design doc) always have a language tag.
- No `.md` doc > 1500 lines (split if longer).
- Video: H.264 mp4 preferred for tech talk and coaching sessions; if hosted externally (Vimeo, internal Drive, Loom), `talks/README.md` and `coaching/session-NN-*/recording.url` provide stable links.
- Coaching recordings: if you cannot record real sessions due to consent issues, structured plans + reflections are acceptable substitutes; mark each as a reconstruction in `coaching/session-NN-*/session.md`.

---

## What You Will Be Asked at Review

A reviewer will sit with your repo for ~60 minutes and try to:

1. Read `docs/worldview.md` and quote your focus area + one falsifiable claim.
2. Read the executive summary of `docs/design-doc.md` and explain the doc's recommendation in one sentence.
3. Open one ADR at random and find a named alternative + a stated consequence.
4. Open `adr/INDEX.md` and follow the dependency graph.
5. Read `docs/mentorship-reflection.md` and assess whether it feels honest.
6. Watch 5 minutes of `talks/tech-talk.mp4` at random and quote the thesis.
7. Open `proposal/abstract.md` and tell what venue + audience it targets.
8. Read `docs/proposal-venue-rationale.md` and assess the fit.
9. Read `docs/connective-synthesis.md` and see how the artifacts connect.
10. Read `docs/self-assessment.md` and see whether you scored honestly.

If any of these fail, the corresponding dimension in `rubric.md` loses a level.

---

## Submission Checklist

Before declaring done:

- [ ] All 21 inventory items present at the documented paths
- [ ] `docs/worldview.md` 3–5 pages with ≥ 3 falsifiable claims + ≥ 2 non-claims + blind spots
- [ ] `docs/design-doc.md` 20–35 pages with all sections, ≥ 5 diagrams
- [ ] ≥ 3 reviewers' comments captured on design doc + ADRs
- [ ] 6–10 ADRs with full structure; INDEX with dependency graph
- [ ] ≥ 2 ADRs with trigger conditions; ≥ 1 ADR "what I'd do differently"
- [ ] `docs/mentorship-plan.md` for ≥ 2 mentees with 12-month axes
- [ ] ≥ 3 coaching artifacts in `coaching/` with reflections
- [ ] `docs/mentorship-reflection.md` 5–10 pages, includes "what I do poorly"
- [ ] Tech talk 30–45 min recorded; slides + speaker notes + abstract committed
- [ ] Conference proposal submitted (or submission-ready); venue rationale + cover letter present
- [ ] `docs/connective-synthesis.md` maps worldview to artifacts
- [ ] `docs/self-assessment.md` per-dimension scores + rationale + weakest-dimension paragraph
- [ ] Final commit tagged `capstone-final`
- [ ] At least 4 reviewers acknowledged in front-matter (technical peer, manager, junior engineer, venue insider where possible)
