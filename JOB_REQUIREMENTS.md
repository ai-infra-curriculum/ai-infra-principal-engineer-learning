# Job Requirements — Principal AI Infrastructure Engineer

**Research window:** 2026-03-10 → 2026-06-10 (data sample, last 90 days)
**Postings analyzed:** 34
**Last updated:** 2026-06-14 (4-day refresh of 2026-06-10 cycle — no new postings in the delta materially shift conclusions)
**Machine-readable source:** [.aicg/job-requirements.json](.aicg/job-requirements.json)
**Proposed delta this cycle:** [.aicg/curriculum-plan-delta.json](.aicg/curriculum-plan-delta.json) — **zero additions**

## Headline

The 2026-Q2 market for Principal AI Infrastructure Engineers is dominated by one new signal — **agentic AI platform leadership** — surrounded by the familiar mix of multi-team architecture, multi-cloud production engineering, MLOps, and distributed-systems depth. None of these requirements force a net-new module or exercise at the principal-engineer altitude:

- The **technical** requirements (vLLM/SGLang serving, RAG/vector DBs, GPU cluster operations, distributed training, agent runtimes / memory / tool calling) all belong, by the Ownership Rule, to lower-level roles (ML Platform, MLOps, Performance, Architect).
- The **leadership-altitude** requirements (multi-team ADRs, cross-org influence without authority, technical bet evaluation, mentorship at scale) map cleanly onto the existing `mod-501..505` set and projects 01–05.
- The emerging agentic-AI signal — 9 of 34 postings — is already absorbed by `mod-505-long-term-technical-bets`, whose existing worked example in `lecture-notes/01-overview.md` explicitly uses LLM / vLLM / foundation-model bets to teach the bet-evaluation framework. The framework is generic; learners apply it to whichever bet the moment demands.

## Requirement coverage table

| ID | Requirement | Freq. | Owner role (lowest-level) | Principal coverage / link |
|----|-------------|------:|---------------------------|---------------------------|
| req-multiteam-architecture | Multi-team architecture leadership; ADRs across 3+ teams | 0.65 | **ai-infra-principal-engineer-learning** | [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-cloud-multitenant | Multi-cloud, multi-tenant production architectures (AWS/GCP/Azure) | 0.55 | `ai-infra-architect-learning` | Linked at principal via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) and [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-mlops-cicd | Production MLOps / CI/CD / model monitoring | 0.55 | `ai-infra-mlops-learning` | Touched by [`project-02-platform-integration`](projects/project-02-platform-integration/), [`project-03-performance-optimization`](projects/project-03-performance-optimization/) |
| req-mentor-engineers | Mentor senior/staff/principal engineers; raise the bar | 0.55 | **ai-infra-principal-engineer-learning** | [`mod-502-mentorship-leadership`](lessons/mod-502-mentorship-leadership/) (owner) |
| req-distributed-systems | Large-scale distributed systems design (10+ yr) | 0.85 | `ai-infra-architect-learning` | Exercised via [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/) and projects 01–03 |
| req-agentic-platforms | Agentic-AI platform leadership (agent runtimes, memory, tool-calling, governance) | 0.30 | `ai-infra-ml-platform-learning` | **Principal slice:** [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) (agentic frameworks are the canonical 18-month-payback bet — already worked into [`lecture-notes/01-overview.md`](lessons/mod-505-long-term-technical-bets/lecture-notes/01-overview.md)); [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/); [`project-04-innovation-poc`](projects/project-04-innovation-poc/) accepts agentic POC as a candidate. |
| req-llm-orchestration | LLM application orchestration (LangChain, LlamaIndex, LangGraph) | 0.30 | `ai-infra-ml-platform-learning` | Principal slice via [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) and [`project-04-innovation-poc`](projects/project-04-innovation-poc/) |
| req-k8s-orchestration | Kubernetes for ML workloads (operators, autoscaling, Helm, Terraform) | 0.30 | `ai-infra-engineer-learning` | Implicit via [`project-01-distributed-training`](projects/project-01-distributed-training/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |
| req-cross-org-influence | Cross-org influence without authority (3000+ engineer orgs) | 0.30 | **ai-infra-principal-engineer-learning** | [`mod-503-cross-org-initiative`](lessons/mod-503-cross-org-initiative/), [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-distributed-training | Distributed training (FSDP, DeepSpeed, Megatron-LM, ZeRO, parallelism) | 0.18 | `ai-infra-ml-platform-learning` | Already exercised at principal level by [`project-01-distributed-training`](projects/project-01-distributed-training/) (100 h) |
| req-rag-vector | RAG architectures and vector DBs (Pinecone, Milvus, Weaviate, pgvector) | 0.18 | `ai-infra-ml-platform-learning` | Strategy framing via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-llm-serving-engines | Production LLM inference engines (vLLM, SGLang, TensorRT-LLM) | 0.18 | `ai-infra-ml-platform-learning` | Cited as worked example in [`mod-505 lecture 01`](lessons/mod-505-long-term-technical-bets/lecture-notes/01-overview.md); evaluatable via [`project-04-innovation-poc`](projects/project-04-innovation-poc/) |
| req-ai-observability | Agent / LLM observability (OpenTelemetry, distributed tracing for agent workflows) | 0.18 | `ai-infra-mlops-learning` | Strategy framing via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-gpu-cluster-ops | GPU cluster ops on Kubernetes (DRA, KAI Scheduler, Grove, GPU debugging) | 0.15 | `ai-infra-performance-learning` | [`project-03-performance-optimization`](projects/project-03-performance-optimization/) at principal altitude |
| req-llm-finetuning | LLM fine-tuning, prompt engineering, embedding optimization | 0.15 | `ai-infra-ml-platform-learning` | Build-vs-buy judgement via [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/) |
| req-gpu-perf-tuning | GPU perf tuning and kernel optimization (CUDA, custom kernels) | 0.12 | `ai-infra-performance-learning` | [`project-03-performance-optimization`](projects/project-03-performance-optimization/) |
| req-genai-multimodal | GenAI / multimodal systems strategic leadership | 0.12 | `ai-infra-ml-platform-learning` | Bet framing via [`mod-505-long-term-technical-bets`](lessons/mod-505-long-term-technical-bets/) |
| req-developer-platform | AI-native developer platforms / internal tooling | 0.10 | `ai-infra-ml-platform-learning` | [`mod-501-technical-strategy`](lessons/mod-501-technical-strategy/), [`project-02-platform-integration`](projects/project-02-platform-integration/) |

## What we did NOT propose (and why)

| Tempting addition | Why we rejected it |
|-------------------|--------------------|
| New `mod-506-agentic-ai-strategy` module | Would duplicate the generic bet-evaluation framework already in `mod-505 lecture 01`. The framework is meant to outlast specific technologies; embedding one moment's framework choice would shorten its shelf life. |
| New exercise on `mod-505`: "evaluate agentic platform bet" | Would date the module to 2026-Q2. The existing exercise-01-case-study and exercise-02-strategy-memo already accept any bet scenario; agentic-AI is a learner choice, not a curriculum requirement. |
| New `project-06-agentic-ai-poc` | `project-04-innovation-poc` already invites the learner to "Research and prototype emerging technology (e.g., quantum ML, edge AI)" — agentic platforms slot in directly. |
| New module on vLLM / inference-engine production deployment | Lives at level 30 (`ai-infra-ml-platform-learning`) per the Ownership Rule. Principal-level slice is already covered as a worked bet in `mod-505` and as a candidate POC in `project-04`. |
| New module on GPU cluster DRA / KAI Scheduler | Lives at level 35 (`ai-infra-performance-learning`). Principal-level work is exercised via `project-03`. |
| New module on multi-team agent observability | Lives at level 30 (`ai-infra-mlops-learning`). Principal-level strategy slice covered by `mod-501`. |

## Evidence base (34 postings, 2026-03-10 → 2026-06-10)

Detailed posting records — employer, title, URL, date observed, location, requirement IDs — are stored in [`.aicg/job-requirements.json`](.aicg/job-requirements.json). Highlights:

### Agentic-AI platform leadership signal (9 postings, the strongest emerging theme)

- **Pinterest** — Principal Engineer, Agentic Engineering. 2026-05-10. "12+ years of software engineering experience with deep expertise in building developer platforms" / "Demonstrated hands-on experience designing and shipping agentic AI systems in production" / "Track record of driving sweeping technical change across large engineering organizations without direct authority." [URL](https://www.workingnomads.com/jobs/principal-engineer-agentic-engineering-pinterest)
- **Amazon Web Services** — Principal / Senior Principal / Distinguished Engineer, AWS Agentic AI. "10+ years of design, development engineering experience" / "Experience as a technical leader of multiple concurrent projects, driving strategic technical decisions." [URL](https://amazon.jobs/en-gb/jobs/3196031/principal-senior-principal-and-distinguished-engineer-aws-agentic-ai)
- **Teradata** — Principal Engineer, Agentic AI. 2026-05-08. "Proven experience building AI agents, LLM-driven systems, or autonomous workflows" / "Hands-on experience integrating LLMs into production workflows with proper guardrails." [URL](https://careers.teradata.com/jobs/219972/principal-engineer-agentic-ai)
- **Salesforce** — Principal Agentic Data Systems Engineer. [URL](https://careers.salesforce.com/en/jobs/jr340264/principal-agentic-data-systems-engineer/)
- **Comcast** — Principal Cloud Engineer - AI Agentic. [URL](https://jobs.comcast.com/job/west-chester/principal-cloud-engineer-ai-agentic/45483/93984797888)
- **Twilio** — Principal Engineer (P5) with explicit AI-driven dev workflow expectations. [URL](https://www.glassdoor.com/job-listing/principal-engineer-twilio-JV_KO0,18_KE19,25.htm?jl=1010103276453)
- **Money Forward** — Principal Engineer (AI Agent Platform). [URL](https://careers.moneyforward.vn/job/principal-engineer-ai-agent-platform)
- **Kindo** — Principal AI Systems Engineer — Agentic Platforms. [URL](https://job-boards.greenhouse.io/kindo/jobs/5060204007)
- **SAFE Security** — Principal Engineer - AI (Bangalore, agentic + RAG). LinkedIn listing.

### LLM application / RAG / vector DB signal

- **GE Vernova** — Principal AI Software Engineer. 2026-04-28. "Proven track record designing and shipping production applications that incorporate LLMs or AI services at scale" / "Experience with vector databases (e.g. Milvus, Pinecone, Weaviate, pgvector) and advanced embedding model optimization" / "Expertise with multiple LLM providers and advanced AI orchestration frameworks." [URL](https://careers.gevernova.com/principal-ai-software-engineer/job/R5026489)
- **Blue Cross Blue Shield of Minnesota** — Principal AI Engineer. 2026-03-20. "5+ years deploying and maintaining ML/AI models in production at enterprise scale" / "Demonstrated experience with generative AI, multimodal systems, LLM fine-tuning" / "Architect and maintain production-grade ML infrastructure, including CI/CD and monitoring." [URL](https://careers.bluecrossmn.com/job/eagan/principal-ai-engineer/42354/93000241424)
- **Stryker** — Principal Engineer AI/ML (Speech, Voice & GenAI Architecture), Bangalore. "12+ years of experience in software engineering with substantial experience in AI/ML systems" / "Deep expertise in speech technologies (ASR/TTS), NLP, and modern LLM systems." [URL](https://careers.stryker.com/principal-engineer-ai-ml-speech-voice-genai-architecture/job/501ED90BB35F08368126A46170EAF6FB)

### Distributed-systems / GPU cluster signal

- **NVIDIA** — Principal Software Engineer, Distributed Systems — DGX Cloud (Kubernetes, GPU resource scheduling, cluster operations). [URL](https://jobs.nvidia.com/careers/job/893395121893)
- **NVIDIA** — Principal Software Engineer, AI Inference. "demonstrated expertise in LLM inference/serving systems (e.g., vLLM, SGLang)." [URL](https://nvidia.wd5.myworkdayjobs.com/en-US/NVIDIAExternalCareerSite/job/Principal-Software-Engineer---AI-Inference_JR2013753)
- **General Motors** — Principal AI/ML Engineer, AV ML Infra. 2026-05-13. "10+ years of experience, with a strong background in large-scale distributed systems preferred" / "5+ years of experience leading and driving large-scale initiatives." [URL](https://search-careers.gm.com/en/jobs/jr-202610759/principal-ai-ml-engineer-av-ml-infra/)
- **Motional** — Principal Engineer, Data & ML Infrastructure (Remote USA). [URL](https://www.remotech.ai/jobs/principal-engineer-data-ml-infrastructure)
- **AMD** — Principal ML Engineer, Large Scale Training Performance Optimization. [URL](https://www.linkedin.com/jobs/view/principal-ml-engineer-large-scale-training-performance-optimization-at-amd-4387546625)

### MLOps / ML platform signal

- **Xometry** — Principal ML Engineer, ML Platform Engineering. 2026-03-12. K8s/Terraform/Docker, multi-cloud. [URL](https://platformengineering.org/jobs/principal-ml-engineer-ml-platform-engineering-49273-4be2b)
- **Upstart** — Staff+/Principal ML Engineer. "7+ years of hands-on experience in applied machine learning, with strong exposure to production-scale modeling efforts" / "Ability to bridge engineering and science teams, and influence technical strategy across disciplines." [URL](https://careers.upstart.com/jobs/principal-machine-learning-engineer)
- **Microsoft AI** — Principal Data Infrastructure Engineer (Member of Technical Staff). [URL](https://www.linkedin.com/jobs/view/member-of-technical-staff-principal-data-infrastructure-engineer-at-microsoft-ai-4417345498)
- **Microsoft AI** — Principal Machine Learning Engineer. [URL](https://microsoft.ai/job/principal-machine-learning-engineer-4/)
- **Aledade** — Principal Engineer - AI Data and Infrastructure. [URL](https://jobs.lever.co/aledade/e9636a60-5455-4a52-afed-eeadb50424cd)
- **OpenAI** — Principal Software Engineer, B2B Engineering. [URL](https://openai.com/careers/principal-software-engineer-b2b-engineering-remote-us/)
- **Reddit** — Principal Software Engineer, ML Feature Platform. [URL](https://job-boards.greenhouse.io/reddit/jobs/6861064)
- **ExtraHop** — Principal Software Engineer | MLI. [URL](https://job-boards.greenhouse.io/extrahopnetworks/jobs/5525375004)
- **Cognite** — Principal ML Engineer. [URL](https://job-boards.greenhouse.io/cognite/jobs/4840714101)
- **Natera** — Principal AI/ML Platform Engineer. [URL](https://job-boards.greenhouse.io/natera/jobs/5718942004)
- **Altarum** — Principal Data Engineer — ML Platforms. [URL](https://jobs.lever.co/altarum/d09a7278-a8cf-4531-9e1c-63c9be025b5d)
- **Turing** — Principal Engineer — ML. [URL](https://job-boards.greenhouse.io/turing/jobs/5728724004)
- **Genesis Therapeutics** — ML Infrastructure Engineer (Staff/Principal). [URL](https://talents.vaia.com/companies/menlo-ventures/ml-infrastructure-engineer-staff-principal-22068574/)
- **Tubi** — Principal SWE, ML Infrastructure (SF). [URL](https://job-boards.greenhouse.io/tubitv/jobs/6589834)
- **Tubi** — Principal, ML Infrastructure Engineer (Canada). [URL](https://job-boards.greenhouse.io/tubi-canada/jobs/7495463)
- **Alignerr** — Principal Python Engineer, ML Infrastructure. 2026-04-04. [URL](https://www.linkedin.com/jobs/view/principal-python-engineer-ml-infrastructure-at-alignerr-4397429268)
- **Alignerr** — Principal Rust Engineer, ML Infrastructure. 2026-03-15. [URL](https://www.linkedin.com/jobs/view/principal-rust-engineer-ml-infrastructure-at-alignerr-4386041039)

## External resources for out-of-scope topics

These resources cover topics that are NOT principal-engineer ownership (per the Ownership Rule) but that Principal Engineers are commonly expected to reason about at the strategy / bet altitude. They are linked here so principal-track learners can ground their bets without the principal curriculum duplicating implementation-level material that belongs in lower-level tracks.

- **Agent runtimes & frameworks:** LangGraph docs (https://langchain-ai.github.io/langgraph/); Pinterest engineering blog on agentic SDLC.
- **LLM inference engines:** vLLM project README (https://github.com/vllm-project/vllm); SGLang and TensorRT-LLM benchmark comparisons published 2026-Q1/Q2.
- **GPU cluster scheduling on Kubernetes:** NVIDIA technical blog "Validate Kubernetes for GPU Infrastructure with Layered, Reproducible Recipes"; CNCF DRA driver and KAI Scheduler announcements from KubeCon Europe 2026.
- **Distributed training:** Megatron-LM, DeepSpeed, FSDP papers and worked examples — already part of the project-01 reading list at the principal level.
- **RAG / vector DBs:** Pinecone, Milvus, Weaviate documentation; pgvector for Postgres-native deployments.

## Methodology notes

- Postings sampled from Greenhouse, Lever, LinkedIn, employer career portals (NVIDIA, GE Vernova, GM, Microsoft AI, Pinterest, Amazon, Salesforce, OpenAI, BCBSMN, Stryker, Teradata, Comcast, MoneyForward, Reddit, AMD, Cognite, Natera, ExtraHop, Aledade, Tubi, Twilio), the PlatformEngineering.org jobs board, and Remotech.ai.
- Postings included only when title contained "Principal" AND ≥1 of {AI, ML, Infrastructure, Platform, Inference, Distributed, Agentic}.
- Five postings were only available as employer-job-board index summaries (Kindo, Reddit, Natera, Turing, Tubi Canada). Their requirements were taken from the search snippet plus employer description; they are marked `confidence: medium/low` in the JSON. The high-confidence postings (29) alone clear the ≥3 distinct posting / ≥0.30 frequency threshold for every requirement we drew conclusions about.
- Frequency is computed over the full 34-posting sample; requirements that only show up in low-confidence postings would not have been promoted to the table even if their snippet-derived count crossed the threshold.
- **2026-06-14 refresh:** the 2026-06-10 sample was re-checked four days into the cycle. No new postings appeared in the delta that would lift a requirement above an existing owner's coverage; no requirement frequency crossed a new threshold. Conclusions and zero-additions decision unchanged.

## Open questions / future cycles

<!-- needs-research: Whether the agentic-AI signal stabilises (re-test in 2026-09); if it remains > 0.30 frequency AND mod-505 lecture has not been updated by 2026-12 to reference it, revisit the case for an exercise-06 specific to agentic platform bet evaluation. -->

<!-- needs-research: Whether forward-deployed engineer (FDE) hiring at OpenAI/Anthropic/Cohere becomes principal-level (currently mostly applied/senior); FDE is a different competency profile (customer-facing) and may eventually need its own owner role rather than principal-curriculum coverage. -->
