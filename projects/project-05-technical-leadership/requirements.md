# Requirements — Project 05: Technical Leadership Capstone

This document defines what the portfolio must contain, the constraints you accept, the assumptions you may make, and the acceptance criteria a reviewer will check against.

Requirements use **MoSCoW** prioritization: **M**ust, **S**hould, **C**ould, **W**on't.

---

## 1. Functional Requirements

### 1.1 Worldview Statement (Must)

- **M-FR-1**: A `docs/worldview.md` (3–5 pages) names your principal-level point of view on a focused area of AI infrastructure. Examples: "the right LLM-serving stack for cost-constrained orgs through 2027"; "convergence path for training stacks in mixed-cluster orgs"; "the next 24 months of inference accelerators and what to build around them"; "how MoE serving changes capacity planning"; "the production case for agentic ML pipelines and where it breaks."
- **M-FR-2**: The worldview states at least 3 falsifiable claims — predictions or principles that future evidence could disprove.
- **M-FR-3**: The worldview names at least 2 things you are explicitly **not** claiming and why.
- **S-FR-4**: Includes a "what I was wrong about a year ago" paragraph.

### 1.2 Design Doc (Must)

- **M-FR-5**: A `docs/design-doc.md` of **20–35 pages** on a substantial AI-infra problem that aligns with your worldview. Examples:
  - A reference architecture for cost-constrained LLM serving at 1K–10K QPS
  - A 24-month roadmap for training-stack convergence in a multi-team org
  - A design for an inference platform supporting both transformer + state-space architectures
  - A design for an evaluation infrastructure that catches silent regressions across model releases
  - A design for an agentic ML pipeline platform with first-class observability and rollback
- **M-FR-6**: The doc has at minimum these sections:
  1. Executive summary (1–2 pages, survivable in 90 sec)
  2. Problem statement and goals
  3. Non-goals
  4. Stakeholders and their decision rights
  5. Proposed solution (high-level)
  6. Architecture (components, data flow, sequence diagrams)
  7. Major decisions (each with alternatives + consequences + trigger to reverse)
  8. Failure model (RTO/RPO if operational; correctness model if research)
  9. Rollout / migration plan
  10. Cost model
  11. Security + compliance considerations
  12. Open questions
  13. Appendix
- **M-FR-7**: Reviewed by ≥ 3 senior or staff engineers; comments thread captured (PR comments, doc comments, or `docs/design-doc-review.md`).
- **M-FR-8**: At minimum 5 diagrams (Mermaid or PNG).
- **S-FR-9**: A "what I'd do differently in v2" section grounded in the review feedback.

### 1.3 ADR Series (Must)

- **M-FR-10**: **6–10 ADRs** on a single coherent domain — not a grab-bag. The domain must connect to the design doc and/or worldview.
- **M-FR-11**: Each ADR contains: context, decision, alternatives considered (≥ 2 named), consequences accepted, status, date, deciders.
- **M-FR-12**: At least **2 ADRs** explicitly name **trigger conditions for revisiting** the decision (observable events that would change the call).
- **M-FR-13**: At least **1 ADR** captures a decision you would now make differently, with the reasoning.
- **S-FR-14**: The ADR series taken together is internally consistent — no two ADRs implicitly contradict each other.
- **C-FR-15**: A `adr/INDEX.md` rationalizes the series and shows the dependency graph between ADRs.

### 1.4 Mentorship Plan & Practice (Must)

- **M-FR-16**: A `docs/mentorship-plan.md` covering **≥ 2 mentees** (real or, if you cannot use real names, realistic personas based on real engineers you've worked with). Per mentee:
  - Current state (level, strengths, growth edges)
  - 12-month plan with growth axes and milestones
  - Stretch goal (one thing that, if achieved, would be a step-change)
  - Risks / what could go wrong
  - Cadence of 1:1s and project pairing
- **M-FR-17**: At least **3 coaching artifacts** — recordings, transcripts, or detailed structured plans of coaching conversations. Each is accompanied by:
  - What the coachee brought
  - Your coaching approach (and a note on what mode you were in: directive, supportive, coaching, delegating)
  - Outcome
  - What you'd do differently
- **M-FR-18**: A `docs/mentorship-reflection.md` (5–10 pages): honest self-assessment of your multiplier behavior. Includes:
  - What you do well (with evidence)
  - What you do poorly (with evidence)
  - What you are actively working on
  - Where you fall back to "doing it yourself" instead of growing others
  - What you'd want a reviewer to push back on
- **S-FR-19**: Feedback solicited from at least one mentee (real or simulated retrospective) and incorporated into the reflection.

### 1.5 Tech Talk (Must)

- **M-FR-20**: A **30–45 minute recorded talk** with audio + slides. Visible face optional; clear audio mandatory.
- **M-FR-21**: The thesis is stated in the **first 90 seconds**. A viewer who watches only the opening can quote the thesis.
- **M-FR-22**: The talk has a story arc, not a feature tour: problem → tension → resolution → what surprised us → what's next.
- **M-FR-23**: At least one slide explicitly addresses something you got wrong — projects with no "what we got wrong" slide are not principal-grade.
- **M-FR-24**: Speaker notes are written out (`talks/speaker-notes.md`) such that someone else could give a close version of the talk.
- **S-FR-25**: A written **abstract** (200–400 words) suitable for submission to a venue (`talks/abstract.md`).
- **S-FR-26**: A 5-minute "lightning" version of the talk (slides + speaker notes) for opportunistic audiences.

### 1.6 Conference Proposal (Must)

- **M-FR-27**: A **submission-ready proposal** for a real venue. Real venues include: MLSys, NeurIPS workshops, KubeCon, MLOps World, RICON, GTC, USENIX OpML, ICML workshops, internal company tech all-hands.
- **M-FR-28**: Proposal contains: title, 200-word abstract, full description, target audience, learning outcomes, speaker bio, prior speaking experience (if any), AV requirements.
- **M-FR-29**: Submission targeted to a **specific** venue with a written rationale (`docs/proposal-venue-rationale.md`).
- **M-FR-30**: Cover letter (200–400 words) to the program committee.
- **S-FR-31**: Proposal actually submitted; submission confirmation captured in `docs/submission-status.md`.
- **C-FR-32**: Proposal **accepted** (any external venue) — stretch.

### 1.7 Connective Synthesis (Must)

- **M-FR-33**: A `docs/connective-synthesis.md` (2–4 pages) explicitly maps how the design doc, the ADR series, the talk, and the conference proposal connect to the worldview.
- **M-FR-34**: An outside reader, given only the worldview + synthesis, can predict the broad shape of the design doc and the ADR domain. (You should validate this with a peer.)
- **S-FR-35**: A diagram (Mermaid) showing the connective tissue between artifacts.

### 1.8 Peer Review Capture (Must)

- **M-FR-36**: At minimum **3 reviewers** on the design doc and ADRs. Comments captured (PR thread, doc comments, or written summary in `docs/peer-reviews.md`).
- **M-FR-37**: At minimum **1 reviewer** on the talk (rehearsal + feedback).
- **M-FR-38**: At minimum **1 reviewer** on the conference proposal (program committee insight if possible).
- **S-FR-39**: Reviewers represent at least 2 perspectives outside your immediate team (cross-team staff engineer, security or compliance partner, manager, junior engineer).

### 1.9 Self-Assessment (Must)

- **M-FR-40**: `docs/self-assessment.md` scoring yourself against the rubric with per-dimension rationale.
- **M-FR-41**: The dimension you'd lose the most points on, with a paragraph explaining why and what you'd do differently with more time.

---

## 2. Non-Functional Requirements

### 2.1 Craft (Must)

- **M-NFR-1**: All writing is in clean, principal-grade prose. No filler ("This document will discuss…"). No bureaucratic hedging.
- **M-NFR-2**: Long documents have a skimmable structure: tables of contents, callouts, summary boxes, diagrams in the right place.
- **M-NFR-3**: Diagrams have captions and labels; orphan diagrams ("Figure 1" with no narrative) are not acceptable.

### 2.2 Honesty (Must)

- **M-NFR-4**: Every claim defensible. No marketing tone. No survivorship-bias framing of past projects.
- **M-NFR-5**: Mentorship reflection passes a "would I let this be quoted to my mentee?" check.
- **M-NFR-6**: Worldview includes falsifiable claims and named blind spots.

### 2.3 Consistency (Should)

- **S-NFR-7**: Voice across artifacts is recognizable as the same engineer.
- **S-NFR-8**: Decisions in ADRs don't contradict positions taken in the design doc.

### 2.4 Reusability (Should)

- **S-NFR-9**: ADRs are templates that could be applied to a future project in the same domain.
- **S-NFR-10**: Talk slides are reusable for lightning / longer formats.

---

## 3. Constraints

- **C-1**: The capstone is one **integrated** portfolio, not four loosely related items. Scatter is graded down.
- **C-2**: The worldview must be **focused**. "I have opinions about all of AI infra" is not a worldview; it is a brochure.
- **C-3**: Mentorship work must be **real or realistically detailed**. Sketched personas are acceptable; vague gestures at "I mentor people" are not.
- **C-4**: The conference proposal must target a **real venue**. An internal company tech all-hands is acceptable if it has a real selection process.
- **C-5**: Honest reflection is required. A self-assessment that says "I am strong everywhere" loses points; the rubric values calibrated humility.
- **C-6**: No artifact may be written by an LLM end-to-end. LLM-assisted drafting is fine; LLM-authored worldview is not. You sign your name to claims; you must defend them.
- **C-7**: Real names of mentees / colleagues only with their consent. Pseudonyms are fine; the underlying details must be real.

---

## 4. Assumptions

You may assume the following without further justification. If absent, document the substitute.

- **A-1**: You have access to at least one practice audience (peer-review group, mentees, an internal forum) for the talk.
- **A-2**: You have at least 2 real (or realistically-modeled) mentees.
- **A-3**: You have at least one venue (internal or external) to submit the proposal to.
- **A-4**: You have a recording setup (Loom, Zoom, OBS, even a phone with a decent mic).
- **A-5**: You have permission to share your written artifacts with reviewers.

---

## 5. Out of Scope (Won't)

To keep this project at 80 hours and not 200:

- **W-1**: You will not solve all of AI infrastructure. One worldview, one design, one ADR domain.
- **W-2**: You will not build software for this capstone (other than what supports a talk demo, if any). The artifacts are written and verbal.
- **W-3**: You will not author research papers. The conference proposal is enough; full submission to a peer-reviewed venue is out of scope.
- **W-4**: You will not run formal mentorship circles or set up new programs. The plans are for individual mentees.
- **W-5**: You will not collect 360-degree feedback or run org-wide surveys. Reflection is self + small peer set.
- **W-6**: You will not lobby for your own promotion. The portfolio is the artifact; promotion is downstream.

---

## 6. Acceptance Criteria

A reviewer should be able to mechanically check these.

### A. Worldview
1. `docs/worldview.md` exists, 3–5 pages, with ≥ 3 falsifiable claims and ≥ 2 explicit non-claims.

### B. Design doc
2. `docs/design-doc.md` exists, 20–35 pages, with all required sections.
3. ≥ 3 reviewers' comments captured.
4. ≥ 5 diagrams included.

### C. ADR series
5. 6–10 ADRs in `adr/` with full structure.
6. ≥ 2 ADRs name trigger conditions for revisiting.
7. ≥ 1 ADR captures a decision you'd now make differently.
8. Series is on a coherent domain that connects to the design and worldview.

### D. Mentorship
9. `docs/mentorship-plan.md` covers ≥ 2 mentees with structured plans.
10. ≥ 3 coaching artifacts in `coaching/` with reflections.
11. `docs/mentorship-reflection.md` 5–10 pages, honest, includes "what I do poorly."

### E. Tech talk
12. `talks/tech-talk.mp4` (or link) 30–45 min.
13. Thesis stated in first 90 seconds.
14. "What I got wrong" slide present.
15. Speaker notes in `talks/speaker-notes.md`.

### F. Conference proposal
16. `proposal/` directory contains title, abstract, full description, audience, learning outcomes, bio, AV.
17. `docs/proposal-venue-rationale.md` names the venue and justifies the fit.
18. Cover letter in `proposal/cover-letter.md`.
19. Submission status in `docs/submission-status.md`.

### G. Connective synthesis
20. `docs/connective-synthesis.md` maps artifacts to worldview.
21. An outside reader can predict the design doc shape from the worldview.

### H. Peer review
22. ≥ 3 reviewers' comments on the design doc.
23. ≥ 1 reviewer on the talk; ≥ 1 reviewer on the proposal.

### I. Self-assessment
24. `docs/self-assessment.md` with per-dimension scores + rationale.

### J. Quality bar
25. Writing is principal-grade (no filler; clean structure).
26. No `.md` doc > 1500 lines (split if longer).
27. All diagrams labeled and captioned.

---

## 7. Dependencies on Other People (For the Plan, Not the Code)

For this capstone you depend on:

- **Reviewers** — at least 3 for the design doc and ADRs
- **Mentees** — at least 2, real or realistically modeled
- **A talk audience** — at least one practice run with feedback
- **A venue** — internal or external, with a real selection process
- **A peer reviewer for the proposal** — ideally someone who has reviewed for the target venue
- **A manager or principal peer** — to push back on the worldview and the mentorship reflection

Naming real people is not required; naming roles + commitments is.

---

## 8. Glossary

- **Worldview** — A focused, defensible technical point of view; not a brochure
- **ADR** — Architecture Decision Record
- **Coaching mode** — asking questions vs. giving answers; helping the coachee think
- **Multiplier behavior** — actions that make other engineers more effective (vs. doing the work yourself)
- **Trigger condition** — observable event that would cause you to reverse or revisit a decision
- **Sticky thesis** — a one-sentence claim a talk's audience can quote correctly two weeks later
- **Connective tissue** — the through-line that makes a portfolio feel like one engineer's work
