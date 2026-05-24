# Rubric — Project 01: Distributed Training Framework

Total: **100 points**. Minimum to pass: **70**. Portfolio-grade for principal interviews: **85+**.

Scoring is on a 5-level scale per dimension. A dimension's contribution = (dimension level / 4) × dimension weight.

Levels:
- **0 — Missing or unacceptable**
- **1 — Below bar** (would not pass senior-engineer level)
- **2 — Meets senior-engineer bar** (works but doesn't reflect principal-level thinking)
- **3 — Meets principal bar** (defensible, complete, ready to ship)
- **4 — Exceeds principal bar** (portfolio-grade; could appear on a promo packet or external talk)

---

## Dimension 1 — Technical Depth & Correctness (25 pts)

Does the framework do what it claims, and is the choice of *how* defensible?

| Level | Evidence |
|-------|----------|
| 0 | No working framework; or only single-GPU. |
| 1 | Multi-GPU DDP only. No FSDP/ZeRO. Toy model only. |
| 2 | FSDP works on a single node. Reference workload runs but no scale or MFU numbers. |
| 3 | FSDP **and** at least one of {DeepSpeed ZeRO-3, TP, MoE} integrated. Reference 7B model converges at ≥ 32 GPUs with ≥ 40 % MFU (or hw-adjusted). Sharding decision tree documented. ADR `0001-sharding-strategy` defends primary + secondary with model-size boundaries. |
| 4 | Three or more parallelism strategies integrated and selectable per spec. Reference 70B converges (or simulated equivalent with credible math). MFU on H100 ≥ 50 %. Optimization log shows ≥ 5 measured improvements with before/after numbers. |

**Sample evidence accepted:** `benchmarks/reference-7b.md` with loss curve PNG, MFU table, profiler trace screenshots, optimization log.

---

## Dimension 2 — Fault Tolerance & Operations (20 pts)

Does it survive real failure modes, and is the operability bar real?

| Level | Evidence |
|-------|----------|
| 0 | No checkpointing or no recovery. |
| 1 | `torch.save` on rank 0; resume only on identical world size; manual restart. |
| 2 | Sharded checkpoint, async upload, auto-restart on pod failure. Chaos suite has ≤ 3 scenarios. |
| 3 | All 10 chaos scenarios pass with recovery behavior documented. Spot preemption handled (SIGTERM → ckpt → requeue). MTTR ≤ 10 min @ 32 GPU on node loss. On-call runbook (`docs/oncall.md`) exists with ≥ 8 entries. |
| 4 | Topology-flexible resume (different world size) demonstrated. Cross-cluster migration on region outage demonstrated. Failure-mode analysis doc covers the *non-obvious* cases (slow node, partial spot loss, control-plane partition). |

**Sample evidence accepted:** `scripts/chaos/run_all.sh` output, postmortem doc, RTO/RPO table in design doc, on-call runbook.

---

## Dimension 3 — Design Doc & ADRs (20 pts)

Is the load-bearing artifact actually load-bearing?

| Level | Evidence |
|-------|----------|
| 0 | No design doc, or < 4 pages of bullet points. |
| 1 | Design doc exists but reads as implementation notes, not a decision artifact. ADRs missing or boilerplate. |
| 2 | Design doc ≥ 8 pages, covers problem + solution. 3–4 ADRs in place but shallow (no alternatives section). |
| 3 | Design doc 15–25 pages, covers all sections in `STEP_BY_STEP.md` Phase 1. **6 ADRs**, each with context + decision + alternatives + consequences + status. At least 3 named reviewers, comments addressed. |
| 4 | Design doc is publishable as an external tech blog or conference talk. ADRs include at least one that explicitly captures a decision the author would reconsider, with the trigger conditions named. Failure-model section is a reference example. |

**Sample evidence accepted:** `docs/design-doc.md`, `adr/0001..0006`, review thread (PR comments or doc comments).

---

## Dimension 4 — Cross-Team Adoption / Migration (15 pts)

Did the project create *organizational convergence*, not just code?

| Level | Evidence |
|-------|----------|
| 0 | No migration plan, no stakeholder discovery. |
| 1 | Migration plan is a high-level brochure ("Teams will migrate over the next quarter"). |
| 2 | Migration plan names 1–2 teams with current state and high-level path. |
| 3 | **3 teams** named, each with current state, step-by-step migration, rollback plan, owner, success metric, estimated effort. Stakeholder interview notes in repo. At least one signed-off review from a hypothetical (or real) adopter. |
| 4 | One adopting team actually migrated end-to-end (in this exercise or real). Adopter retro included. Migration plan covers cultural / political factors (e.g., "Team X has veto power on the K8s namespace and requires X"). |

**Sample evidence accepted:** `docs/migration-plan.md`, `docs/stakeholder-interviews.md`, adopter retro.

---

## Dimension 5 — Cost / Business Framing (10 pts)

Could this argument survive a finance partner?

| Level | Evidence |
|-------|----------|
| 0 | No cost model. |
| 1 | One-liner ("saves money"). No numbers. |
| 2 | Cost spreadsheet with per-job cost. No comparison vs status quo. |
| 3 | Cost model with separated assumptions + outputs. 12-month savings projection that a manager could defend to finance. Executive summary ≤ 1 paragraph that a non-engineer can read. Per-team budget enforcement implemented in code. |
| 4 | Multiple scenarios (conservative / base / aggressive). Sensitivity analysis on key assumptions (spot mix, MFU achieved, GPU price evolution). Cost model integrated with the framework so jobs auto-emit cost telemetry. |

**Sample evidence accepted:** `docs/cost-model.xlsx` or `.ipynb`, executive summary in design doc, code in `control/budget.py`.

---

## Dimension 6 — Communication (Tech Talk + Writing) (10 pts)

Can you explain what you built, and what you learned?

| Level | Evidence |
|-------|----------|
| 0 | No talk, no docs beyond a stub README. |
| 1 | Talk exists but is < 15 min or reads off slides. Docs are stubs. |
| 2 | Talk 20–40 min, recorded, comprehensible. README + getting-started exist. |
| 3 | Talk 30–40 min, well-structured (problem / solution / two deep dives / lessons / Q&A). README is opinionated; getting-started gets a new user to a running job in ≤ 30 min. Troubleshooting doc with ≥ 10 entries. Slide deck checked in. |
| 4 | Talk accepted (or credibly submittable) to an external venue. Includes a "what I'd do differently" slide grounded in observed behavior. Writing across docs is consistent in voice, tight, and free of fluff. |

**Sample evidence accepted:** `talks/tech-talk.mp4` (or link), `talks/slides.pdf`, `README.md`, `docs/getting-started.md`, `docs/troubleshooting.md`.

---

## Scoring Worksheet

```
Dimension                             Weight   Level (0–4)   Subtotal
Technical depth & correctness          25         ___         ___ × 25/4 = ___
Fault tolerance & operations           20         ___         ___ × 20/4 = ___
Design doc & ADRs                      20         ___         ___ × 20/4 = ___
Cross-team adoption / migration        15         ___         ___ × 15/4 = ___
Cost / business framing                10         ___         ___ × 10/4 = ___
Communication (talk + writing)         10         ___         ___ × 10/4 = ___
                                                              ─────────────
                                                  TOTAL:      ___ / 100
```

---

## Calibration Notes for Reviewers

- A "Level 3" project is **publishable internally** at a real company. It should not feel like a learning exercise.
- A "Level 4" project should feel like the artifact a principal engineer would link from their promo packet.
- A common error: scoring "Level 4" for high effort that didn't actually produce the artifact (e.g., "spent 30 hours on chaos but only 3 scenarios pass"). Score on **outcome**, not hours.
- A common error in the other direction: penalizing absence of polish (typos, broken Markdown) when the substance is principal-grade. Polish matters, but Dimension 6 catches it; do not double-count.
- Honest "I'd do this differently" content **adds** points, doesn't subtract. Principal engineers admit mistakes in public.

---

## Self-Assessment Before Submission

Before handing this in for review, score yourself. If your honest self-score is below 70 in any single dimension, fix that dimension before submitting — don't let a reviewer be the one who tells you.
