# Lecture 01: Open Source and Community Influence — Overview

## Why this matters for principal engineers

In 2026, modern AI infrastructure is built almost entirely on
open source. PyTorch, vLLM, Kubernetes, Ray, MLflow,
LangChain, FastAPI — these are the substrate of every serious
ML platform. A principal engineer who can't navigate the OSS
world is operating with one hand tied.

This isn't only about consuming OSS. It's about:

- Engaging deeply with the projects you depend on.
- Contributing back when the engineering value is real.
- Releasing your own OSS where it makes strategic sense.
- Participating in foundation governance for the projects
  that matter.
- Building personal credibility in the community.

This lecture is the orientation.

## OSS engagement as a portfolio

A company's OSS engagement is a portfolio of relationships,
not a single decision. For each major project you depend on,
you sit in one of these modes:

**Consume.** You use the project but contribute nothing back.
Appropriate for projects that are well-maintained, broadly
used, and not strategic to your differentiation.

**Fix and PR.** You file bug fixes and small improvements
upstream as you find issues that affect you. Appropriate for
projects where you're a significant user but not central to
their direction.

**Co-maintain.** You have engineers contributing
substantially on an ongoing basis. Appropriate for projects
that are critical to your platform.

**Sponsor.** You provide financial support to project
maintainers. Appropriate for projects you depend on where
your contributors don't fit the project's needs.

**Lead.** You drive a project's direction (whether your own
project or by majority-contribution to someone else's).
Appropriate for projects where you're shaping a strategic
ecosystem.

Most companies muddle these modes. They free-ride on
critical dependencies and over-invest in projects that
aren't strategic.

## When to release your own project

The decision to release company-internal code as a new OSS
project is high-stakes. The downside cases dominate:

- Released code with no maintenance commitment becomes a
  zombie that hurts your company's reputation.
- Released code with poor design embarrasses your team for
  years.
- Released code that competes with existing projects
  splinters the community.
- Released code that conflicts with your strategic interests
  gets used in ways you didn't anticipate.

The upside cases that justify release:

- Standard-setting. The project becomes the canonical
  implementation of an emerging category, and your team
  gains the influence that comes with it.
- Recruiting. The project is visible enough to attract
  talent who recognize the work.
- Ecosystem building. Adjacent businesses build on top of
  your project, expanding the value of the core platform.
- Strategic positioning. The project commoditizes a layer
  that competitors had differentiation in.

Release is a 5-10 year commitment. Treat it accordingly.

## Foundation participation

For projects in foundations (CNCF, Linux Foundation, ASF,
LF AI & Data), participation in the foundation's governance
is its own decision.

What you get:

- Insight into the project's direction earlier than other
  participants.
- The ability to nominate technical leadership.
- A voice in technical decisions that affect your roadmap.
- Recruiting visibility.

What it costs:

- Annual membership fees ($30k-$300k depending on tier).
- Engineering time at TSC / TOC meetings.
- The political work of building relationships with other
  member companies.

The principal engineer's job is making sure the company is
*using* its foundation membership rather than passively
paying for it.

## Personal credibility

Beyond company-level OSS strategy, the principal engineer's
own community standing matters. The patterns that build it:

- Sustained contribution to a few projects beats sporadic
  contribution to many.
- Public writing (conference talks, blog posts, papers)
  about real engineering work compounds over time.
- Helping others (reviewing PRs, answering questions, mentoring
  new contributors) builds the kind of credit that's hard to
  earn other ways.
- Being known as a person who tells the truth — including
  uncomfortable truths — distinguishes principal engineers
  who get listened to from those who don't.

## What this lecture doesn't cover

This is the orientation. Specific topics — running an OSS
release process inside a company, structuring foundation
participation, managing personal community visibility — each
deserve their own treatment.

For working examples and rubrics, see the principal-engineer-
solutions repo, mod-504.

## Further reading

- "Working in Public" — Nadia Eghbal. The economics and
  social dynamics of OSS.
- The CNCF TOC charter and the recent governance posts —
  how foundation projects actually run.
- "How To Open Source" (the GitHub guide) — the practical
  layer.
