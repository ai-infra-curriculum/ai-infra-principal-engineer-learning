# Job Requirements — Principal AI Infrastructure Engineer

**Research window:** 2026-07-11 → 2026-10-09 (data sample, last 90 days)
**Postings analyzed:** 25 in-window (18 carryovers from the 2026-09 cycle whose date_observed is still in-window + 7 fresh postings observed this cycle; 6 prior-cycle postings deliberately dropped to rebalance sample toward ML-infra and FDE Principal roles)
**Last updated:** 2026-10-09
**Machine-readable source:** [.aicg/job-requirements.json](.aicg/job-requirements.json)
**Proposed delta this cycle:** [.aicg/curriculum-plan-delta.json](.aicg/curriculum-plan-delta.json) — **zero additions**

## Headline

Three thresholds fired this cycle — and **the continuity-bias rule still says zero additions**:

- **req-agentic-platforms held exactly at 0.68** (17 of 25 postings) after deliberate sample rebalancing. The signal is confirmed real, not a 2026-09 artifact. The 2026-06 trigger (lecture-update deadline 2026-12-31) still has three months to run before firing.
- **req-ai-governance-safety went 0.20 → 0.36**, crossing the 0.30 promotion threshold. Prior-cycle pre-registered disposition was "absorb into mod-501 or mod-505, do NOT create a new module." On re-reading mod-501 and mod-505 exercises (both sets are topic-agnostic framework shapes: case-study, strategy-memo, stakeholder-mapping, roadmap, presentation), a learner can select AI governance as their case-study topic without any curriculum change. **Third clause of the continuity-bias rule fails: existing content CAN be incrementally extended via topic choice.** Same carve-out logic that applied to agentic-platforms in 2026-09. Zero additions; flag re-registered with a tighter trigger (0.40 floor, deadline 2027-03-31).
- **req-fde-leadership went 0.12 → 0.20**, firing the trigger registered last cycle. Prior-cycle disposition routes this OUT of the Principal packet: "open a scoping proposal for a new owner role (ai-infra-forward-deployed-learning); do NOT add to principal-track content." Still the right call — Amazon AWS and ServiceNow each published Principal-titled FDE roles this cycle.

The sample was rebalanced away from the 2026-09 AI-app bias (dropped MaintainX, OnBoard, Mitratech, G-P, Nscale Network, EBRD; added Oracle Core Infra, both NVIDIA ML-infra Principals, Kindo Agentic Platforms, Inflection Agentic AI, Amazon AWS FDE, ServiceNow FDE). The 2026-09 cycle's hypothesis that `req-distributed-systems` had crashed from 0.85 → 0.32 as a sample artifact only partially held: with rebalancing, the frequency stayed flat at 0.32. The 2026-06 reading of 0.85 was clearly inflated by over-indexing on title keywords; the current 0.32 is credible. The market has materially partitioned into "Principal AI Engineer / Agentic Principal" and "Principal ML Infrastructure Engineer" title families — next cycle should stratify sampling by family.

## Requirement coverage table

| ID | Requirement | Freq. this cycle | Δ vs 2026-09 | Owner role (lowest-level) | Principal coverage / link |
|----|-------------|------:|------:|---------------------------|---------------------------|
| req-agentic-platforms | Agentic-AI platform leadership | 0.68 | 0.00 | `ai-infra-ml-platform-learning` | **Principal slice:** [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) (agentic-AI is the canonical 18-month-payback bet; exercises 01–05 admit it as case-study content — see [`lecture-notes/01-overview.md`](lessons/mod-505-long-term-technical-bets/lecture-notes/01-overview.md)); [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/); [`project-04-innovation-poc`](projects/project-04-innovation-poc/) accepts agentic POC as a candidate. |
| req-multiteam-architecture | Multi-team architecture leadership; ADRs across 3+ teams | 0.60 | +0.08 | **ai-infra-principal-engineer-learning** | [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-mentor-engineers | Mentor senior/staff/principal engineers; raise the bar | 0.52 | +0.16 | **ai-infra-principal-engineer-learning** | [`mod-502-mentorship-leadership`](lessons/mod-502-mentorship-leadership/) (owner) |
| req-llm-orchestration | LLM application orchestration (LangChain, LlamaIndex, LangGraph) | 0.40 | -0.08 | `ai-infra-ml-platform-learning` | Principal slice via [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) and [`project-04-innovation-poc`](projects/project-04-innovation-poc/) |
| req-rag-vector | RAG architectures and vector DBs (Pinecone, Milvus, Weaviate, pgvector) | 0.36 | -0.08 | `ai-infra-ml-platform-learning` | Strategy framing via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/); bet framing via [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) |
| req-ai-governance-safety | AI governance / safety leadership (red-teaming, prompt-injection, guardrails, responsible-AI review) | **0.36** | **+0.16** | `ai-infra-mlops-learning` (provisional) | **Topic-agnostic framework coverage:** governance-as-strategy is a legitimate case-study topic for [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) exercises 01–05; safety-investment is a legitimate 18-month bet for [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) exercises 01–05. No curriculum change needed. |
| req-distributed-systems | Large-scale distributed systems design (10+ yr) | 0.32 | 0.00 | `ai-infra-architect-learning` | Exercised via [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/) and projects 01–03 |
| req-cross-org-influence | Cross-org influence without authority (3000+ engineer orgs) | 0.28 | +0.08 | **ai-infra-principal-engineer-learning** | [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/), [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-cloud-multitenant | Multi-cloud, multi-tenant production architectures (AWS/GCP/Azure) | 0.24 | +0.04 | `ai-infra-architect-learning` | Linked at principal via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) and [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-k8s-orchestration | Kubernetes for ML workloads (operators, autoscaling, Helm, Terraform) | 0.24 | +0.08 | `ai-infra-engineer-learning` | Implicit via [`project-01-distributed-training`](projects/project-01-distributed-training/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-fde-leadership | Forward Deployed Engineer leadership at Principal altitude | **0.20** | **+0.08** | **TBD — proposed `ai-infra-forward-deployed-learning`** | Routed OUT of this packet. Trigger fired: scoping proposal for a new owner role should be opened separately. Not a Principal-track addition — the competency profile is distinct. |
| req-mlops-cicd | Production MLOps / CI/CD / model monitoring | 0.12 | -0.08 | `ai-infra-mlops-learning` | Touched by [`project-02-platform-integration`](projects/project-02-platform-integration/), [`project-03-performance-optimization`](projects/project-03-performance-optimization/) |
| req-gpu-cluster-ops | GPU cluster ops on Kubernetes (DRA, KAI Scheduler, Grove, GPU debugging) | 0.16 | +0.04 | `ai-infra-performance-learning` | [`project-03-performance-optimization`](projects/project-03-performance-optimization/) at principal altitude |
| req-developer-platform | AI-native developer platforms / internal tooling | 0.16 | -0.04 | `ai-infra-ml-platform-learning` | [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-distributed-training | Distributed training (FSDP, DeepSpeed, Megatron-LM, ZeRO, parallelism) | 0.16 | +0.08 | `ai-infra-ml-platform-learning` | Already exercised at principal level by [`project-01-distributed-training`](projects/project-01-distributed-training/) (100 h) |
| req-ai-observability | Agent / LLM observability (OpenTelemetry, distributed tracing for agent workflows) | 0.08 | -0.04 | `ai-infra-mlops-learning` | Strategy framing via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-gpu-perf-tuning | GPU perf tuning and kernel optimization (CUDA, custom kernels) | 0.08 | +0.04 | `ai-infra-performance-learning` | [`project-03-performance-optimization`](projects/project-03-performance-optimization/) |
| req-llm-serving-engines | Production LLM inference engines (vLLM, SGLang, TensorRT-LLM) | 0.08 | +0.04 | `ai-infra-ml-platform-learning` | Cited as worked example in [`mod-505 lecture 01`](lessons/mod-505-long-term-technical-bets/lecture-notes/01-overview.md); evaluatable via [`project-04-innovation-poc`](projects/project-04-innovation-poc/) |
| req-llm-finetuning | LLM fine-tuning, prompt engineering, embedding optimization | 0.04 | -0.04 | `ai-infra-ml-platform-learning` | Build-vs-buy judgement via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-genai-multimodal | GenAI / multimodal systems strategic leadership | 0.00 | 0.00 | `ai-infra-ml-platform-learning` | Bet framing via [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) |

## What we did NOT propose (and why)

| Tempting addition | Why we rejected it |
|-------------------|--------------------|
| `mod-501` exercise-06 "AI governance as technical strategy" | req-ai-governance-safety did cross 0.30 (now 0.36), BUT mod-501 exercises 01–05 (case-study, strategy-memo, stakeholder-mapping, roadmap, presentation) are topic-agnostic framework exercises. A learner can and will pick AI governance as their case-study topic for exercise-01 and work through the strategy framework end-to-end for governance without any curriculum change. Adding a governance-specific exercise now would date the module to 2026-Q4 while duplicating framework content learners already drill. The topic-agnostic carve-out that spared agentic-platforms in 2026-09 applies here identically. Flag re-registered: if freq stays >= 0.40 AND lecture notes have not been updated to cite governance as a canonical worked example by 2027-03-31, promote. |
| `mod-505` exercise-06 "safety investment as 18-month bet" | Same reasoning: mod-505 exercises 01–05 are bet-agnostic and already admit any 18-month technical bet (including safety tooling, governance platform) as the case study. The generic bet-evaluation framework survives technology churn precisely because it does not name specific bets. |
| New `mod-506-ai-governance` module | Would duplicate the generic strategy/bet-evaluation frameworks already in mod-501 and mod-505. Prior-cycle pre-registered disposition explicitly forbade this ("do NOT create a new module"). |
| New `mod-505` exercise-06 "evaluate agentic-AI platform bet" | req-agentic-platforms held exactly at 0.68 — frequency clause of the 2026-06 trigger continues to fire, but the lecture-update clause (deadline 2026-12-31) is still three months away. mod-505 exercises 01–05 remain bet-agnostic and admit agentic-AI as case-study content. Adding a bet-specific exercise now would still date the module. The flag is preserved for 2026-12 or 2027-03 if the trend holds and no lecture update has landed by then. |
| Principal-track content on Forward Deployed Engineering | req-fde-leadership crossed the 0.20 trigger (0.12 → 0.20) — Amazon AWS and ServiceNow both published Principal-titled FDE roles this cycle. The prior-cycle pre-registered disposition explicitly routes this to a new owner role, NOT Principal content: "the competency profile is distinct." A separate scoping proposal for `ai-infra-forward-deployed-learning` should be opened outside this packet. |
| New module or exercise on RAG / vector-DB strategy | Owner is level 30 (`ai-infra-ml-platform-learning`) per the Ownership Rule. Principal slice already framed by mod-501 and mod-505. RAG frequency slipped slightly (0.44 → 0.36) with rebalancing but remains comfortably above 0.30; the owner determination has not changed. |
| Restore removed distributed-systems / MLOps / multi-cloud coverage | req-distributed-systems held flat at 0.32 even after rebalancing — confirms the 2026-09 drop was NOT a sample artifact and the 2026-06 reading of 0.85 was inflated. These requirements are correctly owned at lower-level roles. Removals are out of scope for this packet in any case. |

## Evidence base (25 in-window postings, 2026-07-11 → 2026-10-09)

Detailed posting records — employer, title, URL, date observed, location, requirement IDs — are in [`.aicg/job-requirements.json`](.aicg/job-requirements.json). Highlights below.

### Agentic-AI platform leadership signal (17 postings — still the load-bearing theme)

- **Salesforce** — Principal AI Engineer. 2026-09-09. SF / Seattle / Chicago / Palo Alto / NYC / Bellevue. [URL](https://careers.salesforce.com/en/jobs/jr344074/principal-ai-engineer/)
- **ServiceNow** — Principal AI Platform Engineer (FDE lead). 2026-08-31. Santa Clara / NA Remote. "acting as the Forward Deployed Engineer (FDE) lead architect directing a group of FDEs in the CRM & Industry Engineering organization." [URL](https://jobs.smartrecruiters.com/ServiceNow/744000146559474-principal-ai-platform-engineer)
- **ServiceNow** — Principal AI Forward Deployed Engineer. **2026-10-07 — fresh**. Santa Clara / Bay Area / Remote. "designing and operating autonomous agent systems — planning, tool invocation, memory, context design, and guardrails." [URL](https://careers.servicenow.com/jobs/744000154008689/principal-ai-forward-deployed-engineer/)
- **Wells Fargo** — Principal Engineer, AI Engineering. 2026-08-13. London. "define and promote responsible AI practices including evaluation, explainability considerations, fallback behavior, guardrails." [URL](https://www.wellsfargojobs.com/en/jobs/r-566959/principal-engineer-ai-engineering/)
- **Nordstrom** — Principal Engineer: AI Agentic Platform. 2026-07-15. Seattle Hybrid. [URL](https://careers.nordstrom.com/principal-engineer-ai-agentic-platform-hybrid-seattle-wa/job/444B4ADCF86579509074A4443C6A8C82)
- **NVIDIA** — Lead Principal Engineer, Enterprise Agentic AI Platform. 2026-08-01. Santa Clara. [URL](https://jobs.nvidia.com/careers/job/893393699154)
- **GE Vernova** — Principal AI Engineer. 2026-07-13. Bangalore. [URL](https://careers.gevernova.com/principal-ai-engineer/job/R5046671)
- **LTS** — Agentic AI Security Engineer (Principal-track IC). 2026-08-10. US Remote. "Assess risks associated with prompt injection, jailbreak attempts, indirect prompt attacks, tool misuse, hallucinations, data poisoning." [URL](https://job-boards.greenhouse.io/lts/jobs/4340457009)
- **Lila Sciences** — Principal Engineer, AI Security. 2026-07-20. Cambridge MA. [URL](https://job-boards.greenhouse.io/lilasciences/jobs/4210497009)
- **Strive Health** — Principal AI Engineer. 2026-07-18. Denver. "Act as a forward-deployed AI engineer, embedding with teams to understand workflows in detail and translate them into practical automations, copilots, agents, and decision-support tools." [URL](https://job-boards.greenhouse.io/strivehealth/jobs/4697975006)
- **US Bank** — Principal AI Engineer, AI Acceleration & Enterprise Enablement. 2026-08-10. 7 US locations. [URL](https://builtin.com/job/principal-ai-engineer-ai-acceleration-enterprise-enablement/10466219)
- **Vertex Inc** — Principal AI Engineer. 2026-08-12. US Remote. [URL](https://builtin.com/job/principal-ai-engineer/10211173)
- **DataCamp** — Principal AI Engineer. 2026-07-30. UK / BE / PT / UAE Remote. [URL](https://job-boards.greenhouse.io/datacamp/jobs/8074115)
- **Talkdesk** — Principal Forward Deployed Engineer, HLS. 2026-07-24. Seattle. "shaping how Talkdesk's most strategic global enterprise customers deploy and scale our Agentic AI platform." [URL](https://job-boards.greenhouse.io/talkdesk2/jobs/8083711)
- **Amazon AWS** — Principal Forward Deployed Engineer, AWS Forward Deployed Engineering. **2026-10-09 observation — fresh**. Seattle / Arlington VA / NYC / Denver. "Enterprise-scale agentic workflows, multi-agent orchestration, or AI system governance." [URL](https://amazon.jobs/en/jobs/10521876/principal-forward-deployed-engineer-aws-forward-deployed-engineering)
- **Kindo** — Principal AI Systems Engineer — Agentic Platforms. **Fresh**. Venice CA / SF Hybrid or Remote. [URL](https://job-boards.greenhouse.io/kindo/jobs/5060204007)
- **Inflection AI** — Principal Engineer, Agentic AI Systems. **Fresh**. Palo Alto. "Build production AI agents that can plan, retrieve context, call tools, take actions, and recover from failures." [URL](https://job-boards.greenhouse.io/inflectionai/jobs/4693056006)

### ML-infrastructure Principal signal (rebalanced into sample this cycle — 6 postings)

- **Coupang** — Principal Engineer, ML. 2026-09-09. Seattle. [URL](https://boards.greenhouse.io/coupang/jobs/8175850)
- **Nscale** — Principal Infrastructure Engineer, AI Cluster Performance & Validation. 2026-09-03. Houston / NYC / SF / Seattle. [URL](https://job-boards.greenhouse.io/nscaleoperationsukltd/jobs/4880753101)
- **Oracle** — Principal Core Infrastructure Engineer, AI Infrastructure. **2026-09-14 — fresh**. Nashville TN. [URL](https://careers.oracle.com/en/sites/jobsearch/job/344324/)
- **NVIDIA** — Principal AI and ML Infra Software Engineer, GPU Clusters. **Fresh**. Santa Clara. [URL](https://jobs.nvidia.com/careers/job/893394928067)
- **NVIDIA** — Principal Software Engineer, AI Inference. **Fresh**. Santa Clara. "demonstrated expertise in LLM inference/serving systems (e.g., vLLM, SGLang)." [URL](https://nvidia.wd5.myworkdayjobs.com/en-US/NVIDIAExternalCareerSite/job/Principal-Software-Engineer---AI-Inference_JR2013753)
- **Airbnb** — Principal ML Engineer, LLM Fine-tuning and Optimization. 2026-08-20. US Remote-eligible. [URL](https://careers.airbnb.com/positions/7955579/)
- **Twilio** — Principal Engineer. 2026-07-15. US Remote. [URL](https://job-boards.greenhouse.io/twilio/jobs/7944264-9)

### Forward Deployed Engineer Principal signal (CROSSED 0.20 TRIGGER THIS CYCLE)

- **ServiceNow** — Principal AI Platform Engineer (FDE lead architect). 2026-08-31. (Cross-referenced from agentic list.)
- **ServiceNow** — Principal AI Forward Deployed Engineer. **2026-10-07 — fresh, standalone Principal FDE title**. (Cross-referenced from agentic list.)
- **Amazon AWS** — Principal Forward Deployed Engineer, AWS Forward Deployed Engineering. **2026-10-09 observation — fresh**. (Cross-referenced from agentic list.) Application deadline 2026-10-10.
- **Strive Health** — Principal AI Engineer. 2026-07-18. "Act as a forward-deployed AI engineer." (Cross-referenced from agentic list.)
- **Talkdesk** — Principal Forward Deployed Engineer, HLS. 2026-07-24. (Cross-referenced from agentic list.)

### AI safety / governance leadership signal (CROSSED 0.30 THRESHOLD THIS CYCLE)

- **Lila Sciences** — Staff/Principal Research Engineer, AI Safety, Technical Mitigations. 2026-07-20. Cambridge / London / SF. "automated safety-testing / red-teaming systems, and monitoring systems." [URL](https://job-boards.greenhouse.io/lilasciences/jobs/4210472009)
- (Plus Wells Fargo, LTS, Lila Sciences AI Security, ServiceNow Platform, ServiceNow FDE, Amazon AWS FDE, Kindo, Inflection AI — cross-referenced from agentic / FDE lists above.)

## External resources for out-of-scope topics

These resources cover topics that are NOT principal-engineer ownership (per the Ownership Rule) but that Principal Engineers are commonly expected to reason about at the strategy / bet altitude.

- **Agentic frameworks & runtimes:** LangGraph (https://langchain-ai.github.io/langgraph/); OpenAI Assistants v2 / Responses API; Anthropic Agent SDK; NVIDIA NIM Agent Blueprints; Pinterest engineering blog on agentic SDLC; AWS Strands Agents.
- **LLM inference engines:** vLLM (https://github.com/vllm-project/vllm); SGLang; TensorRT-LLM; NVIDIA Triton; Hugging Face TGI.
- **RAG / vector DBs:** Pinecone, Milvus, Weaviate, pgvector, LanceDB. Retrieval quality benchmarks (RAGAS, BEIR).
- **GPU cluster scheduling on Kubernetes:** NVIDIA technical blog "Validate Kubernetes for GPU Infrastructure"; CNCF DRA driver; KAI Scheduler; Grove.
- **Distributed training:** Megatron-LM, DeepSpeed, FSDP.
- **AI safety & governance (threshold-crossing this cycle):** OWASP LLM Top 10 (2025 refresh); NIST AI RMF; EU AI Act (Regulation (EU) 2024/1689, enforcement began 2026-08-02 — now two months in); MITRE ATLAS; Anthropic Responsible Scaling Policy; OpenAI Preparedness Framework; UK AI Safety Institute evaluation methodology.
- **Forward Deployed Engineering (threshold-crossing this cycle, routed to new-role scoping):** Palantir FDE role writeups; OpenAI / Anthropic FDE blog posts; Talkdesk Agentic Platform docs; AWS FDE discipline overview.

## Methodology notes

- Postings sampled from Greenhouse, Lever, Ashby, SmartRecruiters, LinkedIn, BuiltIn, employer career pages (Salesforce, ServiceNow, Wells Fargo, NVIDIA, GE Vernova, Airbnb, Nordstrom, Oracle, Amazon AWS, Coupang, Nscale, Talkdesk).
- Postings included only when title contained "Principal" AND ≥1 of {AI, ML, Infrastructure, Platform, Inference, Distributed, Agentic, Forward Deployed}.
- Sample deliberately rebalanced this cycle: 18 carryovers from 2026-09 retained; 6 carryovers dropped to free sample slots for ML-infra and FDE Principal postings (MaintainX, OnBoard, Mitratech, G-P, Nscale Network, EBRD). 7 fresh postings added (ServiceNow FDE, Amazon AWS FDE, Oracle Core Infra, NVIDIA GPU Clusters, NVIDIA AI Inference, Kindo Agentic Platforms, Inflection AI Agentic). post-051 Turing dropped because its date_observed (2026-07-08) is three days outside the current window.
- Two postings included on snippet-only evidence (Nordstrom carryover, Oracle Core Infra) and flagged with `confidence: medium` in the JSON. The high-confidence postings (23) alone clear the ≥3 distinct posting / ≥0.30 frequency threshold for every requirement we drew conclusions about.
- The 2026-09 cycle's hypothesis that req-distributed-systems' drop from 0.85 → 0.32 was a sample artifact was partially falsified: with rebalancing, the frequency held flat at 0.32. The 2026-06 reading of 0.85 was inflated by over-indexing on title keywords in the 2026-06 sample.
- The market has materially partitioned into AI-application-Principal, ML-infrastructure-Principal, and Forward-Deployed-Principal title families. Next cycle should stratify sampling by family and report requirement frequencies within each stratum.

## Open questions / future cycles

<!-- needs-research: Whether req-agentic-platforms frequency stays >= 0.50 into 2026-12. Current cycle: 0.68 (identical to 2026-09, after rebalancing — signal confirmed real). If yes AND mod-505 lecture-01 has not been updated by 2026-12-31 to name agentic AI as a worked example, promote to mod-505 exercise-06 addition (case study on evaluating an agentic-platform bet). The lecture-update clause is the only remaining branch of the 2026-06 trigger. -->

<!-- needs-research: Whether req-ai-governance-safety stays >= 0.40 into 2027-03. Current cycle: 0.36 (crossed 0.30 threshold this cycle). Prior-cycle pre-registered disposition was absorbed by the topic-agnostic carve-out — mod-501 and mod-505 exercises 01-05 admit AI governance as case-study content without curriculum change. If freq stays >= 0.40 AND neither mod-501 nor mod-505 lecture notes have been updated to cite governance as a canonical worked example by 2027-03-31, promote to a governance-specific exercise. Tighter 0.40 floor than the generic 0.30 threshold because the current cycle's reading plausibly includes sampling lift from the EU AI Act enforcement news (enforcement began 2026-08-02, two months before this snapshot). -->

<!-- needs-research: Whether req-fde-leadership stays >= 0.20 into 2026-12. Current cycle: 0.20 (crossed trigger from 2026-09). FDE now has standalone Principal-level titling at Amazon AWS and ServiceNow (two postings this cycle, in addition to carryovers from Talkdesk, Strive Health, and ServiceNow Platform's FDE-lead title). If frequency holds >= 0.20 next cycle, open a scoping proposal for a new owner role (ai-infra-forward-deployed-learning) in a SEPARATE packet. Do NOT add to principal-track content — the competency profile is distinct (customer-embedded, deployment-focused, partly sales-adjacent). This packet is for the Principal AI Infrastructure Engineer learning repo only. -->

<!-- needs-research: Stratify next cycle's sample by title family (AI-application-Principal vs ML-infrastructure-Principal vs Forward-Deployed-Principal) and report requirement frequencies within each stratum, not just across the whole sample. The within-stratum frequencies will almost certainly tell different stories (e.g. req-rag-vector is likely ~0.80 in the AI-application stratum and ~0.00 in the ML-infra stratum) and the aggregate-frequency reading obscures that. The current cycle's rebalancing confirms the partition is real: req-agentic-platforms and req-distributed-systems both held flat at exactly the same frequencies as 2026-09 despite swapping six postings in and out, which strongly suggests the sample lives on a 3D plane rather than a single axis. -->

<!-- needs-research: Whether req-mentor-engineers' climb (0.36 -> 0.52) is driven specifically by FDE Principal roles mentoring the FDE group, or by a broader shift in how Principal roles itemize mentorship. If driven by FDE, the signal is real but routed to the proposed ai-infra-forward-deployed-learning role rather than mod-502. If broader, mod-502-mentorship-leadership remains the correct owner and no curriculum action is needed. -->
