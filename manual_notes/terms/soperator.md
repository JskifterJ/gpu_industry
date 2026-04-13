---
term: Soperator
sphere: software
source: Nebius AI — open-sourced 2025 (github.com/nebius/soperator)
tldr: Nebius's open-source Kubernetes operator for running Slurm-based HPC/AI workloads — the publicly available equivalent of CoreWeave's SUNK, delivering ~92% goodput.
---

## Definition

Soperator is a Kubernetes operator developed by Nebius that enables running Slurm (the HPC job scheduler) natively within Kubernetes clusters. It bridges two worlds:

- **Slurm**: the standard scheduler for HPC and large-scale training jobs (gang scheduling, MPI jobs, dependency management)
- **Kubernetes**: the standard platform for container orchestration, service management, and cloud-native workloads

By running both together, Soperator allows a single GPU cluster to handle both:
1. Large distributed training jobs (gang-scheduled, bare-metal performance)
2. Inference serving, notebooks, and interactive workloads (K8s-native)

## Why Nebius open-sourced it

Nebius made Soperator open-source as a strategic move:
- Positions Nebius as the "open alternative" to CoreWeave's proprietary SUNK
- Builds community adoption and talent acquisition
- Creates an ecosystem moat — others building on Soperator infrastructure favour Nebius hardware
- EU positioning: open-source + European data residency as combined differentiator

## SUNK vs Soperator

| Feature | CoreWeave SUNK | Nebius Soperator |
|---------|---------------|-----------------|
| Open source | ✗ (proprietary) | ✓ (GitHub) |
| Hardware | BlueField-3 DPU offload | Standard InfiniBand/RDMA |
| Storage | CAIOS (DPU-linked) | Shared root filesystem (Lustre/NFS) |
| MFU | ~96% | ~92% |
| Self-hostable | ✗ | ✓ |
| Gang scheduling | ✓ | ✓ |

## Strategic significance

Soperator's open-source release changed the competitive landscape fundamentally:

**Before**: SUNK-grade orchestration was CoreWeave's proprietary moat. No one else could match it without years of engineering.

**After**: Any provider with the right hardware can deploy Soperator and achieve ~92% MFU — closing most of the gap. The moat has shifted from *software* to *operational excellence at scale* and *hardware-software co-optimisation*.

This is the "barrier to code" argument in pres3: AI coding tools lower the cost of writing orchestration software, but the moat that remains is 96% goodput at 10,000 GPU scale — which requires operational knowledge, hardware partnerships, and years of failure data.

## Why it matters for ICN

Soperator is the recommended path for ICN's PoC hardware deployment:
- Open-source: no licensing cost
- Community-maintained: reduces ongoing engineering burden
- ~92% MFU: competitive with commodity neoclouds, superior to virtualised clouds
- Blackwell 6000 compatible (requires configuration)

The gap between Soperator (~92%) and SUNK (~96%) matters at scale but is acceptable for ICN's initial 8–64 GPU positioning.

## Key sources

- Nebius GitHub: github.com/nebius/soperator
- Nebius engineering blog: "Introducing Soperator"
- manual_notes/SUNK_&_Soperator_notes.md (this repo)
