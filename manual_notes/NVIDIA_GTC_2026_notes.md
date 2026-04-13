Keynote: Jensen Huang
3 platforms: CUDAx, Systems and AI Factory
5 layers of AI
Drives down computing cost -> GeForce is the start to all developers; later enterprise customers
GeForce got CUDA to the world; started the big bang of AI: programmable shading
Neural Rendering: fusing controllable (structured information) 3d graphics with generative AI (probabalistic computing)
Structured data (the ground truth) = Sequel, Pandas, Snowflake, Azure Fabric, Databricks, Oracle DB, 
Unstructured data = PDFs, vectorized, no easy indexing of unstructured data
Accelerated data processing -> integrations and partnerships with enterprises. NESTLE for supply chain, Google Cloud acceleration of Vertex, accelerate AWS, 
NVIDIA built the accelerated compute platform: integrate into OEMs 
Confidential computing: accelerating workloads across clouds to protect proprietary models of e.g. Anthropic.
"vertically integrated, horizontally open -> integrate into any system and technology to bring accelerated computing to everyone and all applications"
TelCo -> AI will run on basestations to run at the edge.
"AI Natives" -> category of companies that serve different verticals of LLMs
Everything will revolve around tokens: creators and consumers of them 
New computing platform shift; who will be the Amazon's of the new world? 
Computing moved from retrieval to generation -> alters how computing must be designed.
Inference inflection has arrived (?) - sure? We're in the field of inference, reasoning now - less training. We're compute-bound. 
Useful life of GPUs -> lowest cost infra you can get in the world. 
Open-source models have reached the frontier. 
50% of business is hyperscalers. -> moving to LLM workloads.
NVFP4 - completely different physical infrastructure required.
Token factories (Together.AI, Eigen AI, Clarifai, Baseten, Fireworks)
Token will be the new commodity of the future. k
Kyberrack enables fitting 144 GPUs on one NVlink system
Token commodity has transitioned into different segments: tiering of millions of token outputs.
From free tier to medium to high to premium tiers in terms of the quality of output tokens. 
United the Vera Rubin GPU and Groq to pair the prefill and decode chips in a single chip.
AI needs CPUs for tool-use -> why NVIDIA now expanded into CPU.
Storage will be a limiting factor 
Racks: Oberon (NVL72) -> Kyber (NVL144, NVL1152)
AI factory = tokens/watt as the performance KPI
Omniverse -> will hold the world's digital twin.
NVIDIA DSX: AI Factory platform
OpenClaw: manages resources, accesses tools, able to schedule (Cran jobs), IO (any modality), -> an operating system? The operating system of agentic systems.
Enterprise IT renaissane from SaaS to Agent-as-a-Service -> SaaS will become a AGaaS.
NemoClaw: OpenClaw with enterprise security; enterprise-ready. Connect policy-engines with all the SaaS companies in the world.
HuggingFace = open models
Tokens are going to be a more prevalent access parameter for engineers than money. Tokens will be the proxy for aceess to power. As big of a deal as Linux. 
NemoClaw

## Gemini recap
NVIDIA’s GTC 2026 keynote delivered by Jensen Huang marked a massive strategic pivot. The core narrative shifted from simply building artificial intelligence (training) to running it at industrial scale (inference, AI agents, and physical robotics). Nvidia is no longer pitching itself just as a chip company, but as an "AI infrastructure and factory operator."If you are looking to build graphs, the data below highlights the massive leaps in compute density, cost-efficiency, and market projections announced at the event.Here are the key takeaways, broken down with the hard specs and data you need.1. Market Projections (Data for Trend & Bar Graphs)Nvidia effectively doubled its market outlook, signaling that demand for compute is dramatically outpacing traditional infrastructure cycles.Market MetricPrevious Estimate (2025)GTC 2026 Revised EstimateGlobal AI Infrastructure Demand (2025–2027)$500 Billion$1 Trillion+

2. The Vera Rubin Architecture (Data for Comparative Bar Charts)
The successor to Blackwell is the Vera Rubin platform, which is purpose-built for massive Mixture-of-Experts (MoE) models and agentic AI workloads. The flagship system is the Vera Rubin NVL72, which integrates 72 Rubin GPUs and 36 Vera CPUs via the new NVLink 6 network.

Here is the generational leap data you can chart against the previous Blackwell architecture:
Performance Metric,Blackwell (Previous Gen),Vera Rubin NVL72 (New Gen),Delta / Improvement
AI Compute Density,1x (Baseline),3x to 4x,+200% to +300%
GPUs Required (1T-Parameter MoE),4x,1x,75% Reduction
Cost Per Token Generation,10x,1x,90% Cost Savings
GPU-to-CPU Data Latency,Milliseconds,Microseconds,Order of magnitude drop

3. Hardware Innovations & Inference Specs (Data for Radar/Performance Charts)
Nvidia directly addressed the bottleneck of "ultra-high-speed inference" by debuting new hardware—most notably leveraging its recent acquisition of Groq.
Hardware Component,Core Function,Key Specifications & Graphable Data
Vera CPU Rack,Agentic AI processing,"256 processors per rack; 2x computing efficiency over traditional CPUs; Can support 10,000+ simultaneous online agents."
Groq 3 LPU (LP40),Ultra-high-speed inference,500MB on-chip SRAM; 35x higher inference throughput for 1-trillion parameter models at the same power level.
Spectrum X CPO,Advanced Networking,Co-packaged optics deliver 2Tb/s single-port bandwidth; 5x improved optical power efficiency; 10x network reliability.
Neotron 3 Super,Edge AI / Local Inference,Up to 10x performance boost for Small Language Models (SLMs) like the Nemotron Nano 9B running locally.