# Step-by-Step Build Guide — Project 01: Distributed Training Framework

This is a **100-hour, 5-week build plan** at full-time pace, or ~10 weeks at 10 h/week. Phases are sequential — do not parallelize until you've hit the gate at the end of the prior phase.

Each phase has: **goal**, **day-level breakdown**, **validation gate** (what reviewers and you check before moving on), and **gotchas** drawn from real production scars.

---

## Phase 0 — Pre-Work (3 h, before Week 1)

### Goal
Stand up the environment and confirm you can spell every word in the project before you start.

### Tasks
1. Provision (or confirm access to) ≥ 8 GPUs you control end-to-end. Confirm `nvidia-smi`, `nccl-tests`, `dcgmi` all work.
2. Verify NCCL on your interconnect: run `nccl-tests/all_reduce_perf` and **record the bandwidth number** — you will refer back to this many times.
3. Skim the FSDP, DeepSpeed ZeRO, and Megatron-LM papers. Skim `torch.distributed.checkpoint` source.
4. Read 2–3 prior team retrospectives or postmortems from the ML training stack you'll be replacing (or invent realistic ones if this is a personal project).

### Gate
You can answer, without notebook: "What is the theoretical peak BF16 FLOPS of one of my GPUs, what is my measured NCCL all-reduce bandwidth, and what is my expected MFU ceiling for a 7B model given those two numbers?" If you can't, do the math now — every later phase depends on it.

### Gotchas
- "Access to GPUs" is not the same as "GPUs that work for distributed training." A single A100 in a notebook is not a substitute. If your shared cluster requires queueing, plan around that.
- NCCL silently falls back to TCP if it can't find IB / RoCE / NVLink. Always log `NCCL_DEBUG=INFO` for the first run on any new environment.

---

## Week 1 — Discovery + Design Doc Draft 1 (18 h)

The most senior engineer mistake on a project like this is to start coding. Don't.

### Day 1 (4 h) — Stakeholder discovery

Interview (or, for personal projects, write user stories for) **at least 3 hypothetical teams** that will adopt the framework. For each capture:
- Current setup (cluster, framework, scale, model size)
- Top 3 pain points
- Current MTTR for a failed training job
- What would make them refuse to migrate

Output: `docs/stakeholder-interviews.md` (~1–2 pages). This becomes appendix material in your design doc.

### Day 2 (4 h) — Design doc skeleton

Use a real template (Google "design doc template" — pick one). Sections at minimum:
1. Problem statement (what's broken today)
2. Goals + non-goals
3. Proposed solution (high level)
4. Architecture (components + data flow)
5. Major decisions (will be ADRs)
6. Failure model (RTO/RPO table)
7. Migration plan (high level)
8. Cost model (high level)
9. Open questions
10. Appendix (stakeholder notes, benchmarks)

Write the **problem statement and goals first**. If you can't write a one-paragraph problem statement that a director would nod at, you don't yet know what you're building.

### Day 3 (4 h) — Decision menu

For each of these, write a 2–4 sentence position:
- Sharding library (FSDP / DeepSpeed / Megatron / mix)
- Checkpoint approach (custom / `torch.distributed.checkpoint` / DeepSpeed's)
- K8s integration (CRD operator / Job + scripts / Karmada)
- Job spec format (YAML / Python / both)
- Comms backend (NCCL only / NCCL + UCC)
- Observability (Prometheus / OpenTelemetry / mixed)

These become your first 6 ADR stubs in `adr/`.

### Day 4 (3 h) — Architecture diagrams

Draw the 4 mandatory diagrams (system context, component map, training-step sequence, multi-cluster sequence). Use Mermaid or excalidraw, but **commit them to the repo**.

### Day 5 (3 h) — Peer review of design doc draft 1

Get the doc in front of at least one experienced engineer. Use their feedback to refactor before you write any framework code.

### Validation gate
- [ ] Design doc draft 1 (≥ 8 pages) exists
- [ ] Stakeholder interviews captured
- [ ] 4 architecture diagrams committed
- [ ] 6 ADR stubs (titled, status=proposed)
- [ ] At least one external reviewer has commented

### Gotcha
- Do not let yourself draft the doc in isolation for two weeks. Get it reviewed at draft 1, not draft 5.

---

## Week 2 — Core Framework (22 h)

### Goal
A minimal control plane + a runtime that can train a small model with FSDP on a single node, end-to-end, through your API.

### Day 6 (4 h) — Project scaffolding
```
src/training_framework/
  api/            # FastAPI submission service
  cli/            # mlctl
  control/        # planner, scheduler, lifecycle, state_store
  runtime/        # bootstrap, parallelism, ckpt, metrics
  backends/
    k8s/          # operator + CRD
    slurm/        # agent
  shared/         # schemas, config, errors
tests/
adr/
docs/
scripts/
```

Set up: `pyproject.toml`, `mypy.ini --strict` on `runtime/` and `shared/`, `ruff`, `pytest`, pre-commit. CI on GitHub Actions running tests + mypy on every push.

### Day 7 (4 h) — Job spec schema + validator
Define `JobSpec` Pydantic v2 model. Cover: model entrypoint, dataset, parallelism, resources, ckpt policy, budget. Write unit tests for happy path + 8 invalid cases.

### Day 8 (4 h) — Submission API + state store
FastAPI endpoint `POST /jobs`. Persist to Postgres (use `sqlite` in tests, real Postgres in dev). Status query endpoint. OIDC stub for now (just header check); real OIDC comes in Week 5.

### Day 9 (5 h) — K8s operator skeleton
Use `kopf` or `kubernetes` client. Watch a `TrainingJob` CRD. Materialize a `PyTorchJob`-style set of pods (or write your own; PyTorchJob operator from Kubeflow is fine to wrap).

Hard part: rendezvous. Use `c10d` `etcd` rendezvous or the K8s headless-service approach with `c10d::TCPStore`. Pick one, document why.

### Day 10 (5 h) — Runtime: FSDP wrap + dummy train loop
Write `runtime.init(spec)` that:
- Reads rank/world from env
- Sets up NCCL with `NCCL_DEBUG=WARN` by default
- Wraps a user-supplied `nn.Module` in FSDP with sensible defaults (mixed precision BF16, transformer auto-wrap policy)
- Returns a context with `model`, `optimizer`, `lr_scheduler`, `ckpt`

Reference workload at this stage: a tiny GPT (~100M params) on synthetic data. The goal is the plumbing, not the science.

### Validation gate
- [ ] `mlctl submit examples/tiny-gpt.yaml` runs end-to-end on 1 node, ≥ 2 GPUs
- [ ] State store shows the job lifecycle (queued → running → succeeded)
- [ ] At least 30 unit tests, ≥ 80 % coverage on `shared/` + `control/state_store`
- [ ] CI green

### Gotchas
- FSDP defaults change between PyTorch versions. Pin a version in `requirements.txt` and document it.
- Rendezvous failures look like "training hangs forever". Always set a `--rdzv-timeout` and surface it.
- Don't put model code in the framework. Users supply the model; you supply the harness.

---

## Week 3 — Checkpoint, Fault Tolerance, Observability (22 h)

### Goal
Survive failures gracefully and let an on-call engineer see what's happening.

### Day 11 (5 h) — Checkpoint engine
- Sharded write per rank to local NVMe.
- Async uploader (`asyncio` + `aioboto3` or `aiofiles` + a worker thread).
- Manifest written last, contains `world_size`, `step`, `shard_sha256[]`.
- Resume path: read manifest → fetch shards → load state dict (FSDP `load_sharded_state_dict`).
- Unit test: write → corrupt one shard → confirm resume picks previous good ckpt.

### Day 12 (4 h) — Fault detector + lifecycle retries
- Per-rank heartbeat to control plane every 30 s.
- Stall detector: if no step progress for `3 × p99(step_time)` over the last 100 steps, mark unhealthy.
- Lifecycle: on pod failure event from K8s, increment retry, resume from last good ckpt, max retries from spec.

### Day 13 (4 h) — Spot preemption handling
- Trap SIGTERM in the runtime.
- On signal: write synchronous checkpoint, exit with code 75 (a soft-fail code your operator recognizes as "re-queue, don't count as failure").
- Operator on code 75: re-create the K8s Job, same job_id, same priority, ckpt resume path set.

### Day 14 (5 h) — Metrics + dashboards
- Implement `metrics.py` emitter using `prometheus_client`. Cover the metrics listed in `architecture.md` §7.
- Add a DCGM exporter sidecar to the worker pod template.
- Ship 3 Grafana dashboards as JSON in `monitoring/grafana/`. Test locally with a Prometheus + Grafana docker-compose.

### Day 15 (4 h) — Structured logging
- JSON logger with mandatory fields.
- Per-step log at INFO is too noisy; sample 1/100 by default, configurable.
- Test: tail logs of a tiny job, confirm `job_id`, `rank`, `step` present on every line.

### Validation gate
- [ ] Kill a worker pod mid-training; job auto-resumes within 5 min on 8 GPUs
- [ ] Send SIGTERM to a worker; job writes ckpt cleanly, re-queues, resumes
- [ ] Grafana "Job overview" dashboard shows live metrics for an active job
- [ ] Corrupt a checkpoint manifest; framework falls back to previous ckpt automatically

### Gotchas
- Async upload buffers can OOM the node on large checkpoints — bound the queue length.
- `prometheus_client` from a multi-process / multi-rank context needs `multiprocess` mode or per-rank `:port`. Pick one.
- DCGM exporter requires the GPU operator's monitoring component; don't assume it's there.

---

## Week 4 — Reference Workload, Chaos, ADRs (20 h)

### Goal
A non-toy workload converges, a chaos suite passes, and the load-bearing decisions are written up.

### Day 16 (4 h) — Reference 7B model
Use the LLaMA-style reference impl from `torchtitan`, `nanotron`, or your own. Use a small public dataset (a slice of C4, the Pile, or even synthetic). Get loss curves you trust.

### Day 17 (4 h) — Scale to 32+ GPUs
- Multi-node FSDP. Sort out the rendezvous + NCCL on a real fabric.
- Tune `forward_prefetch`, `backward_prefetch`, `sharding_strategy=FULL_SHARD` vs `HYBRID_SHARD`.
- Target ≥ 40 % MFU on H100 (or hardware-adjusted equivalent).
- Record the numbers in `benchmarks/reference-7b.md`.

### Day 18 (4 h) — MFU optimization pass
- Use the PyTorch profiler + nsys for one representative step.
- Look for: small kernels, lost overlap of comms and compute, host-side stalls, suboptimal attention impl.
- Try FlashAttention-2 (or 3 if available) and re-measure.
- Document each change as a row in `benchmarks/optimization-log.md`.

### Day 19 (4 h) — Chaos test suite
Implement under `scripts/chaos/`:
1. Pod kill (random worker)
2. Node drain (cordon + drain)
3. NCCL hang (inject `sleep` inside an all-reduce hook)
4. OOM (allocate junk tensor)
5. Disk full (`fallocate` a large file)
6. Bad checkpoint (corrupt a shard)
7. Slow node (inject `time.sleep(0.5)` per step on one rank)
8. Network partition (drop traffic between rank groups using `tc`)
9. Host reboot (cordon + drain + simulate)
10. Container OOM-kill (set low cgroup memory limit)

Each script must: run, capture before/after state, write a report row. Tie them into `scripts/chaos/run_all.sh`.

### Day 20 (4 h) — Write ADRs
Polish ADRs 0001–0006 from your Week 1 stubs:
- `adr/0001-sharding-strategy.md`
- `adr/0002-checkpoint-format.md`
- `adr/0003-cluster-mgr-abstraction.md`
- `adr/0004-comms-backend.md`
- `adr/0005-observability-schema.md`
- `adr/0006-security-boundary.md`

Each ADR is **at minimum** 1 page and contains: context, decision, alternatives considered, consequences accepted, status, date, deciders.

### Validation gate
- [ ] Reference 7B model converges (loss curve in repo)
- [ ] MFU ≥ 40 % on H100 (or doc'd reason for variance)
- [ ] All 10 chaos scenarios pass with documented recovery behavior
- [ ] 6 ADRs merged

### Gotchas
- MFU on the first run is almost always disappointing. Budget time for the optimization pass.
- Chaos tests that "should" work in a notebook often don't in a real cluster — test in dev cluster, not laptop.

---

## Week 5 — Migration Plan, Talk, Polish (18 h)

### Goal
Hand off — both organizationally (migration plan, cost model) and intellectually (tech talk).

### Day 21 (4 h) — Migration plan
For 3 specific teams (real or realistic), write per-team:
- Current state (one paragraph)
- Migration path (numbered steps)
- Rollback (specific: what command, what state)
- Success metric (one number)
- Owner + reviewer
- Estimated effort

Output: `docs/migration-plan.md`.

### Day 22 (4 h) — Cost model
Spreadsheet or notebook with:
- Inputs (GPU type, $/hr, target utilization, team sizes, current setup waste)
- Per-job cost breakdown
- 12-month projected savings (be conservative; mark assumptions)
- One paragraph executive summary

Have someone non-technical (or a manager) read the executive summary and ask one question. If they don't understand it, rewrite it.

### Day 23 (4 h) — Tech talk
30–40 min, recorded. Outline:
1. The problem (3 min)
2. What we built (5 min)
3. Architecture deep dive — pick **two** decisions, not all six (10 min)
4. Demo (5 min): submit a job, watch a chaos test
5. What we got wrong (3 min)
6. Adoption / what's next (3 min)
7. Q&A

Record yourself. Watch the recording. Re-record once.

### Day 24 (3 h) — Polish docs + README
- Top-level `README.md`: quickstart, link to design doc, link to ADRs, link to migration plan.
- `docs/getting-started.md`: 30-minute path from `git clone` to a running tiny job.
- `docs/troubleshooting.md`: top 10 errors you've seen, with the fix.

### Day 25 (3 h) — Postmortem of an induced failure
Pick one chaos test that didn't behave as you expected (there will be at least one). Write it up in real postmortem format: timeline, root cause, contributing factors, what went well, what went poorly, action items.

### Validation gate
- [ ] Migration plan for 3 teams committed
- [ ] Cost model committed with assumptions
- [ ] Tech talk recorded + slides in repo
- [ ] Postmortem committed
- [ ] README + getting-started + troubleshooting docs

---

## Final Checklist Before Submitting

Tick every box. Each maps to an acceptance criterion in `requirements.md`.

- [ ] `mlctl submit examples/reference-7b.yaml` works from `git clone` in ≤ 30 min
- [ ] Design doc reviewed by ≥ 3 engineers with comments addressed
- [ ] 6 ADRs (at minimum) merged with full structure
- [ ] All 10 chaos scenarios pass in CI (or with a `make chaos` invocation)
- [ ] Reference MFU number reproducible by reviewer (±2 pp)
- [ ] Test coverage ≥ 80 % on core modules; CI green
- [ ] `mypy --strict` clean on public surface
- [ ] No file > 800 LOC, no function > 50 LOC
- [ ] Grafana dashboards work against a fresh Prometheus
- [ ] Migration plan + cost model + tech talk + postmortem all present

---

## Common Failure Modes — Read Before You Start

These are the ways previous principal-track learners have lost points on a project like this.

1. **Started coding before designing.** Spent week 1 on K8s plumbing; ended up rewriting half of it in week 4 because the failure model wasn't clear.
2. **Picked Megatron-style 3D parallelism on day 1** because it sounded principal-level. Burned 3 weeks. The right move is FSDP first, add TP later only if a workload requires it.
3. **Skipped the chaos suite.** "It works on the happy path" — yes, every framework does. The chaos suite is the differentiator.
4. **Used `torch.save` for checkpoints.** Doesn't shard, doesn't survive topology changes, can't recover from torn writes. Use `torch.distributed.checkpoint` or your own sharded format.
5. **Wrote a beautiful framework no one wants.** Skipped stakeholder interviews; built for a hypothetical user. The migration plan won't survive contact with real teams.
6. **No cost model.** Lost the org-level argument because a "10× faster training" claim isn't credible without a $/job comparison.
7. **Tech talk read off slides for 40 minutes.** Bored everyone. A principal-level talk has a *story* (we had this problem; here's what we tried; here's what worked; here's what surprised us).
8. **ADRs that say "we chose X because X is best."** No alternatives, no consequences. These read as junior. Always include what you didn't pick and why.
9. **Multi-cluster claimed in the doc, not in the code.** A reviewer will spot this in 30 seconds. Either build it or scope it out explicitly.
10. **Cost model has no assumptions section.** Numbers without assumptions are not credible. The assumptions section is the actual content.

---

## What "Good" Looks Like at the End

A reviewer with 1 hour should be able to:
1. Clone your repo
2. Read the README and immediately understand what the framework does and doesn't do
3. Run `make demo` and see a tiny training job complete
4. Look at the design doc and find the failure-model table within 30 seconds
5. Open an ADR and see a real decision (with the alternative) defended
6. Watch 5 minutes of your tech talk and understand the headline result
7. Read the migration plan for one team and believe it would actually work

If a reviewer can do all 7 — you've passed. If they can do them while smiling — you've hit the 85 +.
