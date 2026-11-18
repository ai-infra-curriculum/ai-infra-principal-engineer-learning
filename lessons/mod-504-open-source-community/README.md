# mod-504-open-source-community: Open Source + Community Influence

**Level**: principal-engineer-learning
**Duration**: 30-50 hours
**Prerequisites**: senior-engineer or senior-architect level

## Overview

By 2026, the AI-infrastructure stack your company
differentiates on is almost entirely other people's open
source. PyTorch, vLLM, Ray, Kubernetes, MLflow, LangChain,
FastAPI, Triton, and a long tail of smaller projects sit
beneath nearly every serious ML platform. The engineering
judgment that used to be about which internal system you
built is increasingly about which external projects you bet
on, how you engage with them, and what you release — or
deliberately do not release — of your own.

A principal engineer who cannot navigate the OSS world is
operating with one hand tied. The hard calls are rarely
technical. They are decisions about where your company sits
in the portfolio of OSS engagement modes, when a project
deserves a maintainer seat from one of your team's named
engineers, when to ship something new to standardize a
category versus adopt something that already exists, and
when to spend company money and your own time on foundation
governance that has no short-term ROI story.

This is the fourth of five modules in the principal-engineer
learning track. Where `mod-503-cross-org-initiative` taught
the operational craft of driving work across teams you do
not control, this module extends the same influence *outside*
the company — into communities where contributors have no
reporting line to anyone, there is no exec to escalate to,
and your only durable currency is the quality of what you
ship and how you behave when you ship it. The leverage is
enormous. The failure modes are quieter and slower, and the
reputation costs, when they land, land personally.

## Learning objectives

By the end of this module you should be able to:

- **Choose an engagement mode for each dependency, on
  purpose.** For every strategic OSS project your platform
  relies on, pick one of consume, fix-and-PR, co-maintain,
  sponsor, or lead — and defend the choice. Most companies
  muddle these modes, free-riding on load-bearing
  dependencies while over-investing in projects that do not
  move their strategy.
- **Decide when to release a new project.** Weigh the
  standard-setting, recruiting, ecosystem, and strategic-
  positioning upside against the zombie-project, embarrassment,
  and community-splintering downside. Treat a release as the
  5-10 year commitment it is, and write down who inside the
  company carries that commitment after you move on.
- **Run an OSS release process inside a company.** License
  review, IP cleanup, security posture, contributor license
  agreement or developer certificate of origin, release
  notes, a public roadmap, and the governance model the
  project will ship with on day one — not after the first
  controversy.
- **Participate in foundation governance deliberately.**
  Decide whether CNCF, LF AI & Data, ASF, or another
  foundation membership is worth the annual fee and the
  engineering time at TSC/TOC meetings, and if so, name the
  specific decisions you plan to influence and the engineers
  who will be in the room.
- **Contribute at the right altitude.** Know when to send a
  one-line fix upstream, when to take a maintainer seat, and
  when to decline both because the right move is to keep the
  patch internal and own that trade-off honestly.
- **Build personal community credibility that compounds.**
  Sustained contribution to a few projects, public writing
  about real engineering work, and visible help to other
  contributors — chosen and paced so the work is durable
  rather than a performance.
- **Tell the truth in public.** Register technical dissent on
  a mailing list, a GitHub issue, or a conference stage
  without burning the relationship, and know when the right
  move is to stay quiet instead.

## What this module is not

- It's not a marketing or devrel playbook. "Grow the
  community" as a marketing goal is a different discipline
  with a different reporting line. This module is about the
  engineering judgment that governs OSS work, not about
  campaigns, launches, or hype cycles.
- It's not a legal course. License compatibility, patent
  grants, CLAs, and export controls are real, and you need
  competent counsel. This module names where legal input is
  load-bearing and teaches the questions to bring to your
  lawyer — it does not try to replace one.
- It's not a cross-org-initiative module. Internal coalition
  work is `mod-503-cross-org-initiative`. The shapes rhyme,
  but the authority gradient is different: inside the
  company, you can at least escalate to a shared executive,
  while in a community you cannot.
- It's not a project-management course for OSS repos. Triage
  policies, release trains, and bug-bash cadences are
  real, but they are downstream of the strategic questions
  this module centers. If the strategic frame is wrong, the
  mechanics will not save it.

## Module structure

| File | Purpose |
|---|---|
| [`lecture-notes/01-overview.md`](lecture-notes/01-overview.md) | The orientation. Introduces the portfolio-of-engagement frame (consume, fix-and-PR, co-maintain, sponsor, lead), the release decision, foundation participation, and the personal-credibility patterns the rest of the module assumes. |
| [`exercises/exercise-01-case-study.md`](exercises/exercise-01-case-study.md) | Reverse-engineer an OSS project you or your organization depends on. What mode has your company actually been in — not the one leadership would claim? Where did a maintainer or a sponsor company shape the direction in a way that helped or hurt you? What would you have done differently if the call had been yours? |
| [`exercises/exercise-02-strategy-memo.md`](exercises/exercise-02-strategy-memo.md) | Draft an OSS engagement memo for a domain you own. Name the top 10 external projects your platform depends on, assign each an engagement mode with a one-paragraph rationale, and state the one or two projects (if any) you would release yourself in the next 18 months — including the kill criteria for that release. A memo without the mode-per-project table is not yet a strategy. |
| [`exercises/exercise-03-stakeholder-mapping.md`](exercises/exercise-03-stakeholder-mapping.md) | For the memo from 02, map the stakeholders whose buy-in you need to execute. Legal, security, your VP, the engineers who would take the maintainer seats, the foundation program manager, and the external maintainers whose cooperation you are asking for. For each, name what they have to gain, what they have to lose, and what you plan to do in the first six weeks to earn their active cooperation rather than their silent tolerance. |
| [`exercises/exercise-04-roadmap.md`](exercises/exercise-04-roadmap.md) | Translate the strategy into a 12-18 month roadmap: which engagement mode changes happen in which quarter, which engineers are committed to which projects at what percentage of their time, what foundation participation you are starting or ending, and what you are deliberately *not* doing to stay honest about capacity. Include the leading indicators that would tell you a mode choice was wrong. |
| [`exercises/exercise-05-presentation.md`](exercises/exercise-05-presentation.md) | Deliver a 30-minute strategy review to a simulated audience of your engineering VP, a skeptical peer principal, and one external maintainer from a project you want to deepen your engagement with. The exercise is evaluated on honesty about the dependencies you are free-riding on, clarity about the projects you are choosing *not* to invest in, and willingness to name what would cause you to step back from a maintainer seat — not on the ambition of the plan. |
| [`quiz.md`](quiz.md) | Discussion questions on prioritization, failure modes, success measurement, first steps, and stakeholder alignment. Meant to be worked through with a peer, a mentor, or a study group. |
| [`resources.md`](resources.md) | Frameworks, further reading, and cross-references to sibling repos. |

## How to work through it

1. Read `lecture-notes/01-overview.md` end to end before
   starting any exercise. The five engagement modes and the
   release decision frame are referenced by every exercise
   after it, and the vocabulary ("standard-setting release",
   "zombie project", "foundation participation as use rather
   than pay") is assumed rather than reintroduced.
2. Pick a real portfolio. The exercises are only valuable if
   the projects, the maintainers, and the trade-offs are real
   ones you work with. A hypothetical OSS strategy produces
   hypothetical judgment — and OSS is a domain where the
   judgment only calibrates against public record. If you do
   not yet own a portfolio, shadow a principal who does and
   use theirs with permission.
3. Do the exercises in order. 02 through 05 form one
   connected arc: the engagement memo, the stakeholder map,
   the capacity-honest roadmap, and the review that holds you
   accountable to all three. The review is not the end — it
   is the moment you discover whether your mode choices
   survive contact with a maintainer in the room.
4. Share the memo (02) with one external maintainer *before*
   the simulated review, if you can do so without leaking
   anything you cannot share. The most useful feedback on an
   OSS engagement memo is almost never from inside the
   company. If a maintainer of a project you claim to
   "co-maintain" says "we hadn't noticed you were trying to",
   the memo is diagnosing something real.
5. Put a calendar reminder on the roadmap (04) for six months
   out and again at twelve. Re-read the mode assignments. Did
   the engineers you named actually spend the time you
   committed? Did the maintainer seat you promised yourself
   actually materialize? Did the release you planned happen,
   or did you quietly let it go? The gap between the plan and
   the reality is where the skill is learned, and OSS has an
   especially long feedback loop — the discipline of looking
   back is what separates principals who can shape ecosystems
   from those who have only observed them.

## Prerequisites in substance

The "senior-engineer or senior-architect level" line in the
header is a formal gate. The real prerequisites are:

- You have landed a non-trivial contribution to an OSS
  project you did not start. You know what it feels like to
  wait on a maintainer's review, to rewrite a patch to match
  a project's conventions you did not agree with, and to have
  a change accepted or rejected on terms that were not
  entirely about the technical merit.
- You have owned a production system with a load-bearing OSS
  dependency that got into trouble — a CVE, a maintainer
  burnout, a licensing change, a project fork. You can name
  what you did about it at the time and what you would do
  differently if the same situation landed tomorrow.
- You have been in a room where the question "should we
  release this?" came up for a piece of internal code, and
  you can articulate the answer you gave, whether it turned
  out to be right, and what you would want to know now that
  you did not know then.

If none of those are true yet, the material in this module
will read as abstract. Do a tour in
`ai-infra-staff-engineer-learning/mod-305-open-source-contribution`
first and come back.

## What success looks like

You can tell the module landed if, twelve months from
finishing it, the following are true. There is a written
engagement memo for your platform's OSS portfolio, read by
your VP and at least one peer principal, that assigns an
explicit mode to each strategic dependency and is updated at
least annually. At least one of those modes has changed
deliberately on your watch — a project you used to free-ride
on now has a named engineer contributing at a committed
percentage of their time, or a project you used to
co-maintain has been handed back to the community cleanly
because it was no longer strategic. If you released a new
project, it has a maintainer rotation, a public roadmap, and
an internal owner who is not you. If you are in a foundation,
your company is *using* the membership — a named engineer is
in the TSC or TOC meetings and has influenced at least one
decision that affected your roadmap — rather than passively
paying the annual fee. And externally, there is at least one
maintainer at a project you depend on who would answer a DM
from you within a day, because you have earned that access
by sustained, visible, non-performative work.

## Where this module fits

| Module | What it adds |
|---|---|
| [`mod-501-technical-strategy`](../mod-501-technical-strategy/README.md) | How to commit the organization to a technical direction in writing. The OSS engagement memo in 504 is a specific instance of the broader memo discipline taught in 501. |
| [`mod-502-mentorship-leadership`](../mod-502-mentorship-leadership/README.md) | How to influence rooms you do not own. The external version of that skill — influencing maintainer rooms, TSCs, and public mailing lists — is where 504 picks up. |
| [`mod-503-cross-org-initiative`](../mod-503-cross-org-initiative/README.md) | How to deliver on a strategy across team boundaries inside the company. 504 is the same operational craft applied outside the company, where the authority gradient is flatter still. |
| `mod-504-open-source-community` *(you are here)* | How to run the OSS portfolio, release deliberately, participate in foundation governance, and build personal community credibility that compounds. |
| [`mod-505-long-term-technical-bets`](../mod-505-long-term-technical-bets/README.md) | The multi-year research bets that occasionally become the OSS projects a principal has to decide whether to release and sustain. |

Solutions and reference materials live in the paired
`ai-infra-principal-engineer-solutions` repository under
`mod-504`. Use those *after* you've drafted your own
engagement memo, stakeholder map, and roadmap, not before —
the exercises lose their point if you've already seen the
model answer.
