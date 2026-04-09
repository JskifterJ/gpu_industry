# ICN GPU Compute Positioning — Baseline Document
*Date: 2026-04-09 | Author: J. Skifter | Status: Working draft*

---

## Purpose

This document establishes a structured baseline for reasoning about how to position ICN and Impossible Cloud (IC) within the GPU compute market. It is designed to be referenced and updated as our thinking evolves — not a finished strategy, but a shared mental model.

Two perspectives are analysed in parallel:

- **Perspective A** — Clean-slate startup: as if ICN/IC had no prior assets or constraints and were entering the market as a well-funded new entrant.
- **Perspective B** — ICN + IC integrated play: taking IC's S3 storage assets and ICN's token network as starting points, and reasoning from the "reverse playbook" (add compute adjacent to storage).

---

## 1. Nine Forces — Strategic Implications

These nine trends define the macro environment and should anchor every positioning decision.

### Force 1 — Energy Wall
Datacenter power has become the primary bottleneck, exceeding silicon or real estate as the constraining variable. A single frontier training cluster draws 40–100 MW. Data center electricity consumption is projected to exceed 1,050 TWh by 2026. Future infrastructure expansion will tether directly to isolated, gigawatt-scale power sources rather than traditional fibre hubs.

**Implication:** Location strategy matters more than GPU procurement strategy. Players who control power capacity (or co-locate with renewable/nuclear sources) hold a durable moat independent of chip generation. For ICN/IC: energy access per node — where are IC's data centers, and do they have stranded-power access or liquid cooling?

### Force 2 — Commoditisation of Bare-Metal GPU Access
H100s and H200s are becoming commodities as long-term hyperscaler contracts approach end-of-life and flood the secondary market. A wave of B200s will follow the same cycle. Simply providing raw GPU access is a race to the bottom.

**Implication:** Differentiation must live in the orchestration and software layers above bare metal. Bare metal is the table stake, not the moat.

### Force 3 — Hypervisor Tax Elimination
Frontier model labs will no longer accept the FLOP loss from virtualisation. Neoclouds offering bare-metal + optimised orchestration (e.g. CoreWeave's SUNK = Slurm-on-Kubernetes) are winning the large training market. 20%+ higher Model FLOP Utilisation (MFU) is a commercially meaningful gap.

**Implication:** Any serious GPU offering needs bare-metal access as a baseline. Orchestration quality — specifically Slurm + Kubernetes integration — is a tier-1 differentiator for training workloads.

### Force 4 — Financialisation of AI Infrastructure
The AI infrastructure buildout is structurally debt-financed. CoreWeave: $66.8B backlog, $30B capex in 2026. Nebius raised $4.3B in convertible notes. This creates a circular dynamic: NVIDIA funds neoclouds; neoclouds buy NVIDIA silicon; the bet requires downstream AI application revenue to materialise. Market analysts flag this as a fragile circular economy.

**Implication:** Scale and capital intensity are the terrain of established neoclouds. ICN/IC cannot (and should not) compete on datacenter scale directly. The strategic play is finding the layer of the stack where capital intensity is lower and defensibility is higher.

### Force 5 — Hyperscaler Custom Silicon (TPU, Trainium, MTIA)
Google, AWS, Meta are all building proprietary silicon to escape NVIDIA pricing. These chips are optimised for their internal workloads and create deep lock-in. They reduce margin for NVIDIA but also reduce openness.

**Implication:** For ICN/IC, this is a background threat. Open GPU compute (NVIDIA/AMD) remains the default for third-party providers. No action needed short-term, but worth watching as AMD ROCm matures and provides a CUDA alternative.

### Force 6 — Agentic Workload Shift
Agentic AI (long-horizon, multi-step, tool-using) creates qualitatively different infrastructure requirements: larger KV caches, longer context windows, CPU/GPU ratio rebalancing (from 8:1 to 4:1 or 2:1), adjacent vector database storage (Redis Streams, S3-backed vector DBs), and 8-hour autonomous execution loops (see Z.ai GLM-5.1 benchmarks).

**Implication:** Agentic workloads benefit from co-located high-performance storage adjacent to compute. IC's S3 platform is a natural vector DB backend (most write to S3 indexes). This is a structural advantage for Perspective B.

### Force 7 — Inference Efficiency as Primary Revenue Battleground
Inference now exceeds training in total compute consumption (approximately 2:1). Providers with proprietary inference serving platforms (TensorRT-LLM, vLLM + hardware tuning, speculative decoding, FP4/FP8 quantisation) are capturing durable, sticky enterprise OpEx. The metric is shifting from TFLOPS to tokens/watt.

**Implication:** Inference-first positioning is commercially correct for a new entrant. ICN/IC's early PoC using Blackwell 6000 GPUs with Multi-Instance GPU (MIG) is well-aligned — MIG allows multiple parallel inference workloads on the same card, improving utilisation and reducing cost per customer job.

### Force 8 — Fine-Tuning Proliferation
Knowledge-intensive firms are hiring AI researchers and setting up proprietary fine-tuning pipelines on open-weight models (Llama 4, Qwen 2.5, Mistral). A 70B parameter model can be fine-tuned via QLoRA for $5K–$25K in compute. This creates a large, recurring market for mid-tier compute (single node to small clusters) with associated data storage needs.

**Implication:** Fine-tuning SMEs are a strong ICP for ICN/IC. They need: a modest number of GPUs (8–64), high-speed storage for training datasets, competitive pricing relative to hyperscalers, and a simple developer experience. All of these intersect with IC's storage + ICN's GPU positioning.

### Force 9 — Metric Evolution (Tokens/Watt, TCO over card-hour)
Buyers are moving from card-hour pricing to tokens-per-dollar and TCO evaluation. Data egress fees — often invisible at small scale — become a major cost driver as training data and model artefacts scale to petabytes. CoreWeave charges ~$30/TB/month for storage. Hyperscaler egress fees compound on top.

**Implication:** TCO framing is ICN/IC's strongest commercial narrative. IC at ~$7/TB with zero egress vs CoreWeave at ~$30/TB creates a 4x storage cost difference. At 1 PB of training data, this is $23,000/month in savings on storage alone — before any egress savings.

---

## 2. Market Structure — The ClusterMAX Framework

The GPU compute market is best understood through four distinct business model tiers, each occupying a different horizontal slice of the value stack (L1–L7).

### Stack Layers (L-1 to L7)
| Layer | Name | Description |
|-------|------|-------------|
| L-1 | Hardware Design & Silicon | GPU/ASIC design, HBM fabrication, foundries (NVIDIA, AMD, TSMC) |
| L1 | Physical Data Center | Land, power, cooling, racks (Equinix, REITs, Neoclouds) |
| L2 | Cloud Compute & Networking | Bare metal, virtualization, InfiniBand/RoCE fabrics |
| L3 | Orchestration & Resource Mgmt | Kubernetes, Slurm, GPU slicing, fractional allocation |
| L4 | Distributed Frameworks & Aggregation | PyTorch/JAX, marketplaces, unified API routing |
| L5 | Model-as-a-Service | Fine-tuning platforms, inference endpoints, token APIs |
| L6 | Middleware & DePIN | Cross-cloud routing, decentralized compute, agentic middleware |
| L7 | End-User Applications | SaaS, enterprise tools, agentic workflows |

### Four Business Models

**Hyperscalers (AWS, GCP, Azure, OCI)** — Stack coverage L1–L7
- Moat: data gravity + ecosystem lock-in + global compliance footprint
- Weakness: egress fees, rigid orchestration, high pricing, slow provisioning
- Direction: custom silicon (TPU, Trainium), energy verticalization (Google: $4.75B Intersect Power acquisition), wholesale capacity deals

**Neoclouds (CoreWeave, Lambda, Nebius, Voltage Park)** — Stack L1–L4
- Moat: bare-metal performance, InfiniBand networking, no egress, OEM partnerships, MFU
- Weakness: capital intensity ($10–12M per MW), geographic constraints, concentrated customer risk
- Direction: becoming wholesale foundries for hyperscalers; massive debt financing; robotics/physical AI

**Orchestrators (Run:ai, Together AI, Anyscale, SkyPilot)** — Stack L3–L5
- Moat: utilization efficiency, developer velocity, cross-cloud abstraction
- Weakness: hardware dependency, margin pressure as hardware commoditises
- Direction: building own data centers (Together AI), hybrid infrastructure, data transformation

**Aggregators (Vast.ai, Foundry, Shadeform, Salad)** — Stack L4
- Moat: pricing arbitrage, instant availability, distributed coverage
- Weakness: inconsistent quality, no SLA guarantees, poor security posture for enterprise
- Direction: unified API provisioning, DePIN integration, spot market sophistication

---

## 3. Player Landscape by Business Model

### Hyperscalers
| Provider | ClusterMAX | Core Differentiator |
|----------|-----------|---------------------|
| AWS | Gold | Ecosystem breadth, SageMaker, P6/B200 instances |
| Google Cloud | Gold | AI Hypercomputer, TPU v7p, Vertex AI integration |
| Azure | Silver | OpenAI partnership, enterprise installed base |
| OCI | Gold | RDMA networking, aggressive pricing, 131K GPU clusters |

### Neoclouds
| Provider | ClusterMAX | Core Differentiator |
|----------|-----------|---------------------|
| CoreWeave | Platinum | SUNK (Slurm-on-Kubernetes), 20% higher MFU, $66.8B backlog |
| Lambda Labs | Gold | Developer experience, 1-Click Clusters, transparent pricing |
| Nebius | Silver | InfiniBand-first, European focus, $4.3B financing |
| Voltage Park | NR | Non-profit, long-term reserved contracts, cost-first |

### Orchestrators
| Provider | ClusterMAX | Core Differentiator |
|----------|-----------|---------------------|
| Run:ai (NVIDIA) | Gold | GPU pooling/slicing in Kubernetes; NVIDIA integration |
| Together AI | Gold | Together Kernel (inference speedups), data transformation |
| Anyscale | Silver | Ray framework, Global Resource Scheduler, multi-cloud |
| SkyPilot | Preferred | Open-source cross-cloud CLI, auto cost optimisation |

### Aggregators/DePIN
| Provider | ClusterMAX | Core Differentiator |
|----------|-----------|---------------------|
| Vast.ai | Gold | Largest P2P GPU marketplace, spot pricing |
| Foundry | Silver | Institutional capacity search, enterprise portability |
| Shadeform | Silver | Unified API across 20+ providers |
| Salad | Silver | Consumer GPU network, batch/async inference |
| Akash | NR | On-chain compute marketplace, DePIN native |

---

## 4. Emerging Business Models and Vying Plays

### Storage-Adjacent Compute (IC's Reverse Playbook)
Adding GPU compute adjacent to an existing large-scale object storage platform. The thesis: data gravity pulls compute toward data, not the other way around. If training datasets and model artefacts live in S3, bringing compute to the data eliminates egress costs and reduces latency. IC's existing customer (600TB → 1PB on CoreWeave) is the clearest validation signal: they are already paying CoreWeave $30/TB/month for storage that could sit on IC at $7/TB.

### Inference-First / Model Serving Specialists
Providers like Together AI, Groq (ASICs for inference), and Fireworks AI are building businesses entirely on inference optimisation rather than training clusters. These providers extract margin through software efficiency rather than hardware scale.

### Sovereign / Regulated Compute
European-focus providers (Nebius, Hetzner Cloud, OVHcloud) capitalising on GDPR data residency requirements and EU AI Act compliance needs. National AI initiatives (France, Germany, UAE) are creating government-contracted sovereign GPU capacity.

### Physical AI / Robotics Clouds
Nebius's 2028 Missouri facility is the first explicit "robotics cloud" — compute designed for physical AI simulation (digital twins, reinforcement learning environments). This is a medium-term emerging segment, not immediately relevant for ICN.

### DePIN GPU Networks (ICN's current territory)
Decentralized Physical Infrastructure Networks aggregating globally distributed consumer and prosumer GPUs. Strengths: cost, elasticity, no single point of failure. Weaknesses: inconsistent reliability, poor enterprise compliance, "lazy worker" verification challenges. ICNT token sits in this layer.

---

## 5. Ideal Customer Profile (ICP) Breakdown

### ICP-1 — Enterprise AI Integrator (Regulated Industries)
Finance, healthcare, pharma, defence. Moving from pilots to production agentic workflows and RAG systems.
- **Pain:** egress costs at scale (60–70% of equivalent on-prem hardware costs), data sovereignty requirements
- **Expectation:** white-glove SLAs, private cloud or VPC isolation, compliance (SOC 2, ISO 27001, GDPR)
- **ICN fit:** Low in the short term. Requires mature compliance posture and dedicated infrastructure.

### ICP-2 — AI-Native Startup / Scale-up
Rapid-iteration teams building coding copilots, vertical AI apps, open-weight fine-tuning pipelines.
- **Pain:** hyperscaler egress fees destroying runway, slow provisioning, complex IaC requirements
- **Expectation:** zero egress, transparent pricing, instant deploy, containerised endpoints (pre-configured vLLM/ComfyUI)
- **ICN fit:** High. Zero-egress + IC storage at $7/TB is a direct TCO weapon. Developer experience (DX) is addressable.

### ICP-3 — Foundation Model Lab / Deep Tech Researcher
Capitalized research labs and academic consortia running large-scale pre-training or multi-thousand-GPU fine-tuning.
- **Pain:** hardware underutilisation (<70% MFU), network bottlenecks, provisioning delays
- **Expectation:** bare metal, non-blocking InfiniBand or 400–800 Gbps RoCE, MFU metrics, dedicated cluster
- **ICN fit:** Low-medium currently. PoC-scale (12 GPUs) is too small. Network and InfiniBand capability unclear. Not the priority ICP.

### ICP-4 — Fine-Tuning SME (Knowledge-Intensive Firms) ← **Highest fit for ICN/IC right now**
Professional services, legal, biotech, enterprise software — firms hiring ML engineers to fine-tune open-weight models on proprietary data for internal tools.
- **Pain:** proprietary data cannot leave the firm; hyperscaler storage costs are prohibitive at petabyte scale; egress fees on training data movement are budget-killers; they need a modest number of GPUs (8–64) not a 10,000-GPU cluster
- **Expectation:** data residency, competitive pricing, simple setup (managed endpoint with their container), high-speed storage adjacent to compute
- **ICN fit:** Very high. The combination of IC's S3 (data sits here, cheap, zero egress) + ICN compute next to it is a compelling story: "your proprietary training data never leaves our infrastructure, and you pay 4x less for storage than on CoreWeave." MIG-enabled Blackwell 6000 PoC is appropriately scaled.
- **Critical tension:** ICNT token requirement. Enterprise SMEs will not want to touch a crypto token. ICN leadership must decide: fiat-only pricing for ICP-4, or DePIN-native pricing only? These ICPs are incompatible without explicit product segmentation.

### ICP-5 — Media / Creative (Generative Video)
Film studios, marketing agencies, simulation teams needing 60–120 GB VRAM for diffusion video workloads (HunyuanVideo, Wan 2.1).
- **Pain:** consumer GPUs are memory-insufficient (RTX 5090 = 32GB VRAM); hyperscalers don't offer specialised video pipelines; render times in minutes are commercially unacceptable
- **Expectation:** high-VRAM nodes (H200 or B200, 192GB+), fast provisioning, per-generation pricing
- **ICN fit:** Medium. The PoC hardware (Blackwell 6000) may fit inference; unclear if VRAM capacity meets video generation needs at scale. Storage for raw footage and model weights is a natural IC integration.

---

## 6. Perspective A — Clean-Slate GPU Startup Positioning

*Assume: well-funded new entrant with no prior assets or token obligations. What would be the optimal positioning?*

### Target ICP
ICP-2 (AI-native startups) and ICP-4 (fine-tuning SMEs). Both want: zero egress, transparent pricing, fast onboarding, mid-size clusters (8–64 GPUs), managed inference endpoints.

### Recommended Tier Positioning
**Neocloud-lite / Orchestrator hybrid** — own the hardware for quality control and pricing power, but lead with the software/developer experience layer (L3–L5). Analogous to Lambda Labs' model: "built by ML engineers for ML engineers."

### Core Differentiators
1. **Zero egress, flat-rate pricing** — publish all prices publicly, no "talk to sales," no surprise egress fees. This alone disqualifies AWS/Azure for price-sensitive startups.
2. **Pre-configured managed endpoints** — vLLM, ComfyUI, Axolotl (fine-tuning) deployable in one click. The DX layer is where Hyperscalers lose to Lambda.
3. **Fine-tuning pipeline as a product** — managed QLoRA/LoRA fine-tuning with private data isolation, model versioning, and inference endpoint packaging. Target the "we just hired our first ML engineer" firm.
4. **European sovereignty** — GDPR compliance, EU data residency, appeal to European AI-native startups underserved by US-centric neoclouds.

### Go-to-Market
- Direct / product-led: pricing calculator, docs-first, free trial tier (SkyPilot-style frictionless access)
- Partnership: Hugging Face integration (model hub → one-click deploy on our infrastructure)
- Target geography: EU-first (regulatory tailwind + underserved neocloud market vs. US-heavy CoreWeave/Lambda)

### What to Avoid
- Competing on cluster scale (>256 GPU clusters) — this requires capex that conflicts with clean-slate economics
- Enterprise compliance (SOC 2, HIPAA) in v1 — takes 12–18 months and diverts engineering

---

## 7. Perspective B — ICN + IC Integrated Play (Reverse Playbook)

*Assume: IC's S3 storage platform and ICN's token-based GPU network are existing assets. How do we position to maximise their combined value?*

### The Core Thesis: Data Gravity + Zero Egress = TCO Weapon

IC already hosts training data at ~$7/TB/month with zero egress. CoreWeave charges ~$30/TB/month. At 1 PB of training data:
- CoreWeave storage cost: **$30,000/month**
- IC storage cost: **$7,000/month**
- Monthly saving: **$23,000** — before any egress fees on data movement

For a fine-tuning SME running 3–6 GPU training jobs per month, storage cost savings alone can exceed compute costs. This is the TCO argument that opens the door.

The product proposition: **bring your compute to your data, not your data to compute.**

IC's "Nitro" product (high-performance S3 with tuned throughput for GPU workloads) is the technical enabler. Checkpointing during training — where GPUs sit idle while moving data to external storage — is eliminated when storage is local to compute.

### IC's Current Signal
Real customer: 600TB → 1PB training dataset currently stored on IC, compute running on CoreWeave (175 GPUs). This customer is the exact target: they would benefit from collocated IC S3 + ICN compute, and they already have a relationship with IC. This is the first land-and-expand play.

### Positioning for ICP-4 (Fine-Tuning SME)
- **Hook:** "Your training data is already on IC. Pay 4x less and run your fine-tuning jobs here too."
- **Product:** managed fine-tuning endpoints (Axolotl/QLoRA) backed by Blackwell 6000 nodes, collocated with IC S3. 12-GPU PoC → 64-GPU production pathway.
- **Data residency:** proprietary data never leaves IC/ICN infrastructure. This is the compliance argument for SMEs who can't use hyperscalers.
- **Agentic storage play:** as agentic workloads generate context, history, and output at scale, IC's S3 becomes the natural long-term storage tier. Vector databases writing to S3 (LanceDB, LlamaIndex's S3 backend, LambdaDB) are a natural product integration.

### Positioning for ICP-2 (AI-Native Startup)
- **Hook:** "Zero egress, always. Your model artefacts and training checkpoints are free to move."
- **Product:** on-demand inference endpoints + fine-tuning + IC S3 storage, bundled at one transparent price.
- **TCO calculator:** build a public tool that shows CoreWeave vs ICN/IC total cost at various data scales. At 100TB+, the gap is visible; at 1PB, it's decisive.

### The ICNT Token Tension
ICN's token (ICNT on Base/Ethereum) creates a real positioning conflict:

| Customer Segment | Token Requirement | Outcome |
|-----------------|-------------------|---------|
| ICP-4 (Enterprise SME) | Refuses to touch crypto | Cannot use DePIN-native pricing → need fiat product |
| ICP-2 (AI startup) | Technically capable but operationally prefers fiat | Will accept fiat if available |
| DePIN / Web3 native | Requires token, loves the narrative | Incompatible with enterprise ICP |

**Recommendation:** ICN leadership must explicitly choose one of:
1. **Dual-track product**: fiat-priced compute product (IC brand) + ICNT-priced network (ICN brand) — separate GTM, separate ICPs
2. **Enterprise-first fiat**: suppress ICNT in customer-facing positioning, use it as internal settlement only
3. **DePIN-first**: accept that enterprise ICP is not the target and double down on Web3/DePIN native customers

This is the most critical unresolved strategic question. The document cannot resolve it — it requires a leadership decision between Sebastian, Leo, and the IC team.

### IC's Reverse Playbook vs. Hyperscaler Data Gravity
Hyperscalers win by making data egress expensive and ecosystem exit painful. IC can win the inverse: make storage so cheap that compute providers cannot match the TCO, and add compute to the storage (not the other way). The moat is not the compute — it is the data already sitting on IC's platform.

As IC's customer base grows in storage, the "land" becomes: large enterprise data → IC S3. The "expand" is: once data is there, converting storage customers to compute customers is a natural cross-sell. This is structurally analogous to how AWS made S3 sticky and then cross-sold EC2.

---

## 8. ICN Current Assets and Capabilities

| Asset | Description | Strategic Value |
|-------|-------------|-----------------|
| IC S3 Storage | ~$7/TB, zero egress, petabyte-scale | Core TCO weapon |
| Nitro | High-performance S3 product for GPU workloads | Eliminates checkpointing idle time |
| ICNT Token | On-chain settlement, Base/Ethereum | DePIN positioning, also a GTM constraint |
| PoC Hardware | Blackwell 6000 GPUs, MIG-capable | Inference + small fine-tuning workloads |
| IC Customer (600TB→1PB) | Real training data customer currently on CoreWeave | First land-and-expand target |
| ICN/IC team | Darran (product), Thomas, Alex, Mary (GPU product) | Technical capability in storage + product |
| Network | `icn-protocol` GitHub org, web infrastructure | Technical foundation |

### Current Gaps
- **InfiniBand / high-speed interconnect**: critical for >32-GPU training; status unclear
- **Managed orchestration layer** (Kubernetes/Slurm): required to compete above bare-metal
- **Compliance posture** (SOC 2, ISO 27001): required for ICP-1 and regulated ICP-4
- **Developer experience layer**: vLLM, Axolotl, ComfyUI pre-configured endpoints
- **Geographic expansion**: current data center locations relative to customer demand unclear

---

## 9. Strategic Options Matrix

| Option | Business Model Tier | ICP | Capital Required | Time to Revenue | Defensibility | Risk |
|--------|-------------------|-----|-----------------|----------------|---------------|------|
| A1: Orchestrator-only (no owned hardware) | Orchestrator | ICP-2, ICP-4 | Low | Fast | Low-medium | Margin erosion as hardware commoditises |
| A2: Neocloud-lite (own small cluster, EU focus) | Neocloud | ICP-2, ICP-3 | Medium-High | Medium | Medium | Capital intensity, compete with CoreWeave |
| B1: Storage-adjacent inference (IC data gravity) | Neocloud-lite hybrid | ICP-4, ICP-2 | Medium | Medium | High (data moat) | Execution complexity, token tension |
| B2: Fine-tuning platform on IC storage | Orchestrator + storage | ICP-4 | Low-Medium | Fast | High (data gravity) | Requires IC commercial alignment |
| B3: DePIN GPU marketplace + ICNT | Aggregator/DePIN | Web3/DePIN native | Low | Medium | Medium (network effects) | Enterprise-incompatible ICP |
| B4: Sovereign EU inference + storage | Neocloud + compliance | ICP-1, ICP-4 | High | Slow | Very High | Compliance build-out timeline |

---

## 10. Recommended Positioning Thesis Per Perspective

### Perspective A — Recommended: Option A2 (EU Neocloud-lite)
**Thesis:** "The ML-native cloud for European AI builders — bare metal performance, zero egress, one-click fine-tuning, GDPR by default."

Target: AI-native startups and fine-tuning SMEs in Europe who are underserved by US-centric neoclouds. Differentiate on: pricing transparency, developer experience, European data residency, and managed fine-tuning endpoints. Compete on the DX layer, not on cluster scale.

The EU AI Act and ongoing GDPR enforcement create a durable regulatory tailwind that CoreWeave and Lambda are not structured to capture as efficiently as a EU-native operator.

### Perspective B — Recommended: Option B2 (Fine-Tuning Platform on IC Storage)
**Thesis:** "Your training data is already on Impossible Cloud. Fine-tune your models there — zero egress, 4x cheaper storage, your data never leaves."

Target: ICP-4 — fine-tuning SMEs whose training data is (or will be) on IC S3. The land-and-expand model: win on storage economics, cross-sell compute. The first target is IC's existing 1PB customer currently running on CoreWeave.

Short-term: 12–64 GPU PoC with Blackwell 6000 + Nitro (high-perf S3 feed), managed Axolotl/QLoRA endpoint, fiat pricing.  
Medium-term: expand to inference-as-a-service for the models fine-tuned on IC, creating an end-to-end "train on IC, serve on IC" flywheel.  
Long-term: as agentic workloads scale, IC S3 becomes the natural long-term memory tier. The data gravity moat deepens.

**The critical prerequisite:** resolve the ICNT token / fiat pricing decision before any enterprise sales motion. This is not a technical problem — it is a leadership and commercial alignment decision that must be made between ICN and IC.

---

## Open Questions (as of 2026-04-09)

1. **Token / fiat decision**: does ICN intend to offer fiat-priced GPU compute for enterprise ICP-4 and ICP-2, or is ICNT the required payment rail? This is the single highest-priority strategic decision.
2. **IC data center locations**: where are IC's PoPs? Do they have liquid cooling and sufficient power density for B6000 GPU racks?
3. **IC alignment**: is there a formal commercial agreement between IC and ICN for storage-adjacent compute, or is this still exploratory? Darran's PoC is IC-led — how does ICN fit into that?
4. **Network stack**: does ICN's PoC plan include InfiniBand or high-speed RoCE, or is it Ethernet only? This determines whether ICP-3 (foundation model labs) is achievable.
5. **Compliance roadmap**: SOC 2 Type II is a 12–18 month investment. Is this scoped into the roadmap for ICP-4 (regulated SMEs)?
6. **ICN's existing GPU network**: what nodes does ICN currently operate or aggregate? How does the ICNT staking/provider model work in practice?

---

*This document is maintained by J. Skifter (jskifter@icn.global). Last updated: 2026-04-09.*
