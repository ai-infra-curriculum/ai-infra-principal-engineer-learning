# Requirements — Project 01: Distributed Training Framework

This document defines what the framework you build must do and must not do, the constraints you accept, the assumptions you may make, and the acceptance criteria a reviewer will check against.

Requirements use **MoSCoW** prioritization: **M**ust, **S**hould, **C**ould, **W**on't. "Must" items are pass/fail. "Should" items affect score. "Could" items are stretch. "Won't" items are explicit out-of-scope to keep you from boiling the ocean.

---

## 1. Functional Requirements

### 1.1 Job Submission API (Must)

- **M-FR-1**: A user submits a training job via a single CLI (`mlctl train ...`) and/or a Python entrypoint (`from training_framework import submit_job`). The submission interface must accept a declarative job spec (YAML or Python dataclass) covering at minimum:
  - model definition (Python entrypoint or registered model id)
  - dataset reference (URI + format)
  - parallelism strategy (`fsdp`, `deepspeed_zero3`, `ddp`, `auto`)
  - resource request (#GPUs, GPU type, memory floor, interconnect requirement)
  - max wall-clock + max budget
  - checkpoint policy
- **M-FR-2**: The same job spec must run unchanged on at least **two** cluster backends. Mandatory: Kubernetes. Strongly recommended second: Slurm, Ray, or a second Kubernetes cluster in a different region/account.
- **M-FR-3**: Job spec validation catches at least: invalid GPU type for cluster, parallelism strategy incompatible with model architecture, missing dataset access, budget below estimated cost. Validation errors are surfaced **before** any GPU is allocated.
- **S-FR-4**: A `dry-run` mode prints the resolved plan (placement, sharding, expected memory per rank, estimated cost, estimated wall-clock) without launching.

### 1.2 Sharding / Parallelism (Must)

- **M-FR-5**: First-class support for **PyTorch FSDP** as the primary data + sharded-parameter strategy.
- **M-FR-6**: First-class support for **DeepSpeed ZeRO Stage 3** as an alternative, with a documented decision rule for when each is preferred.
- **M-FR-7**: Mixed precision (BF16 default, FP16 supported) with loss scaling correctly wired for FP16.
- **M-FR-8**: Activation checkpointing (selective + full) configurable per-layer or per-model.
- **S-FR-9**: Tensor parallelism (Megatron-style) for models that do not fit on a single node with ZeRO-3 alone.
- **C-FR-10**: Pipeline parallelism (GPipe / 1F1B) for trillion-scale workloads.
- **C-FR-11**: MoE routing helpers (top-k gating, expert placement aware of cross-host bandwidth).

### 1.3 Checkpointing & Fault Tolerance (Must)

- **M-FR-12**: Sharded checkpoints written **asynchronously** so training is not blocked for more than 2 % of total step time on the checkpoint interval.
- **M-FR-13**: Checkpoints are **resumable to a different topology** — a job checkpointed on 64 ranks must be resumable on 32 or 128 ranks (with documented constraints, e.g., divisibility).
- **M-FR-14**: On node failure, the job restarts from the last good checkpoint **automatically**, without operator intervention, up to a configured max retry count.
- **M-FR-15**: On spot/preemption signal (e.g., 2-minute SIGTERM), the framework writes a checkpoint and exits cleanly; the controller re-queues the job at the same priority.
- **M-FR-16**: A "last good checkpoint" is determined by an integrity check (file presence + a written manifest with rank-count + step-id + checksum), not by "newest mtime".
- **S-FR-17**: Checkpoint deduplication across steps — the optimizer state for unchanged shards may be stored once and referenced (e.g., content-addressable via SHA-256), reducing storage by ≥ 30 % on long runs.

### 1.4 Multi-Cluster Orchestration (Must)

- **M-FR-18**: A submitted job can be **placed** on one of N candidate clusters based on a placement policy (cheapest GPU, lowest queue depth, region pin, hardware requirement).
- **M-FR-19**: A single user identity (OIDC) is honored across all backing clusters; users do not manage per-cluster credentials.
- **M-FR-20**: Job status, logs, and metrics are aggregated into a **single** view, regardless of which cluster the job runs on.
- **S-FR-21**: Jobs may migrate between clusters on failure (e.g., region outage), resuming from checkpoint on a new cluster.

### 1.5 Observability (Must)

- **M-FR-22**: Each rank emits at least the following metrics to Prometheus (or equivalent), at ≥ 1 sample per 10 seconds:
  - step time (forward, backward, optimizer, total)
  - GPU SM utilization (via DCGM)
  - GPU memory used + reserved
  - NCCL collective time, broken down by op (AllReduce, AllGather, ReduceScatter)
  - loss + gradient norm
  - tokens / second / GPU (for LLM workloads)
- **M-FR-23**: A Grafana dashboard ships with the framework. A new user can answer "is my job healthy?" from the dashboard within 60 seconds.
- **M-FR-24**: Structured logs include `job_id`, `rank`, `world_size`, `step`, `epoch` on every line; logs are searchable in whatever stack you use (Loki, ELK, Cloud Logging).
- **S-FR-25**: At least one **golden alert** fires reliably: "training stalled" (no step progress for > 5 × p99 step time).

### 1.6 Cost Controls (Must)

- **M-FR-26**: Per-team budget enforcement — when a team's monthly GPU-hour budget is exhausted, new jobs are blocked (or moved to lower priority) with a clear error message.
- **M-FR-27**: Each job records GPU-hours consumed, cost (in $ via a configurable price book), and storage cost.
- **S-FR-28**: Mixed spot/on-demand: framework can request spot capacity for ranks 1..N-K and on-demand for ranks N-K+1..N, with documented behavior on partial spot loss.

### 1.7 Security (Must)

- **M-FR-29**: User identity is federated (OIDC); no shared service-account keys in user job specs.
- **M-FR-30**: Per-job IAM/RBAC scoping: a job can only read the datasets and write the checkpoint paths declared in its spec.
- **M-FR-31**: Secrets (e.g., dataset credentials) are injected via the orchestrator's secret store, never written to the job spec.

---

## 2. Non-Functional Requirements

### 2.1 Performance (Must / Should)

| ID | Requirement | Target |
|----|-------------|--------|
| M-NFR-1 | Reference workload MFU on H100 | ≥ 45 % for 7B model on 32 GPUs; ≥ 40 % for 70B on 256 GPUs (or hw-adjusted equivalent) |
| M-NFR-2 | Checkpoint write overhead | ≤ 2 % of step time amortized |
| S-NFR-3 | Scheduler-to-first-step latency | ≤ 90 s for a warm image, ≤ 5 min cold |
| S-NFR-4 | Strong scaling efficiency | ≥ 85 % from 8 → 64 GPUs on reference workload |
| C-NFR-5 | Weak scaling efficiency | ≥ 90 % from 64 → 512 GPUs |

You must measure these and report numbers. Failing the target with an honest explanation is acceptable for "Should"; for "Must" you need to fix or document a credible mitigation.

### 2.2 Reliability (Must)

- M-NFR-6: Framework control plane has **no single-region single point of failure** for job submission or status query.
- M-NFR-7: A node failure must not cause >1 retry consuming budget; if it does, the system pages on-call.
- M-NFR-8: Mean time to recover from a single node loss: **≤ 10 minutes** at 64 GPUs.

### 2.3 Usability (Should)

- S-NFR-9: A user new to the framework can submit a working "Hello, FSDP" job inside **30 minutes** following the docs.
- S-NFR-10: Job spec errors include the bad field, the rule violated, and a suggested fix — not just a stack trace.

### 2.4 Maintainability (Must)

- M-NFR-11: ≥ 80 % unit test coverage on the scheduling, checkpointing, and orchestration modules.
- M-NFR-12: All public APIs typed (Pydantic / dataclasses / TypedDict + mypy strict on the public surface).
- M-NFR-13: No file > 800 LOC in `src/`. Functions ≤ 50 LOC. Modules organized by feature, not by file type.
- M-NFR-14: All ADRs include alternatives considered and consequences accepted.

### 2.5 Observability (Must)

Covered in functional 1.5. Restated as NFR: dashboards must load in < 3 s for jobs up to 1,024 ranks.

---

## 3. Constraints

- **C-1**: You may not require a specific cloud provider. The control plane must run on commodity Kubernetes. Cloud-specific accelerations (EFA, GCP TPUs, Azure InfiniBand) are pluggable, not assumed.
- **C-2**: You may not require a specific dataset format. Your framework must work with at least: WebDataset, MosaicML Streaming, raw S3-listed shards.
- **C-3**: You may use only OSS as direct dependencies. Commercial dependencies (e.g., Weights & Biases, vendor schedulers) are optional integrations, never required.
- **C-4**: Your framework must not require root inside the training container.
- **C-5**: The interconnect requirement (NVLink, InfiniBand, RoCE, plain Ethernet) must be declarable in the job spec, and jobs must refuse to start on insufficient interconnect (rather than silently degrading to a slow run).
- **C-6**: All checkpoints must be readable by **plain PyTorch** (`torch.load` of an unsharded merged checkpoint) — i.e., users are never locked into your runtime to inspect a model.

---

## 4. Assumptions

You may assume the following without further justification. If you do not have them, document the substitute.

- **A-1**: At least one Kubernetes cluster (≥ 1.27) with NVIDIA GPU operator installed.
- **A-2**: Object storage (S3-compatible) reachable from training nodes at ≥ 10 GB/s aggregate per node.
- **A-3**: A Prometheus + Grafana stack you can deploy into (or that already exists).
- **A-4**: A working OIDC provider (Okta, Google, Auth0, Dex).
- **A-5**: At least 8 GPUs you control for development, with credible plan for at least one 32-GPU run.
- **A-6**: Your reference model is a standard transformer (LLaMA-style). You may use a published reference implementation.

---

## 5. Out of Scope (Won't)

To keep this project at 100 hours and not 1,000:

- **W-1**: You will not build the model registry / model serving layer. Hand-off to an existing one (MLflow, Hugging Face Hub, or an internal mock).
- **W-2**: You will not build the data preprocessing pipeline. Assume datasets are already prepared on object storage in one of the supported formats.
- **W-3**: You will not build a custom inference engine. Trained models are exported to a standard checkpoint format and handed off.
- **W-4**: You will not build a billing system. Cost reports are produced by your framework; actual chargeback is out of scope.
- **W-5**: You will not implement your own collective communication library. NCCL (or, on CPU, Gloo) is assumed.
- **W-6**: You will not solve federated learning, cross-organization training, or differential privacy.
- **W-7**: You will not build a UI beyond the Grafana dashboards and a CLI. A web console is explicitly out of scope.
- **W-8**: You will not solve dataset versioning. Assume an immutable URI is the contract.

---

## 6. Acceptance Criteria

A reviewer should be able to mechanically check these. Each maps back to one or more requirements above.

### A. Reference workload runs
1. From a fresh checkout, following the docs, a reviewer can submit the bundled 7B reference job and see it converge within the documented wall-clock and budget.
2. `mlctl status <job-id>` shows live progress; Grafana dashboard shows the job within 60 s of submission.
3. The MFU number reported matches (within ±2 percentage points) the number claimed in your README.

### B. Fault tolerance demonstrated
4. Reviewer can run the bundled chaos test suite (`scripts/chaos/run_all.sh`) and see all 10 scenarios pass with documented expected behavior.
5. Killing a worker pod mid-training causes auto-recovery in ≤ 10 minutes at 8 GPUs (extrapolated target documented for 64+).
6. A spot-preemption simulation (SIGTERM with 2-minute grace) results in a clean checkpoint and a successful resume.

### C. Multi-cluster works
7. The same job spec submitted with `--cluster=cluster-a` and `--cluster=cluster-b` runs on each, identically.
8. A placement policy demo shows the scheduler choosing between two clusters based on queue depth or price.

### D. Design artifacts complete
9. Design doc exists, is between 15 and 25 pages, has been reviewed by 3+ engineers, and shows their (addressed) comments.
10. At least 6 ADRs exist, each ≥ 1 page, each naming the alternatives.
11. Migration plan exists for 3 teams, written as actionable steps with rollback for each.
12. Cost model exists as a spreadsheet (or executable notebook) with assumptions called out separately from outputs.

### E. Quality bar
13. Unit test coverage ≥ 80 % on the listed core modules; CI runs them on every PR.
14. `mypy --strict` clean on public API surface.
15. No file > 800 LOC, no function > 50 LOC, no nesting > 4 levels (linter-enforced or manually verified).

### F. Communication
16. Tech talk recorded (≥ 25 min, ≤ 45 min).
17. Slide deck checked in.
18. README + Getting Started doc enables a stranger to submit a job in ≤ 30 min.

---

## 7. Dependencies on Other Teams (For the Plan, Not the Code)

For the migration plan and design doc, you must explicitly identify dependencies on:

- **Platform / Infra team** — for cluster capacity, scheduling priorities, networking
- **Security team** — for OIDC integration, IAM scoping, secret store
- **Finance / FinOps team** — for the price book, budget enforcement policy, chargeback
- **At least 3 ML teams** — for migration; you need their current setup, their pain points, and their veto power documented

Naming real people / orgs is not required. The point is to demonstrate principal-level awareness that this project is a cross-org change, not just a code change.

---

## 8. Glossary

- **MFU (Model FLOP Utilization)** — fraction of theoretical peak FLOPS achieved during training
- **FSDP** — Fully Sharded Data Parallel (PyTorch)
- **ZeRO** — Zero Redundancy Optimizer (DeepSpeed); ZeRO-3 shards parameters, gradients, and optimizer state
- **TP / PP / DP** — Tensor / Pipeline / Data parallelism
- **NCCL** — NVIDIA Collective Communications Library
- **DCGM** — NVIDIA Data Center GPU Manager
- **MoE** — Mixture of Experts
- **RTO / RPO** — Recovery Time Objective / Recovery Point Objective
- **Gang scheduling** — all ranks of a job start (or none start)
- **Placement policy** — how the scheduler chooses cluster + nodes for a job
