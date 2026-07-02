---
term: MFU (Model FLOP Utilisation)
sphere: hardware
source: ML systems community / PaLM paper (Chowdhery et al., 2022) popularised the metric
tldr: The percentage of a GPU's theoretical peak FLOPS that are actually used for productive model computation — the key efficiency metric for training infrastructure.
---

## Definition

Model FLOP Utilisation (MFU) measures how efficiently a GPU cluster is being used for actual model computation, expressed as a percentage of the hardware's theoretical peak FLOPS.

```
MFU = (Observed throughput in FLOP/s) / (Peak theoretical FLOP/s of hardware)
```

A cluster achieving 50% MFU is using half its theoretical compute capacity for model work — the rest is lost to memory transfers, communication overhead, kernel launch latency, load imbalance, and framework overhead.

## Two metrics commonly conflated

**MFU** (this term) measures FLOPS efficiency — how much of the GPU's theoretical compute is being used for valid model math. **Goodput** measures *time* efficiency — what fraction of GPU-seconds are productive (not failed jobs, retries, waiting for stragglers). They are independent: a cluster can have 96% goodput and still only 50% MFU if its kernels are memory-bound or its tensor parallelism is poorly tuned.

The "96% / 80% / 70%" numbers commonly cited for SUNK / standard cloud / DePIN are **goodput**, not MFU. They came from CoreWeave's August 2025 training benchmarks whitepaper.

## Typical published numbers

| Setup | Goodput | MFU |
|-------|---------|-----|
| Bare-metal Slurm w/ topology-aware scheduling (CoreWeave) | ~96% | ~50–52% (Llama 70B, H100, FP8) |
| Nebius Soperator (open-source, MLPerf submission) | ~92% | ~45–50% (estimated) |
| Standard virtualised K8s | ~80% | ~35–45% |
| Consumer DePIN / distributed | unmeasured publicly | unmeasured publicly |

NVIDIA's own DGXC reference for Llama 3.1 70B FP8 on 64×H100 is 35.21% MFU. CoreWeave's claim of ~52% MFU is "20% above public benchmarks" of 35–45%.

## Why it matters commercially

A goodput gap of 16 percentage points (80% → 96%) translates to ~20% more productive GPU time per dollar of hardware, which at scale is real money. For a $1M/year training contract, a 16-pp goodput gap is ~$160,000/year in effective compute cost difference. The MFU gap is smaller in *percentage* terms (35–45% → 50–52%) but compounds with goodput because they multiply: total productive FLOPs = goodput × MFU × peak.

The MFU gap comes from:
- **Hypervisor overhead**: virtualisation steals ~5–15% of FLOPS
- **Network inefficiency**: all-reduce operations on slow fabrics (Ethernet vs InfiniBand) stall GPUs waiting for gradient sync
- **Memory bottleneck**: poor KV cache management, suboptimal kernel memory access patterns
- **Gang scheduling failures**: if one GPU in a job is slow, others wait — "straggler problem"

## Why it matters for ICN

Positioning ICN as "bare metal with MIG" claims the MFU advantage vs virtualised clouds. The PoC must be able to demonstrate MFU numbers to be credible with ICP-3 (foundation model labs) and ICP-4 (fine-tuning SMEs) customers. SUNK/Soperator deployment is the path to 90%+ MFU.

## Key sources

- Chowdhery et al., "PaLM: Scaling Language Modeling with Pathways" (2022) — first major public MFU benchmark
- CoreWeave SUNK whitepaper
- Nebius Soperator documentation
