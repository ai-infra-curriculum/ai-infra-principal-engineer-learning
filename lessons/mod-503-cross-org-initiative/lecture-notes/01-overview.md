# Lecture 01: Cross-Org Initiative Leadership — Overview

## Why cross-org work is hard

A cross-org technical initiative is the most failure-prone
project shape in software engineering. The reasons are
structural:

- Multiple teams require *agreement* in addition to
  competence.
- Agreement requires *trust*, which takes time the project
  doesn't have.
- There's no manager who can simply tell the teams to do it.
- Each team has its own roadmap, manager, and quarterly
  commitments that compete with the initiative.

Principal engineers are often asked to lead these initiatives
*because* they lack positional authority — the theory is that
technical credibility + judgment compensates. Sometimes it
does. Often it doesn't.

This lecture is the orientation: what the work actually looks
like when it goes well, and what the patterns of failure are.

## The shape of a cross-org initiative

A cross-org technical initiative typically has:

- A sponsor at exec level (VP+) who consistently asks about
  progress. Without this, the initiative starves.
- A driver at principal-engineer level — that's you.
- 3-10 affected teams, each with their own work to integrate
  with the initiative.
- A timeline of 6-24 months.
- A measurable outcome (migration percentage, latency
  improvement, capability deployed).
- A *stated reason for being* that the affected teams can
  understand even if they don't love it.

Initiatives that lack any of these tend to die. The principal
engineer's first job is verifying that all six are present
before committing.

## The first ninety days

The first ninety days of a cross-org initiative are about
understanding, not driving:

**Weeks 1-3: discovery.** Talk to every affected team's lead
and at least one senior engineer per team. Listen more than
you talk. What do they think the initiative is? What concerns
them? What would make this work for them?

**Weeks 4-6: documentation.** Synthesize what you learned into
a written proposal. The proposal includes: what the initiative
is, why now, what each team is being asked to do, what they
get in return, what we're not doing.

**Weeks 7-9: review and refinement.** Send the proposal to
every team. Receive feedback. Update. The proposal you send
should be 80% the proposal you started with, plus the
refinements that came from team feedback.

**Weeks 10-13: commitment.** Bring it to the exec sponsor.
Walk through. Get explicit approval and budget. Communicate
to all teams that the initiative is greenlit.

Compressing this is the most common failure mode. Initiative
leads who skip discovery end up with proposals that hit
walls of resistance they didn't predict.

## Coalition building

Cross-org initiatives succeed by building coalitions, not by
issuing mandates. The coalition has structure:

**Champions.** 2-3 senior engineers across the affected teams
who actively support the initiative and pitch it in their
team. Find these people early and invest in the relationship.

**Decision authorities.** The managers and senior leaders who
can commit their teams. They need explicit briefings and
their concerns need to be addressed.

**The skeptics.** Every initiative has them. The mistake is
treating them as obstacles. The skill is treating them as
*information sources* — what they're worried about often
reveals real risks.

**The neutrals.** The largest group. They're not opposed but
they're also not actively helping. The work is moving the
neutrals to supportive.

## The execution layer

Once committed, the work shifts from selling to executing.
Patterns that work:

- Weekly cross-team sync with team-level representatives.
- A shared dashboard showing progress per team.
- A monthly written update to the exec sponsor.
- Quick recognition for teams that ship their part.
- Honest acknowledgment when something is slipping.
- A clear escalation path for when teams disagree.

The principal engineer is the program manager for the
initiative whether or not they think they should be.

## The hardest part: when it isn't working

Cross-org initiatives fail in slow motion. The signs are
subtle: a team that was committed becomes unresponsive; the
weekly sync shrinks; the dashboard updates lag.

The principal engineer's discipline is acknowledging early
when the initiative is in trouble. Three responses are
possible:

1. **Diagnose and fix.** Most slippages are recoverable if
   addressed early. Talk to the lagging team. Find out
   what's changed.
2. **Rescope.** Sometimes the original scope was too
   ambitious. Reducing scope (with the sponsor's explicit
   support) saves the initiative.
3. **Kill the initiative.** Sometimes the right move is to
   stop. The skill is recognizing this *before* you've spent
   another six months pushing.

Hiding the trouble is the failure mode. Initiatives that get
kept on life support for two extra quarters consume political
capital that should have been spent elsewhere.

## What this lecture doesn't cover

This is the orientation. Specific topics — managing the
coalition over time, structuring the program-management
work, communicating progress to the exec sponsor — each
deserve their own treatment.

For working examples and rubrics, see the principal-engineer-
solutions repo, mod-503.

## Further reading

- "Switch" — Chip and Dan Heath. Change management for
  multi-stakeholder initiatives.
- "Crucial Conversations" — Patterson et al. Hard
  conversations across team boundaries.
- "Influence" — Cialdini. The psychology of why people say
  yes.
- "Driving Technical Change" — Terrence Ryan. The pattern
  language for moving an organization technically.
