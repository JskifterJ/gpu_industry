---
term: Jevons Paradox
sphere: cross-sphere
source: William Stanley Jevons, "The Coal Question", 1865
tldr: When a resource becomes more efficient to use, total consumption of that resource increases — not decreases.
---

## Definition

Jevons Paradox is the observation that improvements in the efficiency of resource use tend to increase, rather than decrease, the total consumption of that resource. The paradox was first articulated by economist William Stanley Jevons in 1865, observing that more efficient steam engines led to *more* coal consumption, not less — because cheaper coal energy enabled previously uneconomical uses.

The mechanism: efficiency lowers the effective cost per unit of output → demand for the output expands faster than the efficiency gain → absolute resource consumption rises.

## Historical arc

| Era | Efficiency gain | Paradox outcome |
|-----|----------------|----------------|
| Coal, 1865 | Steam engines 3× more efficient | Coal consumption doubled |
| Oil, 1970s | Fuel injection 30% more efficient | Car ownership tripled |
| Internet, 2000s | Bandwidth 100× cheaper | Data consumption 10,000× |
| AI compute, 2020s | Inference cost ↓ 280× in 2 years | GPU demand ↑ 35% CAGR |

## Application to AI compute

Inference prices fell approximately 280× between 2022 and 2024 (GPT-3.5 class: $20/M tokens → $0.07/M tokens, per Epoch AI). Rather than reducing GPU demand, this collapse in cost per token triggered a wave of new AI applications, agentic workloads, and higher per-user consumption — driving GPU demand growth of ~35% CAGR.

This is the foundational meta-law behind every trend in the pres3 deck:
- Cheaper hardware FLOPs → more training runs → more GPU demand
- Better inference software (FlashAttention, speculative decoding) → lower cost/token → more API calls
- Smaller, more efficient open-weight models → accessible to more developers → more inference endpoints

## Why it matters for ICN

Jevons is the reason the GPU compute market does not "solve itself" as models become more efficient. Every efficiency gain expands the addressable market. ICN's positioning thesis depends on this: as inference costs fall and agentic workloads proliferate, the *volume* of compute and storage required grows — not shrinks.

## Key sources

- Jevons, W.S. *The Coal Question*, 1865
- Epoch AI: "Inference prices for AI models, 2021–2025" (llm-perf-history.md / Artificial Analysis)
- McKinsey Global Institute: AI Infrastructure Demand Model, 2025
