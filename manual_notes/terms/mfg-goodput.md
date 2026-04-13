---
term: Goodput
sphere: hardware
source: Networking/HPC community — adapted for GPU cluster efficiency metrics
tldr: The fraction of scheduled GPU time that produces valid, useful model computation — distinct from raw utilisation, which counts idle-but-scheduled time as "used."
---

## Definition

Goodput (good throughput) is the portion of total compute time that results in productive output. In GPU clusters:

```
Goodput = (Time GPUs are doing useful model computation) / (Total scheduled time)
```

It differs from utilisation:
- **Utilisation**: GPU is active (drawing power, executing kernels) — includes wasted computation
- **Goodput**: GPU is producing valid tokens, gradients, or activations — excludes failures, retries, stragglers, network waits

## Why goodput matters more than utilisation

A GPU can show 99% utilisation while producing 60% goodput if:
- 20% of time is spent on failed jobs that must restart
- 15% is spent waiting for gradient sync (network bottleneck)
- 4% is wasted on false-positive hardware error recovery

CoreWeave benchmarks SUNK at ~96% goodput — this means for every 100 hours of scheduled GPU time, 96 hours produce valid model computation. At $3/GPU-hour, 4% goodput loss = $0.12/GPU-hour wasted — significant at scale.

## Key goodput benchmarks

| Provider / Setup | Goodput |
|-----------------|---------|
| Consumer DePIN | ~55–70% |
| Standard VM cloud | ~75–82% |
| Bare metal (industry avg) | ~88–90% |
| Nebius Soperator | ~92% |
| CoreWeave SUNK | ~95–96% |

## Key sources

- CoreWeave SUNK whitepaper
- Nebius Soperator benchmarks
