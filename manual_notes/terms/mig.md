---
term: MIG (Multi-Instance GPU)
sphere: hardware
source: NVIDIA — introduced with A100 architecture (2020), expanded in H100, Blackwell
tldr: Hardware-level GPU partitioning that creates up to 7 fully isolated GPU instances on a single physical card, each with dedicated memory and compute.
---

## Definition

Multi-Instance GPU (MIG) is an NVIDIA hardware feature that partitions a single physical GPU into multiple isolated instances at the hardware level (not virtualisation). Each MIG instance has:

- Its own dedicated VRAM slice (e.g. on H100 80GB: up to 7 × 10GB instances)
- Its own dedicated compute engine slice
- Hardware-enforced isolation — one instance cannot access another's memory
- Independent CUDA context

MIG operates below the hypervisor, meaning there is no virtualisation overhead. Each instance appears to software as a complete, independent GPU.

## MIG profiles (H100 80GB example)

| Profile | Instances | VRAM each | Compute fraction |
|---------|-----------|-----------|-----------------|
| 1g.10gb | 7 | 10 GB | 1/7 |
| 2g.20gb | 3 | 20 GB | 2/7 |
| 3g.40gb | 2 | 40 GB | 3/7 |
| 7g.80gb | 1 | 80 GB | Full GPU |

## Why it matters

- **Utilisation**: a single H100 running one large inference job may use only 40% of compute capacity. MIG allows that card to serve 3–7 independent customers simultaneously, dramatically improving utilisation economics.
- **Per-customer economics**: for inference-focused providers (like ICN's PoC), MIG allows finer-grained billing and better margin per GPU.
- **Isolation without overhead**: unlike vGPU (software virtualisation), MIG isolation is hardware-enforced with no performance penalty.
- **Blackwell MIG**: Blackwell 6000 (ICN's PoC hardware) supports MIG. The specific profile options depend on VRAM capacity.

## Limitations

- MIG instances cannot be used for multi-GPU training jobs (each instance sees only its own compute — no NVLink between instances on the same card)
- Profile changes require brief downtime (reconfiguration)
- Not all workloads fit MIG profiles — very large models (70B+) need full-GPU or multi-GPU instances

## Why it matters for ICN

ICN's PoC specifically uses MIG on Blackwell 6000 cards. This is the right architectural choice for inference-first positioning: multiple parallel inference workloads served efficiently on the same hardware. The S10 slide in pres3 highlights MIG as a direct ICN competitive advantage.

## Key sources

- NVIDIA MIG User Guide (docs.nvidia.com)
- NVIDIA GTC 2026 — Blackwell architecture sessions
