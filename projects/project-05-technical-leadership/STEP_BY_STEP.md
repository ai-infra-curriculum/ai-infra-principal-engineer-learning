# Step-by-Step Build Guide — Project 05: Technical Leadership Capstone

This is an **80-hour, 4-week build plan** at full-time pace, or ~8 weeks at 10 h/week. Phases are sequential — the worldview *must* be written first because it constrains every other artifact.

Each phase has: **goal**, **day-level breakdown**, **validation gate**, and **gotchas** drawn from real promo-packet scars.

---

## Phase 0 — Pre-Work (3 h, before Week 1)

### Goal
Pick the focus area, identify mentees and reviewers, scout the conference venue.

### Tasks
1. **Draft your focus area** in one sentence. Write 3 candidates; pick one. Test it against: "Could I defend this for 60 minutes in front of skeptical peers?"
2. **Identify reviewers.** You'll need ≥ 3 for the design doc, ≥ 1 for the talk, ≥ 1 for the proposal. Email them now; calendars fill.
3. **Identify mentees.** ≥ 2 real engineers (preferred) or detailed personas based on real engineers. Get rough consent if real.
4. **Scout the conference venue.** Look up CFP timelines for MLSys, NeurIPS workshops, KubeCon, MLOps World, RICON, GTC, USENIX OpML. Pick **one** (or an internal company tech all-hands with a real selection process). Note the deadline.
5. **Set up a single repo** with the directory skeleton from the deliverables doc.

### Gate
You can articulate, in 90 seconds: focus area, who your reviewers are, who your mentees are, which venue you're targeting, and why.

### Gotchas
- "I'll figure out who the reviewers are later" is the most common reason this project derails in week 4. Lock in calendar commitments now.
- "I have lots of mentees" is rarely true with the kind of depth this project requires. Two is enough; two is better than three sketched.
- Conference deadlines vary widely. Some are 6 months out; some are 6 weeks. Pick a venue with a deadline that fits your timeline.

---

## Week 1 — Worldview, Topic Selection, Design Doc Draft 1, ADR Domain (16 h)

### Day 1 (4 h) — Worldview statement

Write `docs/worldview.md`. 3–5 pages. Sections:
1. **Focus area** — one sentence, plus a paragraph explaining why this *and not* the obvious adjacent areas
2. **Three falsifiable claims** — each with what evidence would prove it wrong
3. **What I'm not claiming** — ≥ 2 explicit non-claims with reasoning
4. **What I was wrong about a year ago** — concrete, not generic
5. **Blind spots** — hardware, orgs, domains you don't have direct experience with
6. **Implications** — what your worldview implies for org decisions over the next 24 months

Then hand it to one peer (the smartest one you can grab). Ask: "Would you read 35 pages from someone with this worldview?" Iterate before continuing.

### Day 2 (4 h) — Design doc topic + outline + ADR domain

Pick the design doc topic. It must align with the worldview. Examples per worldview shape:
- Worldview about LLM serving stacks → design doc: reference architecture for the proposed stack at production scale
- Worldview about training-stack convergence → design doc: framework + job-spec layer to enable convergence
- Worldview about agentic ML → design doc: agentic-on-deterministic platform with rollback + observability

Pick the ADR domain. Same constraint: must align with worldview and design.

Write a 1-page outline for each. Commit `docs/design-doc-outline.md` and `adr/INDEX.md` (skeleton).

### Day 3 (4 h) — Design doc draft 1 (executive summary + problem + architecture)

Write the executive summary, problem statement, non-goals, stakeholders, high-level proposed solution, and the architecture section. Aim for 8–12 pages.

The executive summary is the most important page in the doc. Spend 90 minutes on it. Test: a non-engineer reads it in 90 seconds and can tell another person what the doc is about.

### Day 4 (2 h) — ADR stubs

Write 6 ADR stubs (`adr/0001..0006-*.md`). Each: title + 2-sentence context + status `proposed`. Order them per the dependency graph in `adr/INDEX.md`.

Verify the ADRs as a set tell a coherent story — if you removed one, would the worldview be incompletely operationalized? If not, the ADR is filler.

### Day 5 (2 h) — Peer review of worldview + design draft 1

Send worldview + design doc draft 1 to reviewers. Ask for **structural** feedback now (is the spine right?) and defer line-edits to later.

### Validation gate
- [ ] `docs/worldview.md` 3–5 pages with falsifiable claims + non-claims + blind spots
- [ ] Design doc outline committed
- [ ] Design doc draft 1 with executive summary + architecture (≥ 8 pages)
- [ ] 6 ADR stubs with dependency graph in INDEX
- [ ] At least one external reviewer has commented on the worldview + draft

### Gotchas
- A worldview that takes 20 pages is not a worldview; it's a textbook. Stay focused.
- A design doc whose executive summary doesn't survive a 90-second read is not principal-grade, no matter how good page 14 is.
- ADR stubs without a dependency graph drift toward grab-bag. Force yourself to draw INDEX.md before writing the bodies.

---

## Week 2 — Design Doc Completion + ADR Authoring (22 h)

### Day 6 (5 h) — Design doc deepen (decisions section)

Write the **major decisions** section. Each decision gets its own H3 sub-section:
- The decision (one sentence)
- Alternatives considered (≥ 2 named, with why-not)
- Consequences accepted (operational, organizational, technical debt)
- Trigger condition for revisiting (or "this is permanent and here's why")

3–5 major decisions per design doc is typical. More than that and you're decision-listing, not decision-making.

### Day 7 (5 h) — Design doc: failure model + rollout + cost

- **Failure model**: RTO/RPO table for operational designs; correctness model for research-flavored designs. Be explicit.
- **Rollout / migration plan**: phases, gates, rollback per phase.
- **Cost model**: separated assumptions + outputs; defensible at the level you'd take to FinOps.

### Day 8 (4 h) — Design doc: security, open questions, appendix; first round of edits

- Security + compliance section: identify the security partner you'd review with; capture the asks.
- Open questions: name 3–5. A design doc with no open questions is dishonest.
- Appendix: stakeholder interview notes, benchmark numbers, anything supporting.
- Pass the doc end-to-end. Edit for prose. Remove filler. Tighten captions.

### Day 9 (4 h) — ADRs 0001–0003

Polish the first three ADRs to full structure:
- Context
- Decision
- Alternatives considered (≥ 2 named)
- Consequences accepted
- Status + date + deciders

Pick the three that are most load-bearing for the design doc — those are the ADRs that, if a reader read them, would understand the design's spine.

### Day 10 (4 h) — ADRs 0004–0006 + INDEX

Finish the remaining ADRs. Update INDEX.md with dependency graph, summary of each, and the worldview-connection note per ADR.

At least 1 ADR captures a decision you'd now make differently — with the trigger condition and the alternative you'd consider.
At least 2 ADRs name trigger conditions for revisiting.

### Validation gate
- [ ] Design doc 20–35 pages with all required sections
- [ ] At least 5 diagrams included
- [ ] 6+ ADRs with full structure
- [ ] INDEX.md with dependency graph
- [ ] ≥ 1 ADR with "what I'd do differently"
- [ ] ≥ 2 ADRs with trigger conditions
- [ ] Reviewer comments on draft 1 incorporated or explicitly punted

### Gotchas
- The decisions section is where most design docs go shallow. Force yourself to write the alternatives, not just the choices.
- Cost models that mix assumptions and outputs are not defensible. Two tabs / two sections; one for assumptions, one for outputs.
- ADRs that say "we chose X because X is the best fit" with no alternatives read as junior. Always name what you rejected.

---

## Week 3 — Mentorship + Talk Preparation (22 h)

### Day 11 (5 h) — Mentorship plans

For each mentee (≥ 2), write the plan per the template in `architecture.md` §3.3:
- Current state
- 12-month growth axes
- Milestones at 3 / 6 / 9 / 12 months per axis
- Stretch goal
- Risks
- Cadence

If using real mentees, validate the plan with them; capture their feedback. If using personas, base each on a specific real engineer (no fictional composites).

### Day 12 (5 h) — Coaching sessions

Run (or document) ≥ 3 coaching sessions. For each, capture:
- What the coachee brought
- The mode you were in (directive / supportive / coaching / delegating)
- The questions you asked
- What was decided
- Outcome
- What you'd do differently

Record where possible (audio). Where not, write up a structured plan and a reflection.

If you can't run real sessions in the time window, do detailed walkthroughs of past sessions — but mark them as reconstructions.

### Day 13 (4 h) — Mentorship reflection

Write `docs/mentorship-reflection.md` (5–10 pages). Honest. Includes:
- What you do well (with evidence)
- What you do poorly (with evidence)
- Where you default to "doing it yourself"
- What you're actively working on
- Pushback you'd welcome

Solicit feedback from at least one mentee (real or via retro) and incorporate it.

This document is graded heavily on **honesty**. A reflection that says "I'm strong everywhere" loses points. A reflection that names a specific bad habit, with the work-in-progress to address it, gains points.

### Day 14 (4 h) — Talk outline + slides v1

Outline:
1. Opening — thesis in first 90 sec
2. Problem + tension
3. Approach — 1–2 core ideas
4. Evidence — demos, numbers, stories
5. Surprise — what we got wrong
6. What's next
7. Q&A

Build slides v1. Keep them sparse — your face / voice / story carries the talk, not the slides.

### Day 15 (4 h) — Talk rehearsal + recording

- Practice once in front of a peer. Get feedback on pace, thesis clarity, the surprise slide.
- Rebuild slides v2 incorporating feedback.
- Record the full talk. Watch the recording. Note the moments you'd cut.
- Re-record once if the first pass had a major problem.

### Validation gate
- [ ] Mentorship plans for ≥ 2 mentees committed
- [ ] ≥ 3 coaching artifacts in `coaching/` with reflections
- [ ] `docs/mentorship-reflection.md` 5–10 pages, includes "what I do poorly"
- [ ] Talk recorded (30–45 min)
- [ ] Slides + speaker notes committed
- [ ] At least one peer has watched + commented on the talk

### Gotchas
- The reflection that says "I always coach well" is the reflection no one believes. Be specific about what you don't do well.
- Recording yourself is uncomfortable. Do it anyway; it's the only way to improve.
- Slides with five bullet points each will sink your talk. Use one image or three words per slide and let your voice carry.

---

## Week 4 — Conference Proposal, Connective Synthesis, Peer Reviews, Polish, Self-Assessment (20 h)

### Day 16 (4 h) — Conference proposal

Write the proposal. Components:
- Title (≤ 80 chars, descriptive + hook)
- 200-word abstract (the program version)
- Full description (audience, learning outcomes, outline)
- Speaker bio (focused, not a résumé)
- Prior speaking experience
- AV requirements
- Cover letter (200–400 words to the program committee)

Pick **one** real venue. Write `docs/proposal-venue-rationale.md` explaining the fit.

### Day 17 (3 h) — Submit (or finalize submission-ready package)

Submit the proposal if the venue's CFP is open. Capture submission confirmation in `docs/submission-status.md`. If the CFP isn't open yet, the proposal must be 100 % submission-ready — a peer who didn't write it could submit it on your behalf.

### Day 18 (5 h) — Connective synthesis

Write `docs/connective-synthesis.md` (2–4 pages). Map the worldview to each artifact. Include a diagram (Mermaid) showing the connections.

Test: hand the worldview + synthesis to a peer who hasn't read the rest of the portfolio. Ask them to predict what the design doc and ADRs are about. If they can't, the synthesis is too vague.

### Day 19 (4 h) — Peer review round 2

Send the full portfolio to your reviewers. Ask for the specific kinds of feedback:
- **Technical peer**: is the design defensible? Are the ADRs missing alternatives?
- **Manager**: does the mentorship reflection feel honest? Where does the worldview overreach?
- **Junior engineer**: does the talk land? Does the thesis stick?
- **Venue program-committee insider (if available)**: would this proposal land?

Capture comments. Address or explicitly punt with rationale.

### Day 20 (2 h) — Polish + self-assessment

- Repo hygiene: file sizes, formatting, broken links.
- README at the top level: one paragraph per artifact; links to each.
- Self-assessment in `docs/self-assessment.md`: per-rubric scores with rationale.
- Identify your weakest dimension and write a paragraph defending the trade-off.

### Day 21 (2 h) — Submission

- Final commit with tag `capstone-final`.
- Submission email or PR to your reviewers / promo committee / hiring manager.
- Update `docs/submission-status.md` with the venue submission status if any.

### Validation gate
- [ ] Conference proposal submitted (or 100 % submission-ready) with venue rationale + cover letter
- [ ] `docs/connective-synthesis.md` maps worldview to artifacts
- [ ] Peer review round 2 captured
- [ ] Self-assessment committed with per-dimension scores
- [ ] README polished; all artifact links work
- [ ] Final tag committed

---

## Final Checklist Before Submitting

Tick every box. Each maps to an acceptance criterion in `requirements.md`.

- [ ] `docs/worldview.md` 3–5 pages with ≥ 3 falsifiable claims + ≥ 2 non-claims + blind spots
- [ ] `docs/design-doc.md` 20–35 pages, all sections, ≥ 5 diagrams, ≥ 3 reviewers
- [ ] 6–10 ADRs with full structure; INDEX with dependency graph; ≥ 2 with trigger conditions; ≥ 1 "what I'd do differently"
- [ ] `docs/mentorship-plan.md` for ≥ 2 mentees with 12-month axes
- [ ] ≥ 3 coaching artifacts in `coaching/` with reflections
- [ ] `docs/mentorship-reflection.md` 5–10 pages, honest, includes "what I do poorly"
- [ ] Tech talk 30–45 min recorded; slides; speaker notes; abstract
- [ ] Conference proposal submitted (or submission-ready) with venue rationale + cover letter
- [ ] `docs/connective-synthesis.md` maps worldview to artifacts
- [ ] Peer reviews captured (≥ 3 on design doc + ADRs; ≥ 1 on talk; ≥ 1 on proposal)
- [ ] `docs/self-assessment.md` per-dimension scores + rationale
- [ ] Voice consistent across all artifacts
- [ ] No `.md` doc > 1500 lines

---

## Common Failure Modes — Read Before You Start

1. **No worldview.** Four artifacts on four unrelated topics. Reviewer reads it as "competent senior engineer", not "principal."
2. **Worldview too broad.** "I have opinions about everything in AI infra." That's a brochure, not a worldview.
3. **Design doc with no decisions.** A 30-page architecture description without explicit trade-offs. Reads as documentation, not design.
4. **ADRs that are decisions, not defenses.** No alternatives, no consequences. Reads as a changelog.
5. **Mentorship work that's a checkbox.** "I had 1:1s with two engineers." No evidence of growth, no reflection, no honesty about your own gaps.
6. **Reflection that's falsely modest or falsely confident.** Both signal a lack of self-awareness; both lose points.
7. **Talk that's a tour, not a thesis.** "Here's all the things we did." Forgettable. Sticky talks have one sentence the audience quotes later.
8. **Conference proposal scatter-shot.** Submitted to 5 venues with no rationale for any. Program committees can tell.
9. **No connective synthesis.** Reviewer can't tell whether the four artifacts are from the same engineer. Portfolio reads as quilt.
10. **LLM-authored worldview.** The wording is generic, the claims are unfalsifiable, the voice is missing. Cardinal sin for this project.

---

## What "Good" Looks Like at the End

A reviewer with 1 hour should be able to:
1. Read `docs/worldview.md` and quote your focus area + one falsifiable claim.
2. Read the executive summary of the design doc and tell another person what the doc is about.
3. Open one ADR and find an alternative that was considered and why it was rejected.
4. Read the mentorship reflection and believe it's honest.
5. Watch 5 minutes of the talk at random and quote the thesis.
6. Open the conference proposal and tell what venue it targets and why.
7. Read `docs/connective-synthesis.md` and see how the artifacts fit together.

If a reviewer can do all 7 — you've passed. If they would put this artifact on a promo packet — you've hit 85+.
