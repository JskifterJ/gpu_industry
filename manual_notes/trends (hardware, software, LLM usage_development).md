# General notes
Bottlenecks; from compute to memory, storage and networking
AI KPI: “tokens per watt”
We need to somehow address a few different scenarios for positioning; 1) entirely clean slate, as if we had no prior reservations/priorities and were entering the market as a well-funded startup and 2) taking into account the key resources and capabilities of IC (fast, cheap S3 storage) and ICN and the synergies between the two organizations (see manual_notes -> interview_notes for context with what IC is considering from their perspective).
It would be cool to develop a final a market positioning interface with an overview of the different existing and upcoming business models, their key strengths, moats, how well they respond to the future market trends covered, what key resources and capabilities they require etc.

# Trend notes
1. The first and most critical trend is the collision between artificial intelligence compute scaling and physical energy constraints, widely referred to as the "Energy Wall." Datacenter real estate and raw silicon availability are no longer the primary bottlenecks in the AI supply chain; access to reliable, gigawatt-scale electrical power has superseded them.7 Global data center electricity consumption is projected to more than double by 2030, with AI workloads serving as the nearly exclusive catalyst for this expansion.7 A single frontier-model training cluster utilizing next-generation NVIDIA architectures can draw between 40 to 100 Megawatts of power. Future infrastructure expansion will inevitably decouple from traditional fiber-optic municipal hubs, tethering instead directly to isolated, massive power sources. Sovereignty—both in terms of localized data compliance and independent energy generation—has evolved from a secondary compliance metric into a primary, non-negotiable selection criterion for AI infrastructure procurement.  
2. Macro-trend: commoditization of bare-metal GPU provisioning and the subsequent rise of orchestration as the ultimate competitive moat. As high-end silicon like the NVIDIA H100 becomes more widely accessible as long-term hyperscaler and neocloud contracts near end-of-contract life, across all market tiers, simply providing raw access to GPUs is insufficient for securing sticky, high-margin enterprise contracts. Neoclouds are aggressively moving up the software stack, focusing on the complex integration of traditional High-Performance Computing job schedulers with cloud-native orchestration systems. The ability to offer seamlessly managed Kubernetes and Slurm environments without incurring a "hypervisor tax" has become the primary technical differentiator for capturing massive, distributed training workloads. Notably, we should thus expect a big inflow of H100/200 over the next period and can expect a similar cycle to occur for newer chip generations (B200, B300). Perhaps one can visualize this lifecycle of GPU generations in terms of timescale and 1. workloads, 2. contract lock-ins/length/ending (when they're 'consumed' by hyperscalers and neoclouds and when they jump to the next generation -> applying multi-year contract length to assess when they will 'flood' the market. Perhaps we can also note where orchestrators and aggregators fit into this picture; which chips/nodes do they offtake and when during the hyperscaler/neocloud contract window?). 
3. For frontier model pre-training, the hypervisor virtualization tax imposed by legacy hyperscalers is no longer acceptable to top-tier AI laboratories. Enterprises will systematically migrate massive, synchronized training workloads to bare-metal, AI-native Neoclouds that possess advanced L3 orchestration, such as SUNK or highly tuned Slurm environments.  
4. Financialization of the AI infrastructure market, which has created a deeply interconnected, and potentially fragile, "circular economy." Institutional capital has flooded the Neocloud sector, but the capital expenditures required to remain competitive are staggering. An AI-ready data center can cost between $10 million and $12 million per megawatt to construct, nearly double the cost of a conventional facility. To finance this, a dynamic has emerged where dominant silicon vendors (NVIDIA) inject capital directly into infrastructure providers, who subsequently utilize those funds to secure exclusive allocations of the vendor's next-generation hardware. While this accelerates capacity deployment, grassroots developer sentiment and retail financial analysis reveal profound skepticism regarding the sustainability of these valuation multiples. Market analysts argue this represents a game-theory-driven buildout, heavily reliant on downstream AI application revenues eventually crossing a massive multi-trillion-dollar threshold to amortize the infrastructure debt.  
5. Macro-trend: hyperscaler custom silicon. e.g. of TPU cores that Google are heavily pursuing (AWS: Trainium, Inferentia), seeking further lock-in through proprietary hardware which requires a matching proprietary software stack on top → good for optimization but hurts versatility; not something open-source community is a fan of, and hard to build on top of if not in the Google/Amazon/Microsoft ecosystem.  
6. Macro-trend: LLM usage and the shift toward agentic workloads with agentic subworkloads within it; assuming this trend continues and more agentic adoption increases as both personal and enterprise guardrails are put in place, this will require massive compute to match demand across a range of different underlying compute requirements, according to agent, task and context window.
7. As the generative AI market matures, the primary revenue battleground is shifting rapidly from raw hardware aggregation to inference efficiency. Providers that have successfully architected proprietary L5 model serving platforms will capture the durable, sticky operational expenditures of the global enterprise market.
8. more fine-tuning done by knowledge-intensive firms who will start hiring more AI researchers from frontier labs/research to setup their own proprietary models to train on propritary data, using open-weight models as a baseline. (Lex Fridman podcast with Nathan Lambert and Sebastian Raschka (ML researchers and engineers))
9. Metrics used: moving from card-hour to more output/consumption-based pricing (Huang, GTC 2026: "tokens per watt" -> i.e., what is the actual output one can expect for different model configurations since this thus accounts for the different hardware contstraints of the system (bandwidth, memory, storage, networking). Problem with this, however, is that the software sitting atop the hardware also makes a large difference in terms of how efficiently different configurations run (recall the GPU compute cost/time configurator from session 1 (pres1_gpu_foundations.html)). Secondly, the cost per x amount of output tokens also doesn't account for the time to those x amount of output tokens, though time will eventually scale highly correlated to the cost since more time = higher opportunity cost (rackspace, power, cost per card-hour etc.). 

# Jevons paradox: a critical concept to understand how all efficiency-gains, hardware and software-agnostic, inevitably lead to more consumption.
Jevons Paradox/Law: "as technology increases the efficiency with which a resource is used, the total consumption of that resource increases rather than decreases" -> what's happening to AI compute; everyone is releasing optimization software (latest; Google TurboQuant (26-03-2026)) whose impacts are massive on industry-scale in terms of the interplay with the underlying hardware constraints and requirements to run large inference workloads. Examples: "Computing: Faster, more energy-efficient computers and data centers have led to increased overall energy consumption due to higher demand for AI and digital services. Transportation: Improved fuel efficiency in vehicles hasn't reduced total fuel consumption because people drive more miles."

Problems on the horizon related to security: how will gpu operators flag and limit the utilization of massive compute power sourced by companies/individuals who want to use the compute for illicit activities such as hacking? 

# Important gpu-related news: both on optimizations across the stack (hardware, software, drivers, agents, hacking/vulnerabilities, security)
## Importance of security at the software level: Summary of the LiteLLM Supply Chain Compromise
https://www.trendmicro.com/de_de/research/26/c/inside-litellm-supply-chain-compromise.html
On March 24, 2026, the widely used open-source Python library LiteLLM—a popular API gateway for routing calls to over 100 Large Language Models—suffered a critical software supply chain attack.

The Compromise: A threat actor group tracked as "TeamPCP" used stolen maintainer credentials (harvested from a prior attack on the Trivy security scanner) to bypass GitHub release protocols and push malicious versions (1.82.7 and 1.82.8) directly to PyPI.

The Mechanism: The malware used a hidden .pth file (LiteLLM_init.pth) that executes silently the moment the Python interpreter starts up. This meant the malicious payload triggered automatically, even if the developer never explicitly imported the library in their code.

The Payload: It functioned as a massive multi-stage infostealer and worm. It scoured the infected system for SSH keys, cloud provider credentials (AWS, GCP, Azure), database passwords, cryptocurrency wallets, and AI API keys.

Lateral Movement: If deployed in a Kubernetes environment, the malware used available service account tokens to enumerate secrets across all namespaces and attempted to deploy highly privileged pods on every node to compromise the underlying host filesystems and install persistent remote backdoors.

Why These Attacks are Prevalent for GPU Infrastructure Companies
GPU infrastructure companies (such as RunPod, CoreWeave, or Lambda Labs) are increasingly finding themselves on the front lines of these supply chain attacks. Here is why this threat is surging and how providers need to adapt to flag malicious behavior.

1. The Target-Rich AI/ML Ecosystem
GPU infrastructure primarily hosts AI developers and machine learning workloads. The AI ecosystem relies heavily on an intricate web of fast-moving, open-source Python dependencies (like LiteLLM, PyTorch, and Hugging Face libraries). Because AI developers rapidly pull in dozens of transitive dependencies to build agents and orchestration tools, the Python AI stack has become the perfect trojan horse for threat actors.

2. Extremely High-Value Assets
When an attacker compromises a workload running on a GPU cluster, they gain access to highly lucrative assets:
Compute Power: GPU compute is incredibly expensive. Attackers can hijack these servers for illicit cryptomining or training their own models for free.
Proprietary IP: AI workloads often contain highly sensitive corporate data, including proprietary model weights and specialized training datasets.
API Secrets: As seen in the LiteLLM attack, AI workloads are loaded with expensive, high-tier API keys (OpenAI, Anthropic, etc.) that can be stolen and resold.

3. Kubernetes as an Attack Surface
GPU providers heavily utilize Kubernetes to orchestrate multi-tenant GPU allocation. Supply chain malware is evolving to specifically target these orchestration layers. By deploying a malicious package, an attacker or a malicious tenant can attempt to escape their container, traverse the Kubernetes network, and gain root access to the physical GPU host machine.

## Anthropic Project Glasswing (April 2026) — frontier model capability as a security event
https://www.anthropic.com/glasswing
Project Glasswing is a coordinated cybersecurity initiative launched by Anthropic in April 2026 to use frontier AI for defensive vulnerability discovery — before malicious actors exploit the same capability.

The trigger: Claude Mythos Preview (an unreleased frontier model) demonstrated a qualitative leap in offensive cyber capability — 83.1% on cybersecurity benchmarks vs Claude Opus 4.6's 66.6%. It identified thousands of zero-day vulnerabilities across every major operating system and web browser, including a dormant 27-year-old flaw in hardened OpenBSD, and autonomously chained multiple Linux kernel vulnerabilities.

The response: rather than release the model publicly, Anthropic coordinated a controlled disclosure process with 12 industry partners — Amazon Web Services, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, Linux Foundation, Microsoft, NVIDIA, and Palo Alto Networks — plus 40+ additional organizations — to patch vulnerabilities before the model became publicly accessible. Commitments: $100M in model compute credits to security defenders; $4M to open-source security organizations; findings shared publicly so the industry can benefit.

Why this is a landmark: it is the first case of "responsible disclosure" applied not to a software bug, but to a frontier AI model's capabilities. The model itself was treated as a potential weapon that required coordinated industry patching before release. This signals that the capability trajectory of frontier models is now outpacing the security infrastructure designed to govern them.

GPU infrastructure relevance: GPU providers who host AI workloads are primary targets for exactly the class of attack Glasswing is designed to patch. The same capabilities that can discover zero-days defensively can be used offensively against GPU clusters — compromising container escape, Kubernetes secrets, and host filesystems. Glasswing accelerates the defensive posture of the entire ecosystem.

# Google Gemma 4 (openweight, lightweight model)
https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/


### Twitter recap 5 days after launch
Gemma 4 is driving a sharp “local-first” wave: multiple posts pointed to Gemma 4 becoming the top trending / #1 model on Hugging Face, with strong enthusiasm for its practical usability rather than just leaderboard performance—see @ClementDelangue, @GlennCameronjr, and @Yampeleg. The strongest signal was how quickly people were running it on consumer Apple hardware: @adrgrondin showed Gemma 4 E2B on an iPhone 17 Pro at roughly 40 tok/s with MLX; @enjojoyy reported a similar iPhone deployment; @_philschmid highlighted Gemma 4 E2B in AI Edge Gallery using skills for Wikipedia queries. Red Hat also published quantized Gemma 4 31B model cards in NVFP4 and FP8-block formats with instruction-following evals live, and reasoning/vision evals pending, via @RedHat_AI. Together these posts suggest Gemma 4 is not just another open release; it is becoming a reference point for edge inference, Apple Silicon tooling, and low-friction local deployment.

The commercial implication is pressure on paid chat subscriptions and cloud dependence: some of the more viral commentary was reductive, but it captures a real shift. @AlexEngineerAI argued that Gemma 4 running locally closes enough of the gap to make a Claude subscription less compelling for some users, while @ben_burtenshaw reminded people that HF-hosted models are free to use and can replace portions of an agent workflow. On the infra side, @ollama launched Gemma 4 on Ollama Cloud backed by NVIDIA Blackwell GPUs, making it available to tools like OpenClaw and Claude-style workflows without self-hosting. The notable ecosystem post from @osanseviero also underscored how broad the launch coordination was—HF, vLLM, llama.cpp, Ollama, NVIDIA, Unsloth, SGLang, Docker, Cloudflare and others—which is a reminder that “open model success” increasingly depends on simultaneous downstream systems support, not just weights.

### Technical specifications

**Model variants (all multimodal: images + video; E2B/E4B also support audio via USM conformer encoder):**

| Variant | Total params | Active params | Context | Primary use |
|---------|-------------|---------------|---------|-------------|
| Gemma 4 E2B | ~2.3B eff. (MoE) | ~2.3B | 128k tokens | Edge/mobile; runs at ~40 tok/s on iPhone 17 Pro with MLX |
| Gemma 4 E4B | ~4.5B eff. (MoE) | ~4.5B | 128k tokens | Edge/on-device, audio-capable |
| Gemma 4 26B A4B | 26B total (MoE) | 4B active | 256k tokens | Cloud inference, efficiency-first; 4B active = near-free inference at scale |
| Gemma 4 31B | 31B dense | 31B | 256k tokens | Frontier quality; top of open-weight class at launch |

**Architecture highlights:**
- Alternating local/global attention layers (reduces compute for long contexts without sacrificing global coherence)
- Per-Layer Embeddings (PLE): each transformer layer has its own input embedding, improving depth of representation
- Shared KV cache across attention heads (memory efficiency for long-context and multi-agent scenarios)
- Dual RoPE configurations: short-range (local attention) and long-range (global attention) positional encodings
- MoE variants use sparse routing: only a fraction of weights activated per token → inference cost tracks active params, not total

**Benchmarks (31B dense, as of April 2026):**
- AIME 2026: 89.2% (competitive with frontier reasoning models)
- GPQA Diamond: 84.3%
- MMLU Pro: 85.2%
- LiveCodeBench: 80.0%
- Codeforces ELO: ~2150
- LMSYS Arena Elo: ~1452 (top open-weight model at launch)

**License:** Apache 2.0 — fully permissive, commercial use allowed, no open-weight “community license” restrictions.

**Infrastructure / market implications:**
- The 26B A4B MoE is strategically important: frontier-adjacent quality at ~4B active-param inference cost. A GPU running 26B A4B effectively serves at the economics of a 4B model — massive throughput-per-dollar improvement for inference providers.
- Combined with the local-first wave (E2B on iPhone, E4B on consumer laptops), this compounds Jevons dynamics: more capable local inference → more agentic tasks attempted locally → KV cache and memory management become the next bottleneck even on-device.
- Red Hat published quantized versions (NVFP4, FP8-block) for enterprise inference stacks — signals enterprise-grade adoption pipeline is already in motion.
- Day-0 support across the full inference ecosystem (HF, vLLM, llama.cpp, Ollama, SGLang, Unsloth, Docker, Cloudflare Workers AI, NVIDIA) is structurally different from prior open releases — Google coordinated downstream tooling simultaneously with weights publication, compressing the time-to-production to near-zero.

# Z.ai GLM-5.1 (April 8, 2026) — frontier agentic model trained without NVIDIA
https://z.ai/blog/glm-5.1

GLM-5.1 is a post-training refinement of GLM-5, released by Z.ai (formerly Zhipu AI) on April 8, 2026. It is an open-weight 754B Mixture-of-Experts model with 40B active parameters per token, a 200k context window, and 128k output tokens. Critically: it was trained entirely on Huawei Ascend chips — no NVIDIA hardware was used.

**Key benchmarks:**
- SWE-Bench Pro: 58.4 — #1, above GPT-5.4, Claude Opus 4.6, Gemini 3.1 Pro
- AIME 2026: 95.3%
- GPQA Diamond: 86.2%
- Codeforces ELO: ~2150
- CyberGym: 68.7 · BrowseComp: 68.0 · τ³-Bench: 70.6 · MCP-Atlas: 71.8

**Agentic capability:** sustains 8-hour autonomous coding execution loops (planning → execution → testing → optimization), which puts it in a different category from typical chat/completion models.

**License:** Apache 2.0 — fully open, commercial use permitted.

**Pricing (API):** $1.00 / 1M input tokens · $3.20 / 1M output tokens

**Why this matters:**
1. Geopolitical: China now has frontier-model capability on domestic (non-NVIDIA) silicon. The assumption that frontier AI requires NVIDIA H100/B200 hardware is invalidated at scale. Export controls slow but do not stop Chinese frontier development.
2. Agentic: 8-hour autonomous execution sessions are qualitatively different from 2-minute completions — they require fundamentally different infrastructure (job scheduling, checkpointing, long-context KV cache management, robust monitoring). This is a new workload class for GPU infrastructure.
3. Open-weight competitive pressure: alongside Gemma 4 (Google), DeepSeek-R1, and Qwen 2.5, the open-weight frontier is rapidly closing the gap with proprietary models — compressing inference API margins industry-wide.