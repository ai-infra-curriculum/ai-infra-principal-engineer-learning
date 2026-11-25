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

The failure modes at this altitude are quieter than the ones at
staff or senior level. A bad implementation fails a code
review. A bad design doc fails a design review. A bad strategy
fails two years later — long after the author has moved on,
the sponsoring executive has rotated, and the engineers
living with the consequences have no written record of what
the alternatives were or which tradeoffs were explicitly
accepted. The discipline this module centers is the opposite
of that drift: written reasoning, legible alternatives,
explicit kill criteria, and a calendared re-read so the
predictions can be scored against what actually happened.

This is the first of five modules in the principal-engineer
learning track. It establishes the posture the rest of the
track assumes: that your output is measured in the right
long-range calls made, not the lines of code shipped. The
later modules all presume this one. Mentorship (502) is in
large part about transferring the kind of judgment this module
asks you to put on paper. Cross-org initiatives (503) are the
operational delivery of what a strategy memo promised. The
long-term bets in 505 sit one horizon above the 2-3 year
commitments this module teaches you to write. If you cannot
produce a strategy memo whose reasoning survives a skeptical
read, the rest of the track will read as theoretical.

## Learning objectives

By the end of this module you should be able to:

- **Distinguish strategy from tactics.** Name the difference
  between a 3-year technical bet, a 6-month roadmap item, and a
  technology-selection decision — and route each to the right
  kind of document. A strategy memo that reads as an ADR is
  under-reaching; an ADR that reads as a strategy memo is
  over-reaching. Know which document each decision needs and
  resist the temptation to collapse them.
- **Author Architecture Decision Records** that future
  engineers (and the future version of you) can read and act
  on. This includes context, decision, alternatives, and
  consequences — and the discipline of writing the ADR
  *before* the decision solidifies, not after. An ADR written
  after the code lands is documentation. An ADR written while
  the decision is still live is a decision tool.
- **Draft a 3-year technical strategy document** for an
  AI-infrastructure domain you own (training platform,
  inference serving, data pipeline, GPU fleet, etc.). The
  document must state the bets, the kill criteria, and the
  path to validation. A memo that commits the organization to
  a direction without naming what would cause the direction to
  change is not a strategy — it is a wish with footnotes.
- **Run a technical strategy review.** Convene the right
  stakeholders, surface dissent before it becomes resistance,
  and leave the meeting with a commitment rather than a plan
  to meet again. The review is not the moment the strategy
  gets ratified; it is the moment the written reasoning gets
  tested against the people who will have to live with it.
- **Influence org-level technical direction** without relying
  on positional authority — by making the reasoning in your
  documents the most legible reasoning in the room. The lever
  at this altitude is clarity, not seniority. If the memo is
  more readable than the alternative being pushed by someone
  with more org weight, the memo wins more often than the
  org chart predicts.
- **Calibrate.** Track the predictions in your previous
  strategy docs. Notice where you were systematically over- or
  under-confident, and adjust. A principal who never re-reads
  their own memos is a principal who cannot tell whether their
  judgment is improving. Calibration is the single
  highest-leverage habit this module teaches, and it is also
  the one most often skipped, because it is uncomfortable.

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
- It's not a futurism course. Predicting the technology
  landscape in detail three years out is not the goal, because
  it is not reliably possible. The goal is to make bets whose
  structure survives being wrong in specific, named ways. That
  is what the kill criteria are for, and it is why a memo
  built on a single confident forecast is a weaker document
  than one that names the trends it is tracking and the
  evidence that would cause it to change course.
- It's not a course on writing persuasively. Persuasion makes
  bad strategies survive the review and good strategies survive
  the first wave of pushback. Both outcomes are failures. The
  frame in this module is the opposite: write so that a reader
  who starts from the opposite instinct can follow the
  reasoning, grant the premises you are granting, and either
  come along or explain exactly where they diverge. A memo
  that no skeptical reader has ever pushed back on is a memo
  that is not yet saying anything strategic.

## Module structure

| File | Purpose |
|---|---|
| [`lecture-notes/01-overview.md`](lecture-notes/01-overview.md) | The orientation. Defines technical strategy at the principal level, introduces ADRs and the strategy memo, names the distinction between a 3-year bet and a 6-month roadmap item, and names the hardest part of the work: judgment under uncertainty, written down in a form other people can act on. |
| [`exercises/exercise-01-case-study.md`](exercises/exercise-01-case-study.md) | Read a real principal-engineer strategy doc (public, from an AI-infra org) and reverse-engineer the bets it made, the alternatives it rejected, and the kill criteria it set. Where the memo is silent on an alternative, name what the honest discussion of that alternative would have had to look like. The exercise is about reading, not writing — but the quality of your own memos will be bounded above by how carefully you have read other people's. |
| [`exercises/exercise-02-strategy-memo.md`](exercises/exercise-02-strategy-memo.md) | Draft a 5-15 page strategy memo for a domain you own or shadow. Must include kill criteria. A memo without them is a wish. The memo must also name at least two alternatives considered and rejected, and must state, for each, the specific evidence that would cause a reasonable principal to revisit the rejection. If you cannot name the alternatives, you have not yet made a strategic choice. |
| [`exercises/exercise-03-stakeholder-mapping.md`](exercises/exercise-03-stakeholder-mapping.md) | For the memo from 02, map the stakeholders whose buy-in you need, what each one has to lose, and how you plan to reach alignment before — not during — the review. Name the one or two stakeholders whose opposition could kill the memo, the question each is most likely to ask, and what you plan to put in writing before the meeting so the conversation is about the strategy rather than about your confidence. |
| [`exercises/exercise-04-roadmap.md`](exercises/exercise-04-roadmap.md) | Translate the memo into a 3-horizon roadmap: this quarter, this year, the three-year bet. Name what you will deliberately *not* do in each horizon. A roadmap that only lists what the team is doing is a plan. A roadmap that also names what the team is explicitly declining to do — and why — is a strategy you can defend when the next request lands. |
| [`exercises/exercise-05-presentation.md`](exercises/exercise-05-presentation.md) | Present the strategy to a simulated engineering-leadership audience. The exercise is evaluated on clarity of reasoning and willingness to articulate what would change your mind — not on persuasion. The hardest slide is the one listing the two or three pieces of evidence that would cause you to reduce investment in the strategy's central bet rather than grow it. |
| [`quiz.md`](quiz.md) | Strategic-thinking discussion questions. These are open-ended and are meant to be worked through with a peer, a mentor, or a study group. |
| [`resources.md`](resources.md) | Frameworks, further reading, and cross-references to sibling repos. |

## How to work through it

1. Read `lecture-notes/01-overview.md` end to end before
   starting any exercise. The vocabulary (ADR, strategy memo,
   kill criteria, pre-mortem) is used throughout, and the
   distinctions it draws — between a strategy and a plan,
   between a decision and a document that records a decision,
   between calibrated confidence and persuasive confidence —
   are assumed by every exercise after it.
2. Pick a real domain for the exercises. The work is only
   valuable if the stakes are real. If you don't currently own
   a domain large enough for a 3-year strategy, shadow a
   principal in your org and use theirs with permission. A
   memo written about a hypothetical domain is a writing
   exercise; it does not calibrate.
3. Do the exercises in order. 02 through 05 are a single
   connected piece of work — one memo, one stakeholder map,
   one roadmap, one presentation of all three. Skipping ahead
   defeats the point; the stakeholder map is only sharp if the
   memo is sharp, and the presentation is only honest if the
   kill criteria in the memo and the deliberate non-goals in
   the roadmap are already written down.
4. Get the memo read by at least one skeptical peer *before*
   the simulated presentation. The point of the review is to
   surface dissent, not to confirm agreement. If the memo gets
   no pushback, either it's not saying anything strategic or
   you asked the wrong reviewer. The useful reviewer is almost
   never a cheerleader; it is the peer whose instinct runs
   opposite to the memo's bet and who is willing to put the
   disagreement on the record.
5. Save the predictions. Put a reminder on your calendar for
   six months out to re-read the memo and score each
   prediction against what actually happened. Calibration is a
   skill only developed by looking back. Writing the
   six-month re-read on the same day you finalize the memo is
   not optional — it is the single thing that converts the
   exercise from a one-time writing assignment into an ongoing
   practice.

## Prerequisites in substance

The "senior-engineer or senior-architect level" line in the
header is a formal gate. The real prerequisites are:

- You have been the technical lead on a system that is
  presently in production and that someone else has to
  maintain. You understand *why* that someone else has to be
  able to read your decision rationale, and you have at least
  once been on the painful side of inheriting a system whose
  reasoning was not written down.
- You have been on the receiving end of a bad technical
  strategy. You can name a time when an organization you were
  in committed to the wrong bet, and you can articulate what
  the warning signs were — in a form sharper than "we should
  have seen it coming." Specifically: what was the kill
  criterion that was never written down, and how many quarters
  of avoidable work happened after a reasonable principal
  would have called it.
- You have written a design doc that was read by more than
  five people and that you would, today, write differently.
  The useful prerequisite here is not that the doc was good;
  it is that you have been held to something you wrote down
  long enough to see how your thinking has changed, and you
  can name the specific sentences you would now cut, keep, or
  add.

If none of those are true yet, the material in this module
will read as abstract. Do a tour in
`ai-infra-senior-architect-learning/mod-402-executive-leadership`
first and come back.
<!-- needs-research: confirm sibling module path for
executive-leadership exposure in the senior-architect track -->

## What success looks like

You can tell the module landed if, in the next real technical
decision you face at work, you reach for an ADR template
before you reach for a Slack thread. You find yourself
writing down what would change your mind. You push back on a
roadmap item by asking what's being deliberately *not* done
to make room for it. And when a peer's strategy memo lands in
your inbox, the first thing you look for is the kill
criteria. Six months out, you re-read your own strategy memo
on a calendar reminder you set the day you finalized it, and
you can state — without rationalizing — which predictions
held, which did not, and which of your dismissed alternatives
would be worth revisiting if you were writing the memo today.
A junior principal in your organization has asked you to
review their first strategy memo, and the review surfaced a
missing kill criterion or an unnamed alternative rather than a
grammar edit.

## Where this module fits

| Module | What it adds |
|---|---|
| `mod-501-technical-strategy` *(you are here)* | How to commit the organization to a technical direction and document the reasoning behind it. The foundation the rest of the track assumes: if the written reasoning is not legible, the mentorship, the cross-org delivery, the OSS posture, and the long-term bets that follow are all weaker. |
| [`mod-502-mentorship-leadership`](../mod-502-mentorship-leadership/README.md) | How to grow other senior engineers into the kind of people who can do the work in 501, and how to move rooms you do not own toward a better technical decision without pulling rank. |
| [`mod-503-cross-org-initiative`](../mod-503-cross-org-initiative/README.md) | How to deliver on the strategy when it crosses team boundaries. The operational companion to this module — a memo that cannot be executed across the org is a memo that did not land. |
| [`mod-504-open-source-community`](../mod-504-open-source-community/README.md) | How to influence technical direction outside your own organization, where the strategy memo has even less positional authority than it does inside. |
| [`mod-505-long-term-technical-bets`](../mod-505-long-term-technical-bets/README.md) | The multi-year, pre-product-market research bets that sit above even strategy memos. The 3-year commitment this module teaches you to write is frequently downstream of a 5-10 year bet; the bet memo in 505 is what tells you whether the strategy in 501 is pointed in a direction the organization has actually committed to. |

Solutions and reference materials live in the paired
`ai-infra-principal-engineer-solutions` repository under
`mod-501`. Use those *after* you've drafted your own memo,
not before — the exercises lose their point if you've already
seen the model answer.
