# Lecture 01: Technical Strategy and ADRs — Overview

## The principal engineer's job

At the staff and principal-engineer level, the work shifts
from *solving the technical problem* to *being responsible for
the technical judgment* exercised on the hard problems. The
principal engineer is the person other engineers' careers look
most like if things go well.

Technical strategy at this level is not about choosing
technologies. It's about deciding which problems are worth
solving deeply, which bets are worth making, and how to make
sure the company's hardest engineering work is being done
well.

This lecture is the orientation.

## What technical strategy is and isn't

Technical strategy is *not*:

- Picking which databases to use.
- Choosing a programming language for a project.
- Refactoring a legacy system.
- Adopting a new framework.

Each of those is a *technology decision*, not a strategy.
Technology decisions matter, but they're tactical.

Technical strategy *is*:

- Deciding what kind of engineering organization we are
  becoming.
- Identifying the technical bets that will define the next
  3-5 years.
- Creating the conditions under which engineers across the
  company make consistent technical decisions.
- Naming what we will deliberately *not* do, so that scope
  stays bounded.

A principal engineer who confuses these two levels operates
ineffectively. They feel busy but the company's engineering
direction isn't actually being shaped.

## Architecture Decision Records

ADRs are the smallest unit of technical strategy. They are
short documents that capture:

- The context for a decision.
- The decision itself.
- The alternatives considered.
- The consequences (good and bad).

ADRs are not architecture documents. They are *decision
documents*. The point is to record why a non-obvious choice
was made so that future engineers (and the future version of
you) can understand and revisit it.

Patterns that work:

- One ADR per non-trivial decision.
- Markdown in the repository, not a separate tool.
- Numbered sequentially.
- Status: proposed / accepted / superseded.
- Author named so people can ask follow-up questions.

ADRs that aren't read aren't doing their job. The principal
engineer's discipline is making sure ADRs get read — by
including them in design reviews, by referencing them in
incident postmortems, by making them part of onboarding.

## The strategy memo

For larger decisions, the principal engineer's tool is the
strategy memo. A strategy memo is longer than an ADR — five
to fifteen pages — and addresses a bet, not just a decision.

A strategy memo includes:

- The bet, stated as a bet. ("We are going to move our entire
  training stack from Horovod to PyTorch DDP over Q3 because
  X, Y, Z.")
- Why now. What's changed?
- What the alternatives are and why we're rejecting them.
- What we're risking. What's the cost of being wrong?
- How we'll know we're wrong. The kill criteria.
- The rollout plan.

A memo that doesn't have kill criteria is a wish, not a
strategy. The principal engineer's discipline is the
willingness to articulate what would make them change their
mind.

## The hardest part: judgment under uncertainty

Technical strategy is the application of judgment under
uncertainty. The principal engineer doesn't have proof; they
have a thesis. The thesis might be wrong.

Skills that pay off:

- Articulating the reasoning explicitly. Not "I think we
  should do X" but "given A and B, I believe X is the right
  bet because C."
- Pre-mortem thinking. Before committing, imagine the world
  18 months from now in which the bet failed. What would
  have caused that?
- Calibration. Track your previous predictions. Are you
  systematically over- or under-confident?
- Listening to dissent. The strongest argument against your
  bet is information you need before you commit.

## What this lecture doesn't cover

This is the orientation. Specific topics — running a
technical strategy review, the structure of effective ADRs,
working with engineering leadership on strategic alignment —
each deserve their own treatment.

For working examples and rubrics, see the principal-engineer-
solutions repo, mod-501.

## Further reading

- "Good Strategy / Bad Strategy" — Rumelt. Strategy at the
  general level; the most-recommended book on the topic.
- "The Crux" — Rumelt's follow-up.
- "Documenting Architecture Decisions" — Michael Nygard's
  blog post that started the ADR pattern.
- "Staff Engineer" — Will Larson. The closest thing to a
  staff/principal job description in the literature.
- "An Elegant Puzzle" — Larson. Engineering management
  perspective that pairs with the technical-strategy view.
