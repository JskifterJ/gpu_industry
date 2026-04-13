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

## Typical MFU benchmarks

| Setup | MFU |
|-------|-----|
| Consumer DePIN / distributed | ~55–70% |
| Standard virtualised cloud | ~75–82% |
| Industry average bare metal | ~85–90% |
| Nebius Soperator | ~92% |
| CoreWeave SUNK | ~95–96% |

## Why it matters commercially

A 20% MFU gap (80% vs 96%) translates to ~20% more throughput per dollar of hardware — which at $30,000/month per H100 cluster means real money. For a $1M/year training contract, a 16% MFU gap is ~$160,000/year in effective compute cost difference.

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
