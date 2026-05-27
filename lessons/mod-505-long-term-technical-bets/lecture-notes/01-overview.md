# Lecture 01: Long-Term Technical Bets — Overview

## The unforgiving asymmetry

Most engineering decisions have payback in weeks to months.
Long-term bets pay back in years. The asymmetry matters:

Short-term decisions are forgiving. If you're wrong, you
discover it fast and pivot. The cost is bounded.

Long-term decisions are unforgiving. If you're wrong, you
discover it after you've already invested years and the
alternatives have moved on. The cost is the years.

Principal engineers are the people in the room expected to
read the technical landscape clearly enough to make these
bets well. This lecture is the orientation: what long-term
bets actually look like and how to make them with calibrated
discipline.

## What counts as a long-term bet

A long-term bet is a technical commitment with payback
horizon ≥ 18 months that, if wrong, costs significant
resources to reverse.

Examples:

- The decision in 2002 by Amazon to build internal services
  as a public cloud.
- The decision in 2013 by Google to invest in TPUs.
- The decision in 2018 by many companies to "go Kubernetes."
- The decision in 2022 by many ML teams to bet on transformer-
  based foundation models for everything.

Some bets paid off massively. Some paid off partially. Some
were wrong. The principal engineer's job isn't to make
infallible bets — it's to make calibrated bets where the
upside justifies the downside risk.

## The shape of a well-formed bet

A long-term bet is well-formed when it includes all of these:

**The trend.** What's changing in the world that makes this
bet possible or necessary? Specific evidence — not "AI is
big" but "inference cost has dropped 10x in 24 months and
will likely drop another 5x in 24 more, which changes the
unit economics of customer-facing AI features."

**The implementation.** The specific commitment you're
making. "We will build our serving stack around vLLM" is
distinct from "We will invest in LLM serving infrastructure."

**The horizon.** When do you expect to see payoff? 18
months? 5 years? Bets without horizons drift.

**The investment.** Headcount, capital, opportunity cost.
Be honest. Bets that "use existing resources" usually use
more than expected.

**The kill criteria.** What data, observed when, would make
you stop? This is the hardest part. Many bets fail because
nobody specified what failure looked like.

**The portfolio context.** What other bets is the company
making? Is this complementary or in conflict?

## Bets in portfolios

A company making one long-term bet is fragile. A company
making zero is stagnant. The right shape is a portfolio:

- 1-2 large bets (significant investment, multi-year
  horizon, strategically defining).
- 3-5 medium bets (moderate investment, 12-24 month
  horizon).
- A pipeline of small experiments (small investment,
  short horizon, feeder for medium bets).

The portfolio is balanced across:

- Time horizon (some payoff sooner, some later).
- Risk level (some safer, some longer odds).
- Domain (not all bets in one area).

The principal engineer's job, with the company's senior
engineering leadership, is *shaping* the portfolio.

## Re-evaluation as the underrated discipline

Most bets are revisited too rarely. The reasons are human:
re-evaluation is uncomfortable; admitting a bet isn't
working is harder than continuing.

The pattern that works:

- Every long-term bet has an explicit re-evaluation date
  in its memo (typically every 6 or 12 months).
- The re-evaluation produces a written update: progress
  against original thesis, what's changed, what should
  change in the plan.
- The re-evaluation includes the kill criteria check —
  are any of them triggered?
- The re-evaluation is calendared at commitment time. Not
  scheduling it makes it not happen.

A bet that's never been revisited has either succeeded
without anyone noticing (rare) or it's drifted from the
original thesis (common).

## When to kill a bet

Killing a bet is the hardest principal-engineer move. The
psychological pull is to continue — sunk cost, public
commitment, hope. The discipline is recognizing the signal.

Concrete signals that a bet should die:

- The original trend turned out to be different from what
  you projected.
- The team executing has lost the people who originally
  championed it.
- The competitive landscape has moved in ways that make
  the bet less valuable.
- The kill criteria have been triggered and the team is
  rationalizing why they don't count.

A bet killed honestly preserves the company's ability to
make future bets. A bet kept on life support indefinitely
trains the organization to distrust bets in general.

## What this lecture doesn't cover

This is the orientation. Specific topics — how to structure
a strategy memo for a long bet, how to run the re-evaluation
cycle, how to negotiate kill criteria with stakeholders —
each deserve their own treatment.

For working examples and rubrics, see the principal-engineer-
solutions repo, mod-505.

## Further reading

- "Good Strategy / Bad Strategy" — Rumelt. The strategy
  foundation.
- "Antifragile" — Taleb. How to design under uncertainty.
- "The Innovator's Dilemma" — Christensen. Why incumbent
  companies miss bets they should have seen.
- Annual letters from CEOs of AI-leading companies — the
  bet structure is often visible.
