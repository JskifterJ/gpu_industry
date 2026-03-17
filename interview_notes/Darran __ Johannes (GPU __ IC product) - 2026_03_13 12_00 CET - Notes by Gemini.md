Mar 13, 2026

## Darran \<\> Johannes (GPU // IC product)

Invited [Johannes Skifter](mailto:jskifter@icn.global) [dsoothill@impossiblecloud.com](mailto:dsoothill@impossiblecloud.com)

Attachments [Darran \<\> Johannes (GPU // IC product)](https://www.google.com/calendar/event?eid=MGljcXRwNGZta2k2bGdtZnVyajhiMTczZ3QganNraWZ0ZXJAaWNuLmdsb2JhbA) 

Meeting records [Transcript](?tab=t.2f32es4v4122) 

### Summary

IC's GPU compute strategy focuses on compute service adjacent to low-cost object storage, leveraging IC's storage expertise and moving beyond object storage to satisfy investor interest.

**GPU Compute Services Overview**  
IC is determining whether to offer virtual machines, bare metal GPUs, or LLM as a service to align with customer needs and IC's competences. Offering LLM as a service would require an entire infrastructure to support building, deploying, and running models, offering greater revenue potential than bare metal GPUs.

**Storage and Compute Strategy**  
IC is developing a high-performance storage product named Nitro for efficient S3 workloads, recognizing that optimizing GPU utilization requires high-performance storage. The strategic decision involves adding compute adjacent to IC's significantly lower-cost S3 platform, eliminating egress fees for customers with inference or training workloads.

**Inference and Hardware Focus**  
A hardware stack is being developed for a Proof of Concept environment, potentially utilizing Blackwell 6000 GPUs and the Multi-Instance GPU feature for running multiple inference workloads. The product strategy is focused on achieving high GPU utilization for profitability by quickly switching and loading models to run more customer jobs.

### Details

* **Initial Conversation and Travel Context**: Darren Soothill is currently at the airport, waiting for passport control to open to get their passport stamped for leaving the country. Johannes Skifter mentions being half-English and applying for a renewal of their English passport due to new rules, referencing the bureaucracy related to using e-gates for European travel ([00:00:00](?tab=t.2f32es4v4122#heading=h.ry2eivu4pnr9)). They acknowledge that the European rules surrounding e-gates require a physical passport stamp upon entry and exit ([00:00:52](?tab=t.2f32es4v4122#heading=h.cjqc2cglb7wa)).

* **Johannes Skifter's Task in GPU Compute**: Johannes Skifter, who describes themself as non-technical, has been tasked by Sebastian to gain an overview of the current industry dynamics in GPU compute ([00:00:52](?tab=t.2f32es4v4122#heading=h.cjqc2cglb7wa)). This overview focuses on large language models (LLMs) and usage patterns, intended for presentation to the IC team to spark discussion on how to position the company in this space, considering IC competences ([00:01:44](?tab=t.2f32es4v4122#heading=h.e9k6x9cdk5v)).

* **IC's Current Approach to GPU Compute Services**: Darren Soothill provides context on IC's current exploration, which involves looking at offering a compute GPU service and validating customer needs. IC is currently trying to determine whether to offer virtual machines (like Amazon EC2), bare metal GPUs, or LLM as a service, which involves a token-based offering model distinct from ICN tokens ([00:03:00](?tab=t.2f32es4v4122#heading=h.1d0y0sbju1i9)). Offering LLM as a service would require an entire infrastructure to support building, deploying, running, and allowing people to select LLMs ([00:04:09](?tab=t.2f32es4v4122#heading=h.2qrs0mxn8nz6)).

* **Business Model Considerations for GPU Compute Offerings**: IC is trying to validate what they want to sell and what customers want to buy, which involves discussions about the target markets ([00:05:04](?tab=t.2f32es4v4122#heading=h.gt911bzahfte)). Offering bare metal GPUs to top-end enterprise customers, while possible, is viewed as a race to the bottom in terms of pricing due to the scale of competitors like Coreweave. The alternative is offering greater abstraction, such as managing Kubernetes infrastructure or standing up full LLMs, which offers more potential for increasing revenue ([00:06:04](?tab=t.2f32es4v4122#heading=h.xji60xnyvn84)).

* **Role of Storage and High-Performance Requirements**: The technical expertise of IC is primarily in storage, and optimizing GPU utilization requires specialized network and high-performance storage ([00:07:03](?tab=t.2f32es4v4122#heading=h.f57uzt48u8ar)). IC is developing a product named Nitro for high-performance S3 workloads to efficiently move data in and out of the storage platform ([00:09:02](?tab=t.2f32es4v4122#heading=h.s6z7xko8jmdr)). They recently signed a customer to use their storage platform for training data (600 terabytes growing to a petabyte), with the customer processing this data on Coreweave's 175 GPUs, effectively positioning IC as the cold storage for training data ([00:08:07](?tab=t.2f32es4v4122#heading=h.vhv3bzdj22yy)).

* **Inference Workloads and Hardware for Proof of Concept (PoC)**: Discussions are underway regarding inference workloads, and IC is developing a hardware stack for a PoC environment, potentially utilizing Blackwell 6000 GPUs ([00:09:02](?tab=t.2f32es4v4122#heading=h.s6z7xko8jmdr)). They are looking at the MIG feature (Multi-Instance GPU) supported by these GPUs to run multiple inference workloads on the same hardware, which could drive down costs for customers who do not need the full GPU capability all the time. This initiative would also require a specialist high-performance network for communication between multiple GPUs and high-speed storage, possibly utilizing high-speed NFS rather than a Ceph block storage system ([00:10:18](?tab=t.2f32es4v4122#heading=h.ywfgh3ojvwwx)).

* **Strategic Decision: Compute vs. Storage Focus**: Johannes Skifter questions whether IC should move into new GPU hardware or stick to the storage side where they have expertise. Darren Soothill explains that IC is looking to expand beyond object storage to satisfy investor interest in compute and GPUs ([00:11:15](?tab=t.2f32es4v4122#heading=h.yg9exmil3w2a)). The strategy involves providing compute services adjacent to storage, taking advantage of the significantly lower cost of IC's object storage (around \\$7/terabyte) compared to competitors like Coreweave (around \\$30/terabyte) ([00:12:29](?tab=t.2f32es4v4122#heading=h.egtvx0edbi2t)).

* **Reverse Playbook: Adding Compute to Storage**: IC is exploring the strategy of adding compute next to the storage, which eliminates egress fees and allows customers to bring their inference or training workloads to IC's infrastructure. While initial focus might be on inference, Darren Soothill suggests long-term training capabilities should not be ruled out, aiming at mid-range customers who may require smaller clusters (e.g., 10, 20, or maybe 100 GPUs) ([00:13:36](?tab=t.2f32es4v4122#heading=h.3gt5ajqcax2x)). The next step is to determine the actual needs by building a relatively small PoC environment, possibly containing around 12 GPUs ([00:16:43](?tab=t.2f32es4v4122#heading=h.jl2ef6bzbvke)).

* **Anticipating Future Storage Needs for Agentic Workloads**: Regarding the storage requirements for future agentic workloads, which will generate massive amounts of context, history, and output, Darren Soothill views this as a tiered storage solution ([00:21:57](?tab=t.2f32es4v4122#heading=h.prnoa4raoxdu)). Local high-performance storage would be needed close to the GPUs for models and initial interactions, while longer-term data could be pushed to remote, deep storage, often utilizing vector databases that write directly to S3 ([00:23:06](?tab=t.2f32es4v4122#heading=h.5y4xtu7l6u1d)). The bulk storage capacity of IC's S3 platform is suitable for storing training data output and handling vector databases that rely on local indexes ([00:20:46](?tab=t.2f32es4v4122#heading=h.p4conm7wi048)) ([00:24:11](?tab=t.2f32es4v4122#heading=h.tbn94kp0o1y)).

* **Team Leading GPU Product Strategy and Efficiency Focus**: The product strategy for the GPU service is being led by Darren Soothill, Thomas, Alex, and Mary ([00:24:11](?tab=t.2f32es4v4122#heading=h.tbn94kp0o1y)). A key focus is the efficiency of GPU usage, as models increase in size (e.g., 40-50 GB of data needing to be loaded into 192 GB VRAM GPUs) ([00:26:13](?tab=t.2f32es4v4122#heading=h.e6e45y8pzcc6)). High GPU utilization is essential for profitability, and the strategy involves being able to switch and load models quickly to run more customer jobs in less time, either increasing profit or reducing customer costs ([00:27:28](?tab=t.2f32es4v4122#heading=h.sfrgufsadalx)).

* **Software Stack and Market Dynamics**: The software stack and location of data relative to compute are becoming major differentiators, with the storage platform needing to become data-aware and smarter ([00:29:36](?tab=t.2f32es4v4122#heading=h.425pb7y0hvfi)). Darren Soothill notes the rapid change in the marketplace, with new models constantly being released and a lot of "gaming of the benchmarks" occurring ([00:37:09](?tab=t.2f32es4v4122#heading=h.uwglcxtcnvsd)). They also highlight the potential for AI to dramatically change traditional software development and workforce requirements, making this a time of massive, dynamic change ([00:38:58](?tab=t.2f32es4v4122#heading=h.8rvjsbr0ttdr)).

### Suggested next steps

- [ ] Darren Soothill will evaluate the high-level PRD document to determine if the executive summary can serve as a resource for Johannes Skifter.  
- [ ] Darren Soothill will send Johannes Skifter a link to a Lambda Labs blog post describing the changes in data location relative to compute and accessibility.

*You should review Gemini's notes to make sure they're accurate. [Get tips and learn how Gemini takes notes](https://support.google.com/meet/answer/14754931)*

*Please provide feedback about using Gemini to take notes in a [short survey.](https://google.qualtrics.com/jfe/form/SV_9vK3UZEaIQKKE7A?confid=Ngc56btuQ-w6yUIhTQE1DxIROAIIigIgABgDCA&detailid=standard)*

**Thomas Demoor**

- Fine-tuning coupled with inference  
- Mega datacenters will be placed near massive power inputs and massive cooling capabilities  
- What type of datacenters will ICN govern? Where will we move?  
- Everything around edge is the way to go \-\> edge inference; low latency  
- **Stack**  
  - Who offer which parts of the stack?  
  - Which product do we focus on?   
  - lambsDB (uses S3 as backend), vLLM \+ specific for inference \-\> interface  
  - ICP and product market fit  
    - Small BMC nodes also viable \-\> for testing/MLOps work  
  - Versus the large-scale \-\> completely different GTM action  
  - Where does the tooling overlap with S3?  
  - Kubernetes \+ LambsDB/K8 \-\> becoming an inference company  
- **Training**: checkpointing \-\> GPUs are idle during the transfer of data to external source  
- Leverage 