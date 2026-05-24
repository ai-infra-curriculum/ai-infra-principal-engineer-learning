# Rubric — Project 05: Technical Leadership Capstone

Total: **100 points**. Minimum to pass: **70**. Portfolio-grade for principal interviews / promo packets: **85+**.

Scoring is on a 5-level scale per dimension. A dimension's contribution = (dimension level / 4) × dimension weight.

Levels:
- **0 — Missing or unacceptable**
- **1 — Below bar** (would not pass senior-engineer level)
- **2 — Meets senior-engineer bar** (works but doesn't reflect principal-level thinking)
- **3 — Meets principal bar** (defensible, complete, ready to put in a promo packet)
- **4 — Exceeds principal bar** (portfolio-grade; the artifact a promo committee or hiring committee would single out)

---

## Dimension 1 — Design Doc Craft & Depth (25 pts)

Is the design doc the kind of artifact a director, a security partner, and three staff engineers can review without major rewrites?

| Level | Evidence |
|-------|----------|
| 0 | No design doc, or fewer than 8 pages. |
| 1 | Document exists but reads as implementation notes; no executive summary; no failure model. |
| 2 | 10–20 pages; covers problem + solution; decisions section thin (no alternatives). |
| 3 | **20–35 pages** with all required sections (exec summary, problem, non-goals, stakeholders, architecture, decisions with alternatives, failure model, rollout, cost, security, open questions, appendix). Reviewed by ≥ 3 senior or staff engineers with comments visibly addressed. ≥ 5 diagrams, each captioned and referenced. |
| 4 | Publishable as an industry tech blog or conference paper. Decisions section names alternatives + consequences + trigger conditions for reversal. Failure model is a reference example. Voice is recognizably principal-level. |

**Sample evidence accepted:** `docs/design-doc.md`, reviewer comment threads, diagrams.

---

## Dimension 2 — ADR Series & Worldview Coherence (20 pts)

Do the ADRs form a defensible thesis? Could an outside reader derive your worldview from them?

| Level | Evidence |
|-------|----------|
| 0 | No ADRs, or fewer than 3. |
| 1 | 3–5 ADRs; no alternatives section; no INDEX. |
| 2 | 6+ ADRs but on a grab-bag of unrelated topics. Some have alternatives; trigger conditions absent. |
| 3 | **6–10 ADRs** on a single coherent domain. Each has context + decision + ≥ 2 named alternatives + consequences accepted. INDEX.md with dependency graph. ≥ 2 ADRs name trigger conditions for revisiting. ≥ 1 ADR captures a decision you'd now make differently. |
| 4 | The ADR series, read top to bottom, is a single defensible thesis about the domain. An outside reader could derive your worldview from the ADRs alone. No two ADRs implicitly contradict each other. The "what I'd do differently" ADR is concrete and shows calibration. |

**Sample evidence accepted:** `adr/`, `adr/INDEX.md`, dependency graph.

---

## Dimension 3 — Mentorship & Multiplier Evidence (20 pts)

Did you do real mentorship work, and can you reflect honestly on it?

| Level | Evidence |
|-------|----------|
| 0 | No mentorship work. |
| 1 | One mentee sketched; no coaching artifacts; no reflection. |
| 2 | Two mentees; coaching artifacts thin or generic; reflection avoids hard truths. |
| 3 | **≥ 2 mentees** with 12-month plans (growth axes, milestones, stretch goal, risks). **≥ 3 coaching artifacts** (recordings or detailed structured plans) each with what-the-coachee-brought, your-mode, your-reflection. **Reflection 5–10 pages**, honest, includes "what I do poorly" with evidence. Mentee feedback solicited and incorporated. |
| 4 | A mentee visibly grows during the project (promotion, stretch role, first talk, scope expansion). Reflection is unusually honest — names a specific bad habit and the work-in-progress to address it, in a way a peer would call brave. Coaching artifacts show genuine mode-switching (directive vs coaching vs delegating) and self-correction across sessions. |

**Sample evidence accepted:** `docs/mentorship-plan.md`, `coaching/`, `docs/mentorship-reflection.md`, mentee feedback notes.

---

## Dimension 4 — Tech Talk & External Communication (15 pts)

Can you land a thesis in front of an audience?

| Level | Evidence |
|-------|----------|
| 0 | No talk. |
| 1 | Talk exists but is < 20 min or reads off slides; thesis unclear. |
| 2 | Talk 20–40 min, recorded, comprehensible. Slides bullet-heavy. No "what we got wrong" slide. |
| 3 | Talk **30–45 min** recorded with clean audio. Thesis stated in first 90 seconds and sticky (quotable). Story arc (problem / tension / approach / evidence / surprise / what's next). Surprise slide present. Speaker notes in `talks/speaker-notes.md`. Slides sparse and supporting. |
| 4 | Talk is publishable internally or accepted externally. Voice is principal-grade — uses calibrated language, owns mistakes, lands a worldview claim. A lightning (5-min) version of the talk is also packaged. Audience members two weeks later can still quote the thesis. |

**Sample evidence accepted:** `talks/tech-talk.mp4`, `talks/slides.pdf`, `talks/speaker-notes.md`, `talks/abstract.md`, reviewer feedback from rehearsal.

---

## Dimension 5 — Conference Proposal (10 pts)

Did you pitch this work to the world (or a credible internal audience)?

| Level | Evidence |
|-------|----------|
| 0 | No proposal. |
| 1 | Proposal drafted but missing cover letter or venue rationale. |
| 2 | Proposal complete but venue scatter-shot or fit unclear. |
| 3 | Proposal **submitted** (or submission-ready) to one real venue. Components present: title, 200-word abstract, full description, audience, learning outcomes, bio, AV. Cover letter (200–400 words). Venue rationale doc explains the fit. |
| 4 | Proposal **accepted** (any external venue) or visibly strong internal commit. Hook is compelling and tailored to the venue. Bio is focused and credible. Outline shows clear time allocation. |

**Sample evidence accepted:** `proposal/`, `proposal/cover-letter.md`, `docs/proposal-venue-rationale.md`, `docs/submission-status.md`.

---

## Dimension 6 — Connective Synthesis (10 pts)

Does the portfolio feel like one engineer's coherent worldview, or a scatter of artifacts?

| Level | Evidence |
|-------|----------|
| 0 | No synthesis; artifacts are unrelated. |
| 1 | Synthesis present but vague ("these all relate to AI infrastructure"). |
| 2 | Synthesis maps some connections; voice across artifacts inconsistent. |
| 3 | `docs/connective-synthesis.md` 2–4 pages explicitly maps worldview → each artifact. Voice across artifacts is recognizably the same engineer. An outside reader given only worldview + synthesis can predict the broad shape of the design doc and ADR domain. |
| 4 | Portfolio feels like a coherent body of work. A peer who reads only 30 minutes of the portfolio could articulate the engineer's worldview and predict the engineer's positions on adjacent questions not covered. |

**Sample evidence accepted:** `docs/worldview.md`, `docs/connective-synthesis.md`, voice consistency across artifacts, peer-prediction note if validated.

---

## Scoring Worksheet

```
Dimension                                         Weight   Level (0–4)   Subtotal
Design doc craft & depth                           25         ___         ___ × 25/4 = ___
ADR series & worldview coherence                   20         ___         ___ × 20/4 = ___
Mentorship & multiplier evidence                   20         ___         ___ × 20/4 = ___
Tech talk & external communication                 15         ___         ___ × 15/4 = ___
Conference proposal                                10         ___         ___ × 10/4 = ___
Connective synthesis                               10         ___         ___ × 10/4 = ___
                                                                          ─────────────
                                                              TOTAL:      ___ / 100
```

---

## Calibration Notes for Reviewers

- A "Level 3" portfolio is **promo-packet ready** at most companies. It should not feel like a learning exercise.
- A "Level 4" portfolio is the artifact a promo committee or hiring committee would single out. It probably already led to a promotion or a job offer.
- Leadership portfolios fail most often on **scatter** (no connective tissue) and **falsely confident reflection** (mentorship reflection that avoids self-criticism). Score Dimensions 3 and 6 strictly.
- A common error: scoring "Level 4" because the design doc is long. Length is not depth. A 35-page doc with no decisions section is Level 2.
- A common error in the other direction: penalizing a "smaller" worldview that is narrow and defensible. Narrow + defensible beats broad + vague every time at the principal level.
- The "what I'd do differently" ADR and the "what I do poorly" reflection are signals of maturity. Their **absence** is graded down; their **presence** should be graded up.
- An LLM-authored worldview is detectable and is an automatic Level 2 cap on Dimension 6 (connective synthesis cannot be coherent without an authentic voice).

---

## Self-Assessment Before Submission

Before handing in for review, score yourself. If your honest self-score is below 70 in any single dimension, fix that dimension before submitting. The two dimensions most often under-scored on the first pass are **Mentorship Reflection** (because honesty is uncomfortable) and **Connective Synthesis** (because integration is the hardest skill in the portfolio). Be honest with yourself first.
