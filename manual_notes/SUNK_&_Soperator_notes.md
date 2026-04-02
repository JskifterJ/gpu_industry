### **Slurm v. Kubernetes**
- AI researchers love slurm → HPC, good queuing
- platform engineers love K8s → auto-scaling, fault tolerance, container orchestration, hardware health management
    - but difficult to use for researchers and terrible at **gang scheduling**
        - gang scheduling = distributed job *only* starts when all required GPUs are available simultaneously

### **Why SUNK/Soperator?**
- researchers get familiar Slurm backed by resilient auto-healing K8s
    - =
        - less operational overhead for bespoke environments
        - higher ‘goodput’ (if K8 detects failure, slurm auto-re-queues and restarts job from last checkpoint)

### **Relevant for**
- pre-training
    - K8s fault tolerance is mandatory in combination with Slurm’s gang schedulihng
- fine-tuning
    - tuning frontier models still requires multi-node, distributed setups
        - enables **resource sharing** among researchers in AI lab
        - enables **environment consistency** with shared **root filesystem**
- inference (contextual relevance)
    - inference requires stateless, auto-scaling REST APIs → what K8s was built for
    - with SUNK/Soperator, AI labs can easily partition hardware dynamically release GPUs back to native K8s to host inference APIs - all on the same underlying cluster

### **1. Architectural Differences: SUNK vs. Soperator**

| **Feature** | **CoreWeave SUNK (Slurm on Kubernetes)** | **Nebius Soperator (Open Source)** |
| --- | --- | --- |
| **Philosophy** | **Proprietary & Vertically Integrated.** Deeply tied to CoreWeave’s specific hardware stack. | **Open Source & Agnostic.** Designed to be portable across different K8s environments. |
| **Networking** | **DPU-Centric (BlueField-3).** Offloads networking/security to hardware to ensure 100% "bare metal" throughput for GPUs. | **Standard InfiniBand/RDMA.** Uses high-speed interconnects but generally relies on standard software drivers within the K8s layer. |
| **Storage** | **Disaggregated VAST Data.** High-performance storage is separated from the compute fabric via dedicated DPU links. | **Shared Root Filesystem.** All nodes share a common `/` directory (often via Lustre or NFS) to mirror an on-prem HPC experience. |
| **Scheduling** | **Topology-Aware + "Mission Control."** Predictive failure detection replaces "straggler" GPUs before they crash a job. | **Topology-Aware + Slurm Native.** Uses Slurm’s native topology features integrated with K8s for optimal placement. |