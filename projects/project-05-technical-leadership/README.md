# Project 05: Technical Leadership Capstone

> **Duration:** 80 hours (3-4 weeks full-time, 8 weeks part-time)
> **Scope:** Portfolio — synthesis of design, decision-making, mentorship, and external communication
> **Difficulty:** Principal / Expert
> **Track:** Individual Contributor (IC), Principal Engineer
> **Project Type:** Tier 4 — Leadership portfolio; the artifact a promotion committee reads

---

## Overview

The four prior projects in this curriculum are about *building* — distributed training platforms, integration layers, performance wins, innovation POCs. This capstone is about *leading* without managing. It is the artifact a promotion committee, a hiring manager, or a future-you will open in three years and use to evaluate whether you actually are a principal engineer or whether you're a senior engineer with a longer title.

Your charter is to produce a **portfolio** that demonstrates the four load-bearing skills of principal-level technical leadership:

1. **Design** — a single ambitious technical design doc on a real (or realistic) problem in AI infrastructure, of the kind that takes a wall and a week to write
2. **Decision-making at scale** — a curated ADR series across a non-trivial domain, defending consequential bets and naming what would change your mind
3. **Mentorship & multiplier behavior** — a mentorship plan, recorded coaching sessions, and a written reflection on how you grow the next layer of engineers
4. **External communication** — a tech talk that lands for a wide audience, and a conference / community proposal pitched to a real venue

This is not "do four small projects." It is one integrated portfolio. The design doc, the ADRs, the mentorship work, the talk, and the proposal must connect — they should look like the output of one engineer with a coherent technical worldview, not a portfolio quilt.

Done well, this is the artifact you point at when you say "this is what principal looks like for me." Done poorly, it is four disconnected pieces that show effort but not depth.

---

## Why This Project Matters

Promotion and credibility at the principal level are not earned by writing code. They are earned by:

1. **Producing artifacts that outlive the engineer.** Design docs that frame how the org thinks. ADRs that prevent the next person from rediscovering the same trade-off. Talks that change what 200 engineers know.
2. **Growing the next layer.** A principal engineer who doesn't visibly multiply other engineers is a senior engineer who finished their own work faster than peers.
3. **Operating as a voice the org listens to.** External talks and conference appearances build the credibility that lets your internal proposals land.
4. **Demonstrating taste.** Anyone can pick an interesting problem; principals pick the *right* interesting problem and defend the choice with a worldview.

The capstone is structured around the principle: **if your principal-level work isn't legible to someone who wasn't there, it didn't happen.** Every artifact must stand on its own.

---

## Learning Outcomes

After completing this project, you will be able to:

### Design
1. **Write a 20–35 page technical design doc** on a substantial AI-infra problem — one a director, a security partner, and three staff engineers can review without major rewrites
2. **Defend the load-bearing decisions** with explicit alternatives, consequences, and named conditions under which you'd reverse course
3. **Structure a long-form technical document** for skimmability — executive summary that survives a 90-second read, sections deep enough for line-by-line review

### Decision-making
4. **Author a coherent ADR series (6–10 ADRs)** on a single non-trivial domain (e.g., LLM serving stack choices for the next 24 months; training stack convergence; inference cost roadmap; data infra for the next-gen RAG; agentic platform foundations)
5. **Calibrate** — distinguish reversible from irreversible decisions; document trigger conditions for revisiting

### Mentorship
6. **Author a 12-month mentorship plan** for ≥ 2 mentees with concrete growth axes, milestones, and stretch goals
7. **Record ≥ 3 coaching sessions** (or write up structured coaching plans) demonstrating coaching mode, not telling mode
8. **Reflect honestly** on your multiplier behavior — what you do well, what you don't, what you're working on

### External communication
9. **Deliver a 30–45 min tech talk** suitable for an external venue — recorded, well-structured, with a sticky thesis
10. **Submit a conference proposal** to a real venue (MLSys, NeurIPS workshop, MLOps World, KubeCon, GTC, RICON, internal company-wide tech all-hands)

### Integration
11. **Demonstrate a coherent technical worldview** — the design, ADRs, talk, and proposal all reflect the same underlying perspective on where AI infra is going

---

## Key Questions This Project Answers

You must defend a clear answer to each. They appear in the rubric.

1. **What is your principal-level focus?** In one sentence: which technical area of AI infrastructure do you have a defensible point of view on, and why are you the right person to have it?
2. **What is the design doc about, and why is it the right one?** Out of many possible designs, why this one — what does it teach about how you frame problems?
3. **What is the ADR domain, and what is the consistent worldview across the ADRs?** A reader should be able to derive your principles from the ADRs, not just the decisions.
4. **Who are you mentoring, and what are you optimizing them for?** A principal who can't articulate this is a principal in title only.
5. **What's the talk's thesis?** One sentence. If you need a paragraph, the talk isn't ready.
6. **What's the conference proposal's hook?** Why would a program committee accept this over the other 200 submissions?
7. **What's the connective tissue?** A reviewer should be able to read the design, two ADRs, watch 10 min of talk, and recognize the same engineer thinking through all four.

---

## Prerequisites

### Required experience
- **8+ years** total engineering, **3+ years** at staff scope (formal or informal)
- Has written at least one design doc that 5+ engineers reviewed
- Has mentored at least one engineer with measurable career impact
- Has presented technical work to ≥ 20 people at least once
- Strong opinions about one or more areas of AI infrastructure, weakly held

### Required completion in this curriculum
- All four prior projects (or equivalent experience) — the capstone leverages and references work from them
- Module 501 (Technical Strategy)
- Module 502 (Mentorship & Leadership) — **central to this project**
- Module 503 (Cross-Org Initiative)
- Module 504 (Open Source / Community) — relevant for the proposal
- Module 505 (Long-term Technical Bets)

### Infrastructure assumed available
- Time on calendar for mentees (real or simulated)
- A peer-review group of ≥ 3 engineers willing to review your design doc and ADRs
- Recording setup for talks and coaching sessions (Loom, Zoom, OBS, anything)
- Permission to submit to at least one conference / external venue (or commitment to an internal venue)

---

## Deliverables (Summary — see `deliverables/README.md` for full submission spec)

1. **`design-doc.md`** (20–35 pages) — one substantial design on a real AI-infra problem
2. **ADR series** `adr/0001..0010` — 6–10 ADRs forming a coherent worldview across one domain
3. **`mentorship-plan.md`** — 12-month plans for ≥ 2 mentees
4. **Coaching artifacts** — ≥ 3 recorded sessions (or structured plans) + a reflection on each
5. **Mentorship reflection** — `mentorship-reflection.md` — honest 5–10 page self-assessment of multiplier behavior
6. **Tech talk** — 30–45 min recorded; slides; speaker notes; written abstract
7. **Conference proposal** — submitted (or submission-ready) to a real venue, with cover letter and rationale
8. **Connective synthesis** — `worldview.md` — the 3–5 page statement of your principal-level point of view, with links across artifacts
9. **Peer reviews** — captured comment threads from ≥ 3 reviewers on the design doc and ADRs
10. **Self-assessment** — honest rubric scoring with rationale per dimension

---

## Week-by-Week Duration (80 hours total)

| Week | Hours | Focus |
|------|-------|-------|
| 1 | 16 h | Worldview statement, topic selection, design doc draft 1, ADR domain outline |
| 2 | 22 h | Design doc completion, ADR series authorship |
| 3 | 22 h | Mentorship plans + coaching sessions, talk preparation |
| 4 | 20 h | Tech talk recording, conference proposal, peer reviews, polish, self-assessment |

Part-time at 10 h/week takes ~8 weeks; same phasing.

Day-by-day in [`STEP_BY_STEP.md`](./STEP_BY_STEP.md).

---

## Success Criteria

You have completed this project at a passing principal level when **all** of these are true:

### Design
- Design doc is **20–35 pages** on a substantial AI-infra problem, reviewed by ≥ 3 senior or staff engineers with their comments visibly addressed
- Doc has an executive summary survivable in 90 seconds and depth survivable in line-by-line review
- Decisions are defended with alternatives, consequences, and trigger conditions for reversal

### ADRs
- **6–10 ADRs** on a single coherent domain (not a grab-bag)
- Each ADR includes alternatives considered, consequences accepted, and either trigger conditions for reconsideration or a frank "this is permanent and here's why"
- At least one ADR captures a decision you would now make differently

### Mentorship
- 12-month mentorship plans for **≥ 2 mentees** with concrete growth axes
- ≥ 3 coaching sessions captured (recordings or structured plans), each with a written reflection
- A 5–10 page reflection on multiplier behavior — what you do, what you don't, what you're working on

### Communication
- Tech talk **30–45 min** recorded with audio + slides; thesis stated in the first 90 seconds
- Conference proposal **submitted** (or submission-ready) with cover letter
- Talk and proposal connect to the design and ADRs — same worldview

### Integration
- `worldview.md` (3–5 pages) names your principal-level point of view and shows the connective tissue
- A reviewer can trace the worldview through the design doc, ≥ 2 ADRs, the talk, and the proposal

### Stretch criteria (for higher scores)
- Conference proposal **accepted** (or invited talk in any external venue)
- Design doc adopted as the basis for a real initiative in an org
- A mentee visibly grows during the project (promoted, takes on a stretch role, gives their first talk)
- Worldview articulated in a way that a peer outside your org would adopt as a reference

---

## Related Lessons

| Lesson | How it feeds this project |
|--------|---------------------------|
| **Module 501 — Technical Strategy** | Design doc framing; ADR worldview |
| **Module 502 — Mentorship & Leadership** | Coaching sessions; mentorship plans; reflection |
| **Module 503 — Cross-Org Initiative** | The design doc almost always implicates other teams |
| **Module 504 — Open Source / Community** | Conference proposal; external-talk craft |
| **Module 505 — Long-term Technical Bets** | The decisions in your ADRs |

---

## Rubric Summary

See [`rubric.md`](./rubric.md) for the full grading rubric. High-level:

| Dimension | Weight | What "Exceeds" looks like |
|-----------|--------|---------------------------|
| Design doc craft & depth | 25 % | Reviewable as an industry tech blog or conference paper; survives line-by-line by a distinguished engineer |
| ADR series & worldview coherence | 20 % | ADRs form a single defensible thesis; outside reader could derive your principles |
| Mentorship & multiplier evidence | 20 % | Real growth in mentees attributable to the work; reflection is unusually honest |
| Tech talk & external communication | 15 % | Talk publishable externally; sticky thesis; well-paced |
| Conference proposal | 10 % | Submitted (or accepted); compelling hook; clean fit to venue |
| Connective synthesis | 10 % | Outside reader reconstructs your worldview from the artifacts alone |

Minimum **70 / 100** to pass. **85+** is portfolio-grade — the artifact you point at in a promo packet.

---

## How to Use This Project

This is structured for **self-paced principal-track learners** treating the capstone as the artifact a promotion committee will open.

1. Read this README, [`requirements.md`](./requirements.md), and [`architecture.md`](./architecture.md) end to end before writing anything.
2. **Write the worldview statement first.** It's only 3–5 pages but it constrains everything else. If you can't write it in week 1, the rest of the portfolio will lack a center.
3. Pick the design doc topic, the ADR domain, the mentees, and the talk thesis so they cluster around the worldview. A scattered portfolio scores poorly even if each piece is strong.
4. Build incrementally per [`STEP_BY_STEP.md`](./STEP_BY_STEP.md). Get the design doc in front of reviewers in week 2, not week 4.
5. Treat the mentorship work as a real commitment, not a checkbox. The reflection is what's graded; the reflection is only honest if the work was real.
6. Have at least four reviewers on the final package: one technical peer at staff+, one current or former manager, one engineer junior to you (for the talk), one venue program committee member (for the proposal).

Good luck. The engineers who get this right have a portfolio that opens doors for the next decade. This is the artifact you'll be glad you wrote.
