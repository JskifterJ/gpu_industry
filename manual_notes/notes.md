# **Notes (14/03)**
- SLUNK/Soperator only relevant for large-scale MLops/training workloads across thousands of GPUs across several days/weeks 

# **Notes (04/03)**

- Specialized brokerage; information assymmetry and connections \-\> could be a viable option but would require better people on the market-making/deal side  
- Secondary market still highly lucrative from 2022/23 contract expirations of e.g. H100s \-\> inference demand outstripping new supply  
  - Grey market platform discounts fraught with warranty voids, locked hardware and themal degraded hardware \-\> need for verification (infomration technology asset disposition (ITAD))  
  - Bridge the trust gap  
- Offering compute as bare metal potentially has lower yield than adding deployment options directly  
- Platforms \-\> targeting shorter-term contracts that want to avoid multi-year SLAs  
- Financialization of compute: derivatives and hedging  
  - Asset-backed private credit fuelling capex (USD 7.5bn coreweave debt facility w. Blackstone)  
  - Price indices are for raw cards rather than server installation \+ software access on top  
  - Architect financial technologies: exchange-traded futures contracts related to compute and memory pricing  
- How to position?  
1. Arbitrage value cascade and secondary markets \-\> aggressive sourcing at mid-point of lifecycle \-\> need offtakers in datacenter proviers if we want to stay asset-light  
2. Source from areas with structural energy surpluses  
3. Offer fixed-price forward contracts for compute capacity to absorb spot-market volatility \-\> act as **market maker,** leasing raw capacity at floating rate, offering fixed SLAs to end-users, heding through derivatives

**Clustermax**

- Rating: plat \-\> gold \-\> silver \-\> bronze  
- Importance of backend interconntect, shared storage amounts, connection speeds etc. in terms of trust in provider and churn  
- 4th criterion: storage \-\> why not become the cheapest, highest-performing S3 object-storage partner for the compute clouds?

Latest GPU Models Available (H100, H200, B200, MI300X, MI325X, MI355X)  
Roadmap for Future GPUs (B300, GB200 NVL72, GB300 NVL72, VR, MI400, etc.)

- 

# **Notes (27/02)**

[https://www.bentoml.com/blog/how-to-vet-inference-platform](https://www.bentoml.com/blog/how-to-vet-inference-platform)

- One early decision point involves the platform’s core approach:  
  - **Model-centric** platforms focus on pre-packaged endpoints with limited customization.  
  - **Code-centric** platforms let you integrate your own models, serving logic, and optimizations directly into the stack.  
    - For enterprises that need flexibility, code-centric platforms typically provide more control over performance, cost, and compliance.  
    - Native integration of hyperscalers and neoclouds \+ on-prem support for compliance  
    - **Bottom line**: Ideal for enterprises that need   
      - deep customization,   
      - advanced performance tuning, and   
      - true deployment portability, with full BYOC, native integration across all major hyperscalers and NeoClouds, and   
      - on-prem support for compliance.  
- Key indicators of flexibility include:  
  - Support for any model or framework, not just the vendor’s ecosystem  
  - Multiple serving modes, from real-time APIs to async jobs, batch processing, and multi-model pipelines  
  - Bring Your Own Cloud (BYOC), on-prem, and multi-cloud/hybrid deployment options  
- At scale, efficiency drives ROI. Prioritize platforms with:  
  - Auto-scaling to match demand without over-provisioning  
  - Cold start minimization for faster response times  
  - Distributed inference support for large models  
  - LLM-specific features like KV cache management and speculative decoding  
- 

![][image1]

![][image2]

# **Notes (26/02)**

- Bitcoin mining companies \-\> transition to storage capacities / gpu compute for inference workloads  
- Targeting listed companies with underperforming ASIC hardware \-\> transition to GPU compute, directly listed on our platform \-\> integration into incentive system of ICNT for GPU compute  
- Build an interpretation of the current GPU market across geographies and business models, placing companies into those buckets on the Miro  
  - Also place company websites to easily showcase USP \-\> perhaps this can be done dynamically so that it updates?  
  - Note any company details with legends? Founding, revenue, employees, profitability etc.

**Example of products** (for checkmarks in big sheet with competitors) \- separate from where in the stack they operate.

![][image3]

# **Notes (25/02)**

- Verifiable compute vs. consumer-grade spoofing  
- H100 PCIe vs. SXM5 (NVLink, NVSwitch \-\> interconnect bandwidth) (Server PCI Express Module)  
- Delivery methods  
  - Bare metal  
  - VMs (AWS, Azure; noisy neighbor: workload impacts each other)   
  - Container/Kubernetes  
- Classification (verified through internet searches; Reddit, X):   
  - Asset-light  
  - Asset-heavy  
  - Hybrid 

**North America: challengers and orchestrators**

- **Asset-heavy (neoclouds)**  
  - Coreweave:  
    - Debt-financed   
    - Direct chip procurement  
    - Exclusivity; B200 deployments  
    - Contracts: massive enterprise contracts  
  - Lambda Labs  
    - Own and operate colocation space and hardware  
    - Verified internal fleet  
    - Constrained by availability  
    - Transparent pricing  
  - Massed Compute  
    - Asset-heavy  
    - Bare metal focus  
  - Vultr  
    - asset-heavy   
    - Bare metal and virtualized instances  
  - Crusoe  
    - Asset heavy  
    - ESG value proposition  
  - Together AI  
    - Hybrid (cloud \+ inference)  
    - Inference API → AI factories  
    - Software layer for model serving  
- **Aggregator / orchestrators (asset-light)**  
  - FluidStack  
    - Tier 2-3  
    - Atlas OS to manage supply   
    - Market of markets \-\> supply chan risk  
    - Bare metal vis Atlas OS  
    - Opaque pricing \-\> must negotiate with suppliers  
  - Shadeform  
    - Unified API access to 15 other providers selling excess capacity (Lambda, vulr etc.)  
    - Arbitrage and convenience \-\> spot pricing  
    - Praised on reddit for cost comparison  
  - SF Compute  
    - Spot market broker  
    - Market for buying time on clusters  
  - RunPod  
    - Hybrid:   
      - Vetted tier 3-4 centers \+ community cloud marketplace  
    - Container virtualization  
  - Modal

**EU sovereign cloud ecosystem**

- Nebius  
  - Asset-heavy  
  - Nvidia reference platform partner  
  - Premier quality  
  - Compliance   
- Datacrunch  
  - Hyperscaler of nordics  
  - Asset-heavy  
  - Unbeatable price-performance  
- Northern Data  
  - Asset-heavy  
  - Bitcoin mining giant \-\> now HPC and AI  
  - Distribute through Gcore  
- Civo  
  - Asset-heavy  
  - K3s   
  - Transparent pricing  
- Hostkey  
  - Asset-heavy   
  - Bare metal  
  - Mix of consumer and professional  
- Exoscale  
  - Privacy and security \-\> ‘Swiss safe haven’ as value proposition

**DePIN and decentralized**

- [Io.net](http://Io.net)  
  - Aggregates from independents  
  - controversial : fake gpu crisis  
  - needs physical verification or cryptographic proof (Gensyn)  
- Gensyn  
  - Solves lazy worker problem   
- Marketplaces  
  - Akash   
    - Kubernetes  
    - No bare metal \-\> are we able to do bare metal at all?  
  - Aethir  
    - Enterprise focus now  
    - Aggregates underutilized supply from telcos  
  - Salad  
    - SOC 2 certified dispute running on consumer hardware  
  - [vast.ai](http://vast.ai)  
    - Web2 marketplace  
    - Market floor  
    - Low compliance  
  - Tensordock  
    - Managed marketplace  
    - Independent hosts