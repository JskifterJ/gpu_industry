# Trend notes
1. The first and most critical trend is the **collision between artificial intelligence compute scaling and physical energy constraints**, widely referred to as the "Energy Wall." Datacenter real estate and raw silicon availability are no longer the primary bottlenecks in the AI supply chain; access to reliable, gigawatt-scale electrical power has superseded them.7 Global data center electricity consumption is projected to more than double by 2030, with AI workloads serving as the nearly exclusive catalyst for this expansion.7 A single frontier-model training cluster utilizing next-generation NVIDIA architectures can draw between 40 to 100 Megawatts of power. Future infrastructure expansion will inevitably decouple from traditional fiber-optic municipal hubs, tethering instead directly to isolated, massive power sources. Sovereignty—both in terms of localized data compliance and independent energy generation—has evolved from a secondary compliance metric into a primary, non-negotiable selection criterion for AI infrastructure procurement.  
2. Macro-trend: c**ommoditization of bare-metal GPU** provisioning and the subsequent rise of orchestration as the ultimate competitive moat. As high-end silicon like the NVIDIA H100 becomes more widely accessible across all market tiers, simply providing raw access to GPUs is insufficient for securing sticky, high-margin enterprise contracts. Neoclouds are aggressively moving up the software stack, focusing on the complex integration of traditional High-Performance Computing job schedulers with cloud-native orchestration systems. The ability to offer seamlessly managed Kubernetes and Slurm environments without incurring a "hypervisor tax" has become the primary technical differentiator for capturing massive, distributed training workloads.  
3. For frontier model pre-training, the hypervisor virtualization tax imposed by legacy hyperscalers is no longer acceptable to top-tier AI laboratories. Enterprises will systematically migrate massive, synchronized training workloads to bare-metal, AI-native Neoclouds that possess advanced L3 orchestration, such as SUNK or highly tuned Slurm environments.  
4. **Financialization of the AI infrastructure market**, which has created a deeply interconnected, and potentially fragile, "circular economy." Institutional capital has flooded the Neocloud sector, but the capital expenditures required to remain competitive are staggering. An AI-ready data center can cost between $10 million and $12 million per megawatt to construct, nearly double the cost of a conventional facility.10 To finance this, a dynamic has emerged where dominant silicon vendors inject capital directly into infrastructure providers, who subsequently utilize those funds to secure exclusive allocations of the vendor's next-generation hardware. While this accelerates capacity deployment, grassroots developer sentiment and retail financial analysis reveal profound skepticism regarding the sustainability of these valuation multiples. Market analysts argue this represents a game-theory-driven buildout, heavily reliant on downstream AI application revenues eventually crossing a massive multi-trillion-dollar threshold to amortize the infrastructure debt.  
5. Macro-trend: is specialization of the TPU cores that e.g. Google are heavily pursuing, seeking further lock-in through proprietary hardware which requires a matching proprietary software stack on top → good for optimization but hurts versatility; not something open-source community is a fan of, probably.  
6. Macro-trend: LLM usage and the shift toward agentic workloads with its agentic subworkloads within it; assuming this trend continues and more agentic adoption increases as both personal and enterprise guardrails are put in place, this will require massive compute to match demand.  
7. As the generative AI market matures, the primary revenue battleground is shifting rapidly from raw hardware aggregation to inference efficiency. Providers that have successfully architected proprietary L5 model serving platforms will capture the durable, sticky operational expenditures of the global enterprise market.
8. more fine-tuning done by knowledge-intensive firms who will start hiring more AI researchers from frontier labs/research to setup their own proprietary models to train on propritary data, using open-weight models as a baseline. (Lex Friedman podcast)
9. Metrics used: moving from card-hour to more output/consumption-based pricing -> i.e., what is the actual output one can expect for different model configurations since this thus accounts for the different hardware contstraints of the system (bandwidth, memory, storage, networking). Problem with this, however, is that the software sitting atop the hardware also makes a large difference in terms of how efficiently different configurations run (recall the GPU compute cost/time configurator from session 1 (pres1_gpu_foundations.html)). Secondly, the cost per x amount of output tokens also doesn't account for the time to those x amount of output tokens, though time will eventually scale highly correlated to the cost since more time = higher opportunity cost (rackspace, power, cost per card-hour etc.). 

Bottlenecks; from compute to memory, storage and networking
AI KPI: “tokens per watt”

# Jevons paradox: a critical concept to understand how all efficiency-gains, hardware and software-agnostic, inevitably lead to more consumption.
Jevons Paradox/Law: "as technology increases the efficiency with which a resource is used, the total consumption of that resource increases rather than decreases" -> what's happening to AI compute; everyone is releasing optimization software (latest; Google TurboQuant (26-03-2026)) whose impacts are massive on industry-scale in terms of the interplay with the underlying hardware constraints and requirements to run large inference workloads. Examples: "Computing: Faster, more energy-efficient computers and data centers have led to increased overall energy consumption due to higher demand for AI and digital services. Transportation: Improved fuel efficiency in vehicles hasn't reduced total fuel consumption because people drive more miles."

Problems on the horizon related to security: how will gpu operators flag and limit the utilization of massive compute power sourced by companies/individuals who want to use the compute for illicit activities such as hacking? 

# Important gpu-related news: both on optimizations across the stack (hardware, software, drivers, agents, hacking/vulnerabilities, security)
## Summary of the LiteLLM Supply Chain Compromise
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

# Slide ideas
no business model is necessarily better; it’s a matter of what they serve and market efficiency
where each business model is heading; what can we expect going forward? What steps are they taking? In terms of stack, in terms of offerings in the stack'
Illustration (flow between illustrations of layers/components in each layer) of how each business model is developing - dynamic.

