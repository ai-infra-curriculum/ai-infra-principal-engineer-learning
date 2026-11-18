# mod-501-technical-strategy: Technical Strategy + ADRs

**Level**: principal-engineer-learning
**Duration**: 30-50 hours
**Prerequisites**: senior-engineer or senior-architect level

## Overview

Every AI-infrastructure organization reaches a point where the
bottleneck is no longer *who can build what*, it's *which
problems we've decided are worth building deeply*. This module
is about the engineering judgment that governs those decisions.

At the principal level, the primary artifact of your work is no
longer code. It's the written reasoning that commits the
organization — ADRs, strategy memos, 3-year direction docs —
and the quality of the technical bets those documents make. The
goal of this module is to make that work deliberate: to teach
you to write documents that cause durable, well-reasoned
decisions to happen, rather than being after-the-fact
justifications of whatever the team was going to do anyway.

This is the first of five modules in the principal-engineer
learning track. It establishes the posture the rest of the
track assumes: that your output is measured in the right
long-range calls made, not the lines of code shipped.

## Learning objectives

By the end of this module you should be able to:

- **Distinguish strategy from tactics.** Name the difference
  between a 3-year technical bet, a 6-month roadmap item, and a
  technology-selection decision — and route each to the right
  kind of document.
- **Author Architecture Decision Records** that future
  engineers (and the future version of you) can read and act
  on. This includes context, decision, alternatives, and
  consequences — and the discipline of writing the ADR
  *before* the decision solidifies, not after.
- **Draft a 3-year technical strategy document** for an
  AI-infrastructure domain you own (training platform,
  inference serving, data pipeline, GPU fleet, etc.). The
  document must state the bets, the kill criteria, and the
  path to validation.
- **Run a technical strategy review.** Convene the right
  stakeholders, surface dissent before it becomes resistance,
  and leave the meeting with a commitment rather than a plan
  to meet again.
- **Influence org-level technical direction** without relying
  on positional authority — by making the reasoning in your
  documents the most legible reasoning in the room.
- **Calibrate.** Track the predictions in your previous
  strategy docs. Notice where you were systematically over- or
  under-confident, and adjust.

## What this module is not

- It's not a course on picking frameworks. "Postgres vs.
  DynamoDB" is a technology decision, not a strategy. ADRs
  record those decisions; they're not the point of the module.
- It's not a management or people-leadership module. That's
  `mod-502-mentorship-leadership`. The leadership-without-
  authority work here is specifically about causing technical
  decisions to happen — not about mentoring individuals or
  running a team.
- It's not a system-design module. We assume you can already
  design the system; the question here is *which system should
  we be designing, and why, and what are we deliberately not
  designing?*

## Module structure

| File | Purpose |
|---|---|
| [`lecture-notes/01-overview.md`](lecture-notes/01-overview.md) | The orientation. Defines technical strategy at the principal level, introduces ADRs and the strategy memo, and names the hardest part of the work: judgment under uncertainty. |
| [`exercises/exercise-01-case-study.md`](exercises/exercise-01-case-study.md) | Read a real principal-engineer strategy doc (public, from an AI-infra org) and reverse-engineer the bets it made, the alternatives it rejected, and the kill criteria it set. |
| [`exercises/exercise-02-strategy-memo.md`](exercises/exercise-02-strategy-memo.md) | Draft a 5-15 page strategy memo for a domain you own or shadow. Must include kill criteria. A memo without them is a wish. |
| [`exercises/exercise-03-stakeholder-mapping.md`](exercises/exercise-03-stakeholder-mapping.md) | For the memo from 02, map the stakeholders whose buy-in you need, what each one has to lose, and how you plan to reach alignment before — not during — the review. |
| [`exercises/exercise-04-roadmap.md`](exercises/exercise-04-roadmap.md) | Translate the memo into a 3-horizon roadmap: this quarter, this year, the three-year bet. Name what you will deliberately *not* do in each horizon. |
| [`exercises/exercise-05-presentation.md`](exercises/exercise-05-presentation.md) | Present the strategy to a simulated engineering-leadership audience. The exercise is evaluated on clarity of reasoning and willingness to articulate what would change your mind — not on persuasion. |
| [`quiz.md`](quiz.md) | Strategic-thinking discussion questions. These are open-ended and are meant to be worked through with a peer, a mentor, or a study group. |
| [`resources.md`](resources.md) | Frameworks, further reading, and cross-references to sibling repos. |

## How to work through it

1. Read `lecture-notes/01-overview.md` end to end before
   starting any exercise. The vocabulary (ADR, strategy memo,
   kill criteria, pre-mortem) is used throughout.
2. Pick a real domain for the exercises. The work is only
   valuable if the stakes are real. If you don't currently own
   a domain large enough for a 3-year strategy, shadow a
   principal in your org and use theirs with permission.
3. Do the exercises in order. 02 through 05 are a single
   connected piece of work — one memo, one stakeholder map,
   one roadmap, one presentation of all three.
4. Get the memo read by at least one skeptical peer *before*
   the simulated presentation. The point of the review is to
   surface dissent, not to confirm agreement. If the memo gets
   no pushback, either it's not saying anything strategic or
   you asked the wrong reviewer.
5. Save the predictions. Put a reminder on your calendar for
   six months out to re-read the memo and score each
   prediction against what actually happened. Calibration is a
   skill only developed by looking back.

## Prerequisites in substance

The "senior-engineer or senior-architect level" line in the
header is a formal gate. The real prerequisites are:

- You have been the technical lead on a system that is
  presently in production and that someone else has to
  maintain. You understand *why* that someone else has to be
  able to read your decision rationale.
- You have been on the receiving end of a bad technical
  strategy. You can name a time when an organization you were
  in committed to the wrong bet, and you can articulate what
  the warning signs were.
- You have written a design doc that was read by more than
  five people and that you would, today, write differently.

If none of those are true yet, the material in this module
will read as abstract. Do a tour in
`ai-infra-senior-architect-learning/mod-402-executive-leadership`
first and come back.

## What success looks like

You can tell the module landed if, in the next real technical
decision you face at work, you reach for an ADR template
before you reach for a Slack thread. You find yourself
writing down what would change your mind. You push back on a
roadmap item by asking what's being deliberately *not* done
to make room for it. And when a peer's strategy memo lands in
your inbox, the first thing you look for is the kill
criteria.

## Where this module fits

| Module | What it adds |
|---|---|
| `mod-501-technical-strategy` *(you are here)* | How to commit the organization to a technical direction and document the reasoning behind it. |
| [`mod-502-mentorship-leadership`](../mod-502-mentorship-leadership/README.md) | How to grow other senior engineers into the kind of people who can do the work in 501. |
| [`mod-503-cross-org-initiative`](../mod-503-cross-org-initiative/README.md) | How to deliver on the strategy when it crosses team boundaries. |
| [`mod-504-open-source-community`](../mod-504-open-source-community/README.md) | How to influence technical direction outside your own organization. |
| [`mod-505-long-term-technical-bets`](../mod-505-long-term-technical-bets/README.md) | The multi-year, pre-product-market research bets that sit above even strategy memos. |

Solutions and reference materials live in the paired
`ai-infra-principal-engineer-solutions` repository under
`mod-501`. Use those *after* you've drafted your own memo,
not before — the exercises lose their point if you've already
seen the model answer.
