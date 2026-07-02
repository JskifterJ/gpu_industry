---
term: SUNK
sphere: software
source: CoreWeave engineering / industry terminology
tldr: Slurm-on-Kubernetes — CoreWeave's proprietary orchestration stack combining gang scheduling with Kubernetes, delivering ~96% goodput on bare-metal GPU clusters.
---

## Definition

SUNK (Slurm-on-Kubernetes) is CoreWeave's orchestration architecture that combines:
- **Slurm**: the traditional HPC job scheduler, optimised for gang scheduling (all GPUs in a job start simultaneously or not at all)
- **Kubernetes**: container orchestration for resource management, networking, and workload isolation

The key innovation is running Slurm as a first-class scheduler within Kubernetes, rather than treating them as alternatives. This allows GPU clusters to serve both large training jobs (gang-scheduled, MPI-based) and interactive inference workloads from the same hardware pool, without the hypervisor tax of virtualisation.

## Why it matters

- **Goodput** (not MFU — see `mfu.md` for the distinction): CoreWeave's August 2025 training benchmarks whitepaper claims ~96% goodput on SUNK vs ~80% on standard virtualised cloud. The "DePIN ~70%" figure circulating in industry decks is marketplace utilisation (fill rate), not goodput, and shouldn't be cited as a like-for-like comparison. The 16-pp goodput gap (80% → 96%) translates to ~20% higher productive GPU-time per dollar, which is commercially decisive for large training runs.
- **MFU is a separate metric** — CoreWeave's published MFU on H100 Llama 70B FP8 is ~50–52% vs the NVIDIA DGXC reference of ~35%, a ~20% relative MFU advantage that compounds with the goodput gap (total productive FLOPs = goodput × MFU × peak).
- **BlueField-3 DPU offload**: SUNK routes all network and storage I/O through NVIDIA BlueField-3 DPUs, keeping compute GPUs focused 100% on model work (no CPU interrupts for storage/networking)
- **Topology-aware scheduling**: jobs are placed on GPUs that are physically closest on the NVLink/InfiniBand fabric, minimising all-reduce latency
- **Mission Control**: CoreWeave's predictive failure detection — identifies GPU hardware degradation before it causes job failure, rescheduling proactively

## SUNK vs commodity Kubernetes

| Feature | CoreWeave SUNK | Standard K8s |
|---------|---------------|--------------|
| Gang scheduling | ✓ (Slurm-native) | ✗ (requires plugins) |
| Hypervisor tax | None (bare metal) | ~5–20% |
| Storage offload | BlueField-3 DPU | CPU-handled |
| Predictive failure | ✓ Mission Control | ✗ |
| Goodput | ~96% | ~80% |
| MFU (Llama 70B H100 FP8) | ~50–52% | ~35–45% |

## Soperator relationship

Nebius released **Soperator** — an open-source Kubernetes operator that deploys a SUNK-equivalent architecture. This changes the competitive landscape: the software recipe for SUNK-grade orchestration is now publicly available. The moat has shifted from "can you build it" to "can you run it at scale with the right hardware."

## Why it matters for ICN

ICN's PoC hardware needs Soperator (or equivalent) deployment to compete above bare-metal. Soperator is now open-source, removing the licensing barrier. The execution challenge — deploying and tuning it for Blackwell 6000 hardware — is addressable.

## Key sources

- CoreWeave engineering blog: "SUNK: Slurm on Kubernetes"
- NVIDIA GTC sessions on CoreWeave infrastructure
- manual_notes/SUNK_&_Soperator_notes.md (this repo)
