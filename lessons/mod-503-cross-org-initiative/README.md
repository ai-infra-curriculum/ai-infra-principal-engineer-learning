# mod-503-cross-org-initiative: Cross-Org Initiative Leadership

**Level**: principal-engineer-learning
**Duration**: 30-50 hours
**Prerequisites**: senior-engineer or senior-architect level

## Overview

A technical strategy that lives inside one team is a design
doc. A technical strategy that has to be executed across five
teams, two orgs, and three quarterly plans is an initiative —
and it is the single most failure-prone project shape in
software engineering. Most of the hard calls a principal
engineer makes in a given year are not about what the right
system looks like; they are about how to get the right system
built when no one reports to you and every affected team has
its own roadmap, its own manager, and its own reasons to
quietly deprioritize your work.

This module is about doing that work on purpose. Where
`mod-501-technical-strategy` is about committing the
organization to a direction in writing, and
`mod-502-mentorship-leadership` is about the long-horizon
investment in the people who will lead the work after you, this
module is the operational companion to both. It is about the
quarter-by-quarter craft of driving a 6-24 month initiative
through an organization that did not entirely agree to it, and
of landing a measurable outcome without burning the political
capital you will need for the next one.

This is the third of five modules in the principal-engineer
learning track. It assumes you can already write the memo and
move the room; it asks whether you can actually deliver on
what the memo promised, across boundaries you do not control.

## Learning objectives

By the end of this module you should be able to:

- **Scope an initiative honestly.** State the measurable
  outcome, the teams that have to change, the timeline, and
  the kill criteria — before you have exec sign-off, not
  after. An initiative that cannot be stated in a paragraph
  cannot be led.
- **Secure and maintain executive sponsorship.** Name the
  VP-or-above who will consistently ask about the work, agree
  with them in writing on what success looks like, and keep
  that agreement fresh as org priorities shift underneath you.
- **Build a working coalition across 3-10 teams.** Identify
  the team leads whose cooperation is load-bearing, understand
  what each of them is being measured on, and structure the
  initiative so their success and yours are the same success.
- **Run a cross-org project cadence that is legible to
  everyone involved.** Weekly or bi-weekly writeups, a shared
  status artifact, a dependency graph that is kept current,
  and decision checkpoints that happen on schedule rather than
  when something breaks.
- **Resolve cross-team conflict without pulling rank.**
  Distinguish disagreement on *facts*, on *priorities*, and on
  *values*, and route each to the appropriate forum. Know when
  to escalate, when to concede, and when to work around.
- **Manage initiative risk explicitly.** Keep a living risk
  register. Name the top three things that could kill the
  initiative, the leading indicators for each, and the move
  you will make the week they turn red.
- **Land the thing.** Drive to the measurable outcome and
  then actually declare done — including the unglamorous work
  of migrating the last 5% of holdouts, writing the
  post-mortem on what the initiative cost, and dissolving the
  coalition cleanly.
- **Decide when to kill an initiative.** Recognize the
  patterns of a dying initiative — sponsor turnover, drifting
  scope, teams quietly rebuilding what you were supposed to
  deliver — and either revive it on new terms or shut it down
  with your reputation intact.

## What this module is not

- It's not a project-management course. Gantt charts,
  sprint planning, and Jira hygiene are not the point.
  The point is the judgment that sits above
  project-management mechanics — what to commit to, whom to
  commit with, and when to change course. If you want the
  mechanics, the engineering-manager track covers them.
- It's not a people-leadership module. Growing the senior
  engineers who will lead *their own* initiatives is
  `mod-502-mentorship-leadership`. This module is about
  delivering on a specific body of work with the engineers
  you have, not about developing them for the next one.
- It's not a technical-strategy module. The *decision* to
  run the initiative lives in `mod-501-technical-strategy`.
  By the time you're in this module, that decision has been
  made and committed to in writing; the question is whether
  you can execute on it.
- It's not an incident-command module. Cross-org incident
  response is a distinct skill with its own sibling material
  in the architect track. The tempo and stakes are different;
  don't confuse a 24-month platform migration with a 24-hour
  outage.

## Module structure

| File | Purpose |
|---|---|
| [`lecture-notes/01-overview.md`](lecture-notes/01-overview.md) | The orientation. Defines a cross-org initiative, names the structural reasons they fail, and introduces the operational vocabulary — sponsor, driver, coalition, risk register, kill criteria — used throughout the module. |
| [`exercises/exercise-01-case-study.md`](exercises/exercise-01-case-study.md) | Reverse-engineer a real cross-org initiative you were affected by (as a participant, an onlooker, or a casualty). What did the driver do well? Where did it go wrong? What would you have done at the three-month mark, the twelve-month mark, the point of no return? |
| [`exercises/exercise-02-strategy-memo.md`](exercises/exercise-02-strategy-memo.md) | Draft the initiating memo for a real cross-org initiative you are leading or shadowing. Must include: the measurable outcome, the affected teams, the sponsor, the timeline, the kill criteria, and a one-paragraph statement of why this is worth doing now. A memo without these is not yet an initiative. |
| [`exercises/exercise-03-stakeholder-mapping.md`](exercises/exercise-03-stakeholder-mapping.md) | For the initiative from 02, build the stakeholder and dependency map. Named team leads, named execs, named blockers. For each, write what they have to gain, what they have to lose, and what you plan to do in the first six weeks to earn their active cooperation rather than their passive compliance. |
| [`exercises/exercise-04-roadmap.md`](exercises/exercise-04-roadmap.md) | Translate the initiative into a quarterly roadmap with explicit cross-team dependencies, decision checkpoints, and kill criteria tied to leading indicators. Name the top three risks and the leading indicator you will watch for each. Name the work the initiative is deliberately *not* doing to stay on schedule. |
| [`exercises/exercise-05-presentation.md`](exercises/exercise-05-presentation.md) | Deliver a 30-minute initiative review to a simulated audience of your sponsor, the affected team leads, and one skeptical peer. The exercise is evaluated on clarity about risk, honesty about the teams that are falling behind, and willingness to name what would cause you to recommend killing the initiative — not on the confidence of the forecast. |
| [`quiz.md`](quiz.md) | Discussion questions on scope negotiation, coalition maintenance, escalation paths, and the politics of declaring done. Meant to be worked through with a peer, a mentor, or a study group. |
| [`resources.md`](resources.md) | Frameworks, further reading, and cross-references to sibling repos. |

## How to work through it

1. Read `lecture-notes/01-overview.md` end to end before
   starting any exercise. The distinctions it draws — between
   a sponsor and an approver, between a coalition and a
   stakeholder list, between a risk and an issue — are
   assumed by every exercise after it.
2. Pick a real initiative. The work is only valuable if the
   stakes are real. If you are not currently driving an
   initiative large enough to need 5+ teams, shadow a
   principal in your org who is, and use theirs with
   permission. A hypothetical initiative produces hypothetical
   judgment.
3. Do the exercises in order. 02 through 05 form one
   connected arc: the memo, the stakeholder and dependency
   map, the roadmap with risks, and the review that holds you
   accountable to all three. The review is not the end — it
   is the moment you discover whether the memo was honest.
4. Share the memo (02) with one skeptical team lead *before*
   the simulated review. If the lead whose team is doing the
   most work for the initiative reads the memo and says "I
   can see why this matters to me," the memo is probably
   good. If they say "sure, happy to help where I can," the
   memo has not yet done its job — it has left the initiative
   as *your* problem rather than *our* problem.
5. Put a calendar reminder on the roadmap (04) for three
   months out and again at six months. Re-read the risks. Did
   the leading indicators actually lead? Which risks did you
   fail to name? The gap between the plan and the reality is
   where the skill is learned, and the discipline of looking
   back is what separates principal engineers from people who
   have driven exactly one initiative.

## Prerequisites in substance

The "senior-engineer or senior-architect level" line in the
header is a formal gate. The real prerequisites are:

- You have been on the receiving end of a cross-org
  initiative driven by someone else. You can describe what it
  cost your team, whether the cost was worth it, and what the
  driver did or didn't do that made the difference.
- You have delivered a project that required real
  cooperation from at least one team outside your own. You
  know what it feels like to depend on work you do not
  control, and you have at least one memory of that
  dependency slipping in a way that reshaped your plan.
- You have been in a room where a senior engineer escalated
  a cross-team disagreement to management — well or badly —
  and you can articulate what you would have done
  differently. The honest answer, including the times you
  were the one who escalated badly, is where this module
  starts.

If none of those are true yet, the material in this module
will read as abstract. Do a tour in
`ai-infra-staff-engineer-learning/mod-304-cross-team-projects`
first and come back.

## What success looks like

You can tell the module landed if, six months from finishing
it, the following are true. An initiative you are driving has
a one-paragraph statement that every affected team lead could
recite without looking it up. There is a shared status
artifact that is updated on a predictable cadence, read by
the sponsor without being prompted, and trusted enough that
leads use it instead of asking you directly. You have had at
least one hard conversation with a team lead whose team was
falling behind, and the conversation ended with a commitment
rather than a grudge. You know — out loud, to your sponsor —
what would cause you to recommend killing the initiative, and
the sponsor has acknowledged that recommendation would be
taken seriously. And when a peer principal starts their own
initiative in an adjacent org, the first thing they ask for
is your memo template.

## Where this module fits

| Module | What it adds |
|---|---|
| [`mod-501-technical-strategy`](../mod-501-technical-strategy/README.md) | How to commit the organization to a technical direction and document the reasoning behind it. The initiative in 503 is almost always executing on a strategy from 501. |
| [`mod-502-mentorship-leadership`](../mod-502-mentorship-leadership/README.md) | How to grow the engineers who will lead *their own* initiatives — and how to influence rooms you do not own, which is the skill every cross-org driver draws on daily. |
| `mod-503-cross-org-initiative` *(you are here)* | How to deliver a 6-24 month technical initiative across team and org boundaries you do not control. The operational companion to 501 and 502. |
| [`mod-504-open-source-community`](../mod-504-open-source-community/README.md) | How to run the same kind of coalition work outside your organization, where you have even less authority and the contributors have no reporting line at all. |
| [`mod-505-long-term-technical-bets`](../mod-505-long-term-technical-bets/README.md) | The multi-year research bets that occasionally graduate into the initiatives this module teaches you to drive. |

Solutions and reference materials live in the paired
`ai-infra-principal-engineer-solutions` repository under
`mod-503`. Use those *after* you've drafted your own
memo, stakeholder map, and roadmap, not before — the
exercises lose their point if you've already seen the model
answer.
