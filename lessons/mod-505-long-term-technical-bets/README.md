# mod-505-long-term-technical-bets: Long-Term Technical Bets

**Level**: principal-engineer-learning
**Duration**: 30-50 hours
**Prerequisites**: senior-engineer or senior-architect level

## Overview

A principal engineer's hardest decisions are rarely the
ones with a payback window of weeks. They are the calls
with a payback window of years — the ones where, if the
read on the landscape is wrong, the cost is the years. In
2002 Amazon began building internal services as a public
cloud. In 2013 Google invested in TPUs. In 2015 Facebook
bet the data center on PyTorch over its own internal
framework. In 2022 a long list of ML organizations bet
their roadmaps on transformer-based foundation models for
everything. Some of those bets paid off massively, some
partially, some not at all. In every case, the engineers
who made the call signed up for a 5-10 year commitment
before the evidence that would have made the decision easy
was available. That is the work this module teaches.

This is the fifth and final module in the
principal-engineer learning track. Where
`mod-501-technical-strategy` is about committing the
organization to a direction over a 2-3 year horizon with
written reasoning, this module operates one horizon
longer: the pre-product-market research bets whose payoff
sits outside the window of any quarterly plan, any
business case built on observed revenue, or any roadmap a
sponsoring executive is likely to survive in their
current role. The craft is different. You cannot defend
a 5-10 year bet with a demo. You defend it with trend
evidence, kill criteria, and the discipline to
re-evaluate on a cadence you set before the enthusiasm
wore off.

The failure modes at this altitude are quieter than the
ones in earlier modules. A bad strategy memo (501) gets
pushed back on in review. A bad initiative (503) fails to
land in a measurable quarter. A bad long-term bet does
not fail on a legible timeline — it drifts, year after
year, consuming headcount and capital until someone
finally writes the retrospective, and by then the people
who made the call have moved on. The discipline this
module centers is the opposite of drift: deliberate
commitments, with written kill criteria, revisited on a
calendared cadence, in a portfolio you can defend as a
portfolio rather than as a collection of individual
excitements.

## Learning objectives

By the end of this module you should be able to:

- **Read a technical trend with calibrated confidence.**
  Name the specific, falsifiable movements in the
  landscape that make a bet possible or necessary —
  hardware cost curves, open-model capability curves,
  benchmark ceilings, systems-research plateaus — and
  separate them from the industry noise that recycles
  every 18 months. A bet grounded in "AI is big" is not
  grounded.
- **Shape a portfolio, not a wish list.** Commit the
  organization to 1-2 large bets, 3-5 medium bets, and a
  pipeline of small experiments, balanced across horizon,
  risk, and domain. A single-bet organization is fragile.
  A zero-bet organization is already losing.
- **Author a bet memo that future-you can be held to.**
  The trend, the implementation, the horizon, the
  investment, the kill criteria, the portfolio context,
  and the re-evaluation date — in writing, before the
  work begins. A bet memo without kill criteria is a
  wish, and a bet memo without a re-evaluation date is a
  commitment to drift.
- **Make the resource case for R&D.** Argue for headcount
  and capital whose return is 5-10 years out, in front of
  executives whose own tenure is likely shorter. Tie the
  ask to a thesis about the world, name the alternatives
  the money is being taken from, and state what evidence
  would cause you to recommend reducing the investment
  rather than growing it.
- **Run the re-evaluation cycle.** Every 6 or 12 months,
  produce a written update: progress against original
  thesis, what has changed in the landscape, whether any
  kill criteria have triggered, and what the next
  period's investment should be. Re-evaluation is the
  single highest-leverage habit in this module — it is
  also the one most often skipped, because it is
  uncomfortable.
- **Kill a bet with your reputation intact.** Recognize
  the signals — triggered kill criteria that are being
  rationalized, loss of the champions who started the
  work, competitive landscape moving around the thesis —
  and shut the bet down cleanly. A bet killed honestly
  preserves the organization's ability to make the next
  one. A bet kept on life support teaches the
  organization to distrust bets in general.
- **Tell bets apart from fashion.** Industry hype cycles
  and conference-driven enthusiasm will present
  themselves as long-term bets. Develop the muscle to
  name the difference — on paper, in review, in front of
  stakeholders who are rooting for the fashionable
  answer.

## What this module is not

- It's not a futurism or forecasting course. Predicting
  the technology landscape in detail five years out is
  not the goal, because it is not possible. The goal is
  to make bets whose structure survives being wrong in
  specific, named ways — the kill criteria are what make
  the bet accountable, not the accuracy of the forecast.
- It's not a product-strategy module. "What should the
  product be" is a different discipline with a different
  reporting line. This module is about the long-horizon
  technical investments that the product organization
  draws on — training platforms, serving stacks, data
  infrastructure, hardware choices — not about what the
  customer-facing product does with them.
- It's not `mod-501-technical-strategy`. The strategy
  memo in 501 commits the organization to a direction for
  the next 2-3 years, with roadmap items and concrete
  deliverables. The bet memo in 505 commits the
  organization to a research thesis for the next 5-10
  years, with no deliverable other than evidence. The
  shapes rhyme; the altitudes are different.
- It's not an incubation or innovation-lab playbook. How
  you staff a research group, how you run a skunkworks,
  how you transition a bet from research to production —
  all real, all downstream of the question this module
  centers. If the bet is wrong, the smoothest incubation
  process in the world produces a smooth failure.
- It's not venture-capital analysis. The vocabulary of
  portfolio, kill criteria, and risk-adjusted horizon is
  borrowed from the investor literature, but the
  constraints are different: you are committing *your
  own* organization's engineers, your own reputation, and
  your own time, not deploying external capital across a
  diversified fund. The ratio of writing to decisions is
  higher.

## Module structure

| File | Purpose |
|---|---|
| [`lecture-notes/01-overview.md`](lecture-notes/01-overview.md) | The orientation. Defines a long-term bet, introduces the portfolio frame (large / medium / small), lays out the shape of a well-formed bet (trend, implementation, horizon, investment, kill criteria, portfolio context), and names the re-evaluation discipline every exercise after it assumes. |
| [`exercises/exercise-01-case-study.md`](exercises/exercise-01-case-study.md) | Reverse-engineer a real long-term technical bet from public record — an AWS, a TPU, a PyTorch, a Kubernetes adoption wave, a foundation-model pivot. What trend was the bet grounded in? What was the implementation? What would the honest kill criteria have been? With hindsight, when should the bet have been revisited, and what evidence would have caused a reasonable principal to kill or double down? |
| [`exercises/exercise-02-strategy-memo.md`](exercises/exercise-02-strategy-memo.md) | Draft one long-term bet memo for a domain you own or shadow. Must include the trend with specific evidence, the implementation, the 5-10 year horizon, the investment ask (headcount and capital), the kill criteria, the portfolio context (what else the organization is betting on), and the calendared re-evaluation date. A memo without the kill criteria *and* the re-evaluation date is not yet a bet — it is a wish. |
| [`exercises/exercise-03-stakeholder-mapping.md`](exercises/exercise-03-stakeholder-mapping.md) | For the bet from 02, map the stakeholders whose commitment the bet needs to survive its horizon. The executive sponsor who will probably rotate before year five. The CFO who will ask for the numeric case. The research group that will be staffed against it. The product orgs whose roadmap decisions depend on the bet landing. For each, name what they have to gain, what they have to lose, what question they will ask in review, and what you plan to put in writing before that review so the conversation is about the bet rather than about your confidence. |
| [`exercises/exercise-04-roadmap.md`](exercises/exercise-04-roadmap.md) | Translate the bet into a multi-horizon plan. What does the first 12 months look like — the staffing, the early milestones, the first re-evaluation gate? The 24-36 month horizon — the medium-bet checkpoints, the capability milestones that would promote the bet from medium to large or demote it to small? The 5-10 year endpoint — what the world looks like if the bet pays off, and what the organization is deliberately *not* investing in to make room for it? Include the top three risks and the leading indicator you will watch for each. |
| [`exercises/exercise-05-presentation.md`](exercises/exercise-05-presentation.md) | Deliver a 30-minute R&D investment review to a simulated audience of your CTO or VP of Engineering, the CFO (or finance partner) who will own the cost line, and one skeptical peer principal. The exercise is evaluated on honesty about the trend evidence, clarity about what would cause you to recommend reducing the investment, and willingness to name the bets you are deliberately *not* making — not on the confidence of the forecast or the ambition of the plan. |
| [`quiz.md`](quiz.md) | Discussion questions on portfolio shape, kill-criteria discipline, re-evaluation cadence, and the politics of killing a bet that still has internal champions. Meant to be worked through with a peer, a mentor, or a study group. |
| [`resources.md`](resources.md) | Frameworks, further reading, and cross-references to sibling repos. |

## How to work through it

1. Read `lecture-notes/01-overview.md` end to end before
   starting any exercise. The vocabulary — long-term bet,
   portfolio, kill criteria, re-evaluation gate,
   calibrated trend read — is used throughout, and the
   distinctions it draws between a strategy memo and a
   bet memo, between a trend and a fashion, between
   killing and pausing, are assumed by every exercise
   after it.
2. Pick a real bet. If the trend is hypothetical, the
   investment ask is hypothetical, and the stakeholders
   are hypothetical, the memo is a writing exercise and
   the judgment it builds is hypothetical. If you are not
   currently in a position to propose a 5-10 year
   organizational bet, shadow a principal who is, and use
   theirs with permission. Bets without stakes do not
   calibrate.
3. Do the exercises in order. 02 through 05 form one
   connected arc: the bet memo, the stakeholder map, the
   multi-horizon roadmap, and the investment review that
   holds you accountable to all three. The review is not
   the end — it is the moment you discover whether the
   kill criteria you wrote down would actually hold up in
   front of a CFO.
4. Share the memo (02) with one skeptical peer *before*
   the simulated review. The useful reviewer is almost
   never a cheerleader. If the peer whose instinct runs
   opposite to your bet reads the memo and still
   understands why a reasonable person would make this
   commitment, the memo is doing its job. If the review
   reads as persuasive but not falsifiable, the kill
   criteria are not sharp enough yet.
5. Put a calendar reminder on the roadmap (04) for the
   first re-evaluation gate you named — typically 6 or 12
   months out. When the reminder fires, write the
   re-evaluation update: progress against original
   thesis, what has changed in the landscape, whether any
   kill criteria have triggered. The discipline of
   writing this update, on schedule, is the single
   highest-leverage habit the module teaches. A bet whose
   first re-evaluation gets quietly skipped is a bet in
   the process of drifting.

## Prerequisites in substance

The "senior-engineer or senior-architect level" line in
the header is a formal gate. The real prerequisites are:

- You have been wrong about a long-horizon technical
  call. You can name the trend you misread, the evidence
  you discounted at the time, and the moment at which an
  honest re-evaluation would have caught the error. The
  module reads as abstract to engineers who have not yet
  been held to a multi-year prediction.
- You have watched a bet die slowly inside an
  organization you were part of. You can name when the
  kill criteria were effectively triggered and how many
  more quarters the work continued anyway, and you can
  articulate what the champions would have had to admit
  to shut it down sooner.
- You have had to argue for engineering investment whose
  return is years out, in front of someone whose own
  incentives were quarterly. You remember what the hard
  question was and how you answered it, and you can say,
  honestly, whether the investment landed the way you
  defended it.

If none of those are true yet, the material in this
module will read as abstract. Do a tour in
`ai-infra-senior-architect-learning/mod-404-technology-radar`
first and come back. <!-- needs-research: confirm sibling
module path for long-horizon landscape reading in the
senior-architect track -->

## What success looks like

You can tell the module landed if, twelve to eighteen
months from finishing it, the following are true. There
is a written bet portfolio for your platform — one or two
large bets, three to five medium bets, a pipeline of
small experiments — read by your CTO and at least one
peer principal, with each bet carrying explicit kill
criteria and a calendared re-evaluation date. At least
one of those re-evaluations has actually happened on
schedule, produced a written update, and either confirmed
the thesis with specific evidence, adjusted the
investment deliberately, or killed the bet cleanly. The
CFO or finance partner who owns the R&D cost line can
state, without looking it up, the two or three leading
indicators you have committed to watch for each large
bet. A junior principal in your organization has written
their first bet memo and asked you to review it, and the
review surfaced a missing kill criterion rather than a
grammar edit. And when a conference talk or a vendor
pitch arrives claiming that a new trend changes
everything, the first thing you reach for is not the
slide deck but your own portfolio — to ask, honestly,
whether anything in it should be killed, grown, or left
alone.

## Where this module fits

| Module | What it adds |
|---|---|
| [`mod-501-technical-strategy`](../mod-501-technical-strategy/README.md) | How to commit the organization to a 2-3 year technical direction in writing. The bet memo in 505 operates one horizon longer than the strategy memo in 501, and the strategy memo frequently draws on the underlying bets this module teaches you to make. |
| [`mod-502-mentorship-leadership`](../mod-502-mentorship-leadership/README.md) | How to grow the engineers who will lead *their own* bets in five years. A long-term bet outlives the principal who authored it; the bet only survives its horizon if someone else can carry it. |
| [`mod-503-cross-org-initiative`](../mod-503-cross-org-initiative/README.md) | How to deliver on a 6-24 month initiative across team boundaries. Long-term bets occasionally graduate into initiatives, and the operational craft of 503 is what turns a bet's early milestones into landed work. |
| [`mod-504-open-source-community`](../mod-504-open-source-community/README.md) | How to run the OSS portfolio, release deliberately, and participate in foundation governance. Some long-term bets become OSS releases; the release decision in 504 is often downstream of a bet decision in 505. |
| `mod-505-long-term-technical-bets` *(you are here)* | How to make, defend, re-evaluate, and when necessary kill the 5-10 year research bets that sit above strategy memos, OSS release decisions, and cross-org initiatives. The capstone of the principal-engineer track. |

Solutions and reference materials live in the paired
`ai-infra-principal-engineer-solutions` repository under
`mod-505`. Use those *after* you've drafted your own bet
memo, stakeholder map, and roadmap, not before — the
exercises lose their point if you've already seen the
model answer.
