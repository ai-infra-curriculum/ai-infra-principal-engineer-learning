# Step-by-Step Build Guide — Project 04: Technical Innovation POC

This is an **80-hour, 4-week build plan** at full-time pace, or ~8 weeks at 10 h/week. Phases are sequential and **time-boxed** — the principal-level skill this project teaches is finishing on time with a defensible recommendation.

Each phase has: **goal**, **day-level breakdown**, **validation gate**, and **gotchas** drawn from real POC scars.

---

## Phase 0 — Pre-Work (3 h, before Week 1)

### Goal
Confirm environment, shortlist candidate techniques, and identify the org-level question.

### Tasks
1. **Identify the org-level question.** Write one sentence: "Should we adopt X for Y purpose in the next 12 months?" If you can't write it without filler words, you haven't picked yet.
2. **Shortlist 3 candidate techniques** that could answer it. Capture one paragraph each on novelty, hardware reach, expected magnitude, productionization difficulty.
3. **Hardware sanity check.** For each candidate, confirm you have the hardware (e.g., FP8 wants H100; MoE inference wants enough total GPU memory; agentic pipelines want a model serving budget).
4. **Read the primary references** for the top candidate. Skim the others. Note the paper's hardware, their workload, their baseline.
5. **Stand up a runnable repo skeleton** with `pyproject.toml`, `Makefile`, CI hook, and a placeholder `docs/charter.md`.

### Gate
You can articulate, in two minutes:
- The org-level question
- Three candidate techniques and why you'd rank them
- The hardware and workload realities that frame the comparison

### Gotchas
- "Cool technique" is not "right technique for the org." Reject candidates that are interesting but not relevant.
- Read the paper's "limitations" section before the "results" section. That's where most of the actual signal lives.
- If the paper's hardware is two generations newer than yours, the extrapolation may be honest but it will be unsatisfying for the reviewer.

---

## Week 1 — Selection, Pre-Registration, Baseline, Design Doc Draft 1 (16 h)

### Day 1 (4 h) — Technique selection + charter

Write `docs/technique-selection.md`: rank the three candidates with one paragraph per dimension (relevance, novelty, hardware, expected magnitude, productionization difficulty, time-box feasibility). Pick the winner. State why explicitly.

Write `docs/charter.md` (≤ 1 page):
- Technique name
- The org-level question
- The 80-hour time box
- The named recommender (you)
- The (real or hypothetical) sponsor who will receive the recommendation

### Day 2 (4 h) — Success criteria pre-registration

Write `docs/success-criteria.md` using the schema in `architecture.md` §3. Commit it. **Do not modify it after experiments begin** — if it turns out to be wrong, that's a methodology lesson for the report.

Include:
- Primary metrics (≥ 2) with baseline, go-threshold, no-go-threshold, not-yet band
- Secondary metrics (correctness, accuracy, cost)
- Stress conditions (≥ 3)
- Decision matrix for mixed outcomes

Tag the commit explicitly (`pre-registration-v1`).

### Day 3 (4 h) — Baseline measurement

- Stand up the baseline path: the system as it exists today, no technique applied.
- Run the harness against the baseline. Compute mean, std, 95 % CI for each primary metric.
- Run the baseline twice back-to-back to estimate the noise floor.
- If noise floor > expected effect, fix noise before continuing (shared host? DVFS? warmup?).
- Commit `benchmarks/baseline.md` with numbers and the workload spec.

### Day 4 (2 h) — Design doc skeleton

Design doc sections (10–18 pages eventually):
1. Problem + org-level question
2. Technique chosen + why
3. Pre-registered success criteria
4. POC implementation plan
5. Harness design
6. Stress-test matrix
7. Productionization gap framing
8. Recommendation framing (template for go / no-go / not-yet)
9. Risks + open questions
10. Time-box plan

Write at least 5 pages of draft 1. Specifically write the recommendation **template** now — i.e., what the recommendation section will look like with the data plugged in.

### Day 5 (2 h) — Peer review + ADR stubs

Send the charter + selection + success criteria + baseline to at least one reviewer (technical peer or sponsor). Iterate before implementation.

Write 5 ADR stubs:
- `adr/0001-technique-choice.md`
- `adr/0002-measurement-methodology.md`
- `adr/0003-harness-design.md`
- `adr/0004-integration-boundary.md`
- `adr/0005-recommendation-framing.md`

### Validation gate
- [ ] `docs/technique-selection.md` compares ≥ 2 alternatives
- [ ] `docs/charter.md` ≤ 1 page
- [ ] `docs/success-criteria.md` committed with `pre-registration-v1` tag *before* implementation
- [ ] Baseline measured with 95 % CI; noise floor reported
- [ ] Design doc draft 1 (≥ 5 pages) committed
- [ ] 5 ADR stubs
- [ ] One external reviewer has commented

### Gotchas
- Pre-registration that gets quietly edited mid-project is the cardinal sin. Use commit tags to make it auditable.
- A baseline measured on a noisy shared cluster will swamp every experiment. Fix the platform first.
- The temptation to pick the "cooler" technique over the "more useful" one is real. Pick useful.

---

## Week 2 — POC Implementation + First Experiments + Harness (20 h)

### Goal
The POC runs end-to-end at credible scale; the harness produces structured output; the first 5 experiments are in the log.

### Day 6 (5 h) — POC scaffolding + flag

- Implement the technique in `src/poc/`.
- Whenever possible, wrap a reference implementation rather than rewriting from scratch — note clearly what's yours vs upstream.
- Make the technique **toggleable via a single flag** so baseline + treatment runs differ in one bit.
- Commit a "smoke" run that exercises the technique on a small example end-to-end.

### Day 7 (5 h) — Harness implementation

- Build the harness per `architecture.md` §4.
- Two-arm by default; alternate arms; fixed seed; warmup discarded.
- Output structured JSON; render Markdown report from JSON.
- Test that running the harness with `--technique=off` and `--technique=on` gives differentiable outputs.

### Day 8 (4 h) — First experiments (within paper's regime)

Run experiments 1–5 at the paper's nominal regime (similar workload, similar hardware where possible). For each:
- Hypothesis (one sentence)
- Run the harness
- Record before/after with CI
- Decision (continue / pivot / kill)
- Commit log row

### Day 9 (3 h) — Correctness validation

For inference techniques:
- Run a held-out accuracy harness (MMLU / HellaSwag / domain-specific) against baseline + technique. Document accuracy diff.
- Run a logits-diff test on a fixed prompt corpus. Threshold per ADR.

For training techniques:
- Train baseline + treatment for matched-seed K steps. Compare loss curves.
- Convergence smoke against a checkpoint milestone.

For pipeline techniques:
- Define a task-success metric (exact-match, judge-model score, etc.).
- Measure on a representative task set.

### Day 10 (3 h) — Mid-week review + plan ahead

- Re-read pre-registered success criteria.
- Are you on track for `go`, `no-go`, or `not-yet`? State your current bet honestly in `docs/current-bet.md` (this is a midpoint snapshot, not the final).
- Identify the stress conditions you'll exercise next week.
- Update the experiment plan for week 3.

### Validation gate
- [ ] POC runs end-to-end via `make poc`
- [ ] Harness produces JSON + Markdown
- [ ] ≥ 5 experiments in the experiment log
- [ ] Correctness validation passes (or documented why it doesn't)
- [ ] Mid-week current-bet committed
- [ ] Pre-registration unchanged

### Gotchas
- "It works on the example" rarely generalizes to "it works at the workload distribution." Run on realistic distributions early.
- A toggle flag that introduces subtle differences in code paths invalidates the comparison. Verify the only difference is the technique.
- Don't tune the technique in this phase — that's mid-campaign rationalization. Run it as-published first; tuning is later.

---

## Week 3 — Stress Tests, Scale-Up, Productionization Gap Analysis (22 h)

### Goal
Find where the technique breaks. Estimate what production adoption would cost.

### Day 11 (5 h) — Stress conditions 1 + 2

From your pre-registered stress matrix, pick the two most likely to break the technique. Run experiments 6–9.

For inference techniques: long-context (prompt p99 ≥ 2048), saturation (QPS at fleet limit), bimodal workload (chat + bulk).

For training techniques: extreme sequence length, mixed-precision interactions, larger world size than paper.

For pipeline techniques: adversarial inputs, long task chains, out-of-distribution requests.

Document each: hypothesis ("we expect a 30 % degradation"), actual outcome ("got 45 % degradation"), root cause if known.

### Day 12 (5 h) — Stress condition 3 + scale-up

Run experiment 10–12 at a scale closer to production:
- For inference: full request distribution, full concurrency, full duration (≥ 30 min).
- For training: scale world size up or down by 2×; observe which wins scale.
- For pipeline: realistic task volume, realistic agent chain depth.

Re-baseline at this scale. Headline result is computed against the **production-scale baseline**, not the small-scale one.

### Day 13 (4 h) — Negative experiments + write-ups

You will have tried things that didn't work. Keep them.
- Document at least 2 negative experiments with hypothesis-of-why-not.
- Negative findings are often the most useful part of a POC — they prevent future engineers from re-trying the same dead end.

### Day 14 (4 h) — Productionization gap analysis

Walk through the gap categories in `architecture.md` §6. Per category, list the missing pieces with the schema:
- Description
- Effort engineer-weeks
- Owning team (real or hypothetical)
- Priority
- Dependencies on other gaps
- Risk if not closed

Draw the gap dependency graph (Mermaid).

Commit `docs/productionization-gaps.md`.

### Day 15 (4 h) — Confidence calibration

For each primary metric:
- State your current best estimate
- State your 95 % CI for the estimate
- State the conditions that would shift the estimate
- State your overall confidence in the recommendation as a probability (e.g., 0.7 for `go`)

Commit `docs/confidence.md`. This becomes a section in the POC report.

### Validation gate
- [ ] ≥ 12 experiments total
- [ ] At least 3 stress experiments
- [ ] At least 2 negative experiments
- [ ] Headline result computed against production-scale baseline
- [ ] `docs/productionization-gaps.md` with dependency graph
- [ ] `docs/confidence.md` with calibrated estimates
- [ ] Pre-registration still unchanged

### Gotchas
- Stress conditions that "almost worked" tempt you to re-tune. Note the temptation; don't yield to it within the time box.
- Productionization gaps that say "small effort" without an estimate are not useful. Force yourself to put a number.
- Calibration is hard. Look up your past forecasts (or have a peer challenge yours) to spot systematic over- / under-confidence.

---

## Week 4 — Recommendation, POC Report, Tech Talk, Hand-Off (22 h)

### Goal
The story lands. The recommendation is unambiguous. The next team can pick up cold.

### Day 16 (4 h) — Recommendation document

Write `docs/recommendation.md` per the structure in `architecture.md` §7:
- Recommendation: `go` / `no-go` / `not-yet`
- Confidence: probability
- Summary paragraph
- Primary findings
- For `not-yet`: explicit observable trigger conditions
- Alternative if not now
- What would change my mind
- Owner after this project
- Next decision point

Have a peer read it. If they can't restate the recommendation in one sentence, rewrite.

### Day 17 (5 h) — POC report

Write `docs/poc-report.md` (8–14 pages). Structure:
1. **Recommendation** (opening — not buried)
2. Org-level question + technique chosen
3. Pre-registered success criteria (with `pre-registration-v1` tag)
4. Baseline + final results table
5. Experiment campaign summary
6. Stress-test findings
7. Productionization gap analysis (summary; details in separate doc)
8. Confidence + what would change my mind
9. Risks + open questions
10. Hand-off

Numbers come from the harness JSON, not retyped.

### Day 18 (4 h) — Tech talk

25–40 min recorded. Outline:
1. Org-level question (3 min)
2. Technique + why we chose it (3 min)
3. Pre-registered success criteria (2 min)
4. Top 3 findings (10 min)
5. Where it breaks — stress results (5 min)
6. Recommendation + confidence (5 min)
7. What surprised us (3 min) — POCs that don't surprise are shallow
8. Hand-off (2 min)
9. Q&A

Record, watch, re-record once.

### Day 19 (4 h) — Hand-off package

Write:
- `docs/hand-off.md`: who owns next, first 30 days outlined, escalation contact
- `docs/where-to-pick-up.md`: a cold-start guide for an engineer landing on the project. Repo tour, "if you only read one file read this", common pitfalls
- `docs/upstream-prs.md`: any patches you'd want upstreamed; status

Hand the package to a peer. Have them spend 30 minutes simulating "I'm picking this up Monday." Address their friction points.

### Day 20 (3 h) — ADRs polish + repo hygiene

Polish the 5 ADRs from Week 1. Each contains: context, decision, alternatives, consequences, status, date, deciders.

Final lint pass:
- File size, function size, type checks
- README quickstart works
- `make poc` and `make harness` both work from `git clone`

### Day 21 (2 h) — Self-assessment + final reviewer

`docs/self-assessment.md`: per-rubric-dimension scores with rationale.

Identify your weakest dimension and write a paragraph defending why you made the trade.

Send to at least three reviewers (technical peer, sponsor, productionization team representative). Address their comments.

### Validation gate
- [ ] `docs/recommendation.md` unambiguous (go / no-go / not-yet) with calibrated confidence
- [ ] POC report 8–14 pages; opens with recommendation
- [ ] Tech talk recorded; slides committed
- [ ] Hand-off package + where-to-pick-up complete
- [ ] 5 ADRs merged
- [ ] Self-assessment committed
- [ ] At least 3 reviewers acknowledged

---

## Final Checklist Before Submitting

Tick every box. Each maps to an acceptance criterion in `requirements.md`.

- [ ] `docs/technique-selection.md` compares ≥ 2 alternatives
- [ ] `docs/charter.md` ≤ 1 page
- [ ] `docs/success-criteria.md` pre-registered (commit tag) *before* implementation
- [ ] Baseline measured on your stack with 95 % CI + noise floor
- [ ] POC reproducible by reviewer via `make poc` within documented budget
- [ ] Harness produces structured JSON + Markdown reports
- [ ] ≥ 12 experiments in `experiment-log.md`
- [ ] ≥ 3 stress experiments outside paper's regime
- [ ] ≥ 2 negative experiments kept
- [ ] `docs/productionization-gaps.md` with engineer-week estimates + dependency graph
- [ ] `docs/recommendation.md` unambiguous with calibrated confidence + trigger conditions
- [ ] `docs/hand-off.md` + `docs/where-to-pick-up.md` complete
- [ ] POC report 8–14 pages; opens with recommendation
- [ ] Tech talk 25–40 min recorded; slides committed
- [ ] 5 ADRs merged
- [ ] No file > 800 LOC

---

## Common Failure Modes — Read Before You Start

1. **Wrote success criteria after the experiments.** You said "go" because the numbers came in good. Fine, but the reviewer notices the commit timestamps, and the rest of the project loses credibility.
2. **Picked the cool technique, not the useful one.** Mamba is interesting; your org doesn't care about long-context. Wasted POC.
3. **Reproduced the paper's headline, never stressed it.** "It works!" — yes, in their regime. Your reviewer asks about your regime; silence.
4. **No correctness check.** "1.6× throughput!" — also: silent accuracy drop the model team would have caught in eval. Credibility tanks.
5. **Implementation is a fork that diverges from upstream.** No clear hand-off path. Productionization team won't touch it.
6. **No baseline on production-scale.** Headline number is from a 100M-parameter toy. Reviewer asks: "what does that mean for our 70B?" — extrapolation hand-waving.
7. **Recommendation buried at page 12.** Reviewer reads page 1 and skims. They miss the recommendation. They make their own. You've lost.
8. **"High confidence."** A reviewer asks: "what would change your mind?" Silence. Confidence wasn't calibrated; it was signaled.
9. **No productionization gap analysis.** "Go" with no estimate of cost. Sponsor can't act on the recommendation.
10. **No hand-off.** Project sits in a personal repo. Two months later the technique advances; nobody knows you did the work.

---

## What "Good" Looks Like at the End

A reviewer with 1 hour should be able to:
1. Read the recommendation paragraph and quote it correctly.
2. Verify pre-registration via commit timestamps.
3. Run `make poc` and see the headline result reproduce.
4. Pick an experiment ID from the log and inspect the corresponding `repro/`.
5. Open `docs/productionization-gaps.md` and find the dependency graph.
6. Read `docs/confidence.md` and understand the trigger conditions for `not-yet`.
7. Watch 5 minutes of the talk and follow the narrative.

If a reviewer can do all 7 — you've passed. If the **sponsor** would act on the recommendation today — you've hit 85+.
