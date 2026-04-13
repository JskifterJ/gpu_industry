---
title: 12 Curated Industry Snippets — Venn Slide (pres3 S-new)
author: Various
date: 2026-04-13
source_type: curated-mix
sphere: cross-sphere
tags: [venn-slide, hardware, software, ai-models, inference, energy, orchestration, open-weight, agentic]
relevance: high
deck_slide: S-venn (new slide between Jevons and Three Spheres)
---

# Curated Snippets for Venn Animation Slide

12 snippets for the two-phase Venn slide: scattered card wall → animate into three-sphere Venn triangle.

## Venn placement summary

| # | Attribution | Sphere | Venn Overlap |
|---|---|---|---|
| H1 | Jensen Huang, GTC 2026 | Hardware | Hardware ↔ Software |
| H2 | Epoch AI, Jan 2026 | Hardware | Pure Hardware |
| H3 | Epoch AI, Aug 2024 | Hardware | Pure Hardware |
| H4 | SemiAnalysis (ICN synthesis) | Hardware | Pure Hardware |
| S1 | Hugo Shi, Saturn Cloud, Jan 2025 | Software | Hardware ↔ Software |
| S2 | TrendMicro, Mar 2026 | Software | Pure Software |
| S3 | Hugo Shi, Saturn Cloud, Jan 2025 | Software | Hardware ↔ Software |
| S4 | SemiAnalysis/BentoML (ICN synthesis) | Software | Software ↔ AI/Models |
| M1 | Google Gemma 4 / ICN, Apr 2026 | AI/Models | Hardware ↔ AI/Models |
| M2 | Z.ai GLM-5.1, Apr 2026 | AI/Models | Software ↔ AI/Models |
| M3 | Jevons Paradox (applied) | AI/Models | CENTER — all three spheres |
| M4 | Lambert + Raschka / Lex Fridman, 2025 | AI/Models | Software ↔ AI/Models |

---

## HARDWARE SPHERE

### H1 — tokens/watt
> "Tokens per watt is the new performance metric for AI infrastructure. It is the compound measure of compute, memory bandwidth, storage, and networking working together — not any one chip in isolation."

**Attribution:** Jensen Huang, CEO, NVIDIA  
**Source:** GTC 2026 Keynote  
**Date:** March 2026  
**Venn:** Hardware ↔ Software overlap

---

### H2 — compute capacity doubling
> "Global AI computing capacity has grown by approximately 3.3× per year since 2022 — equivalent to a doubling time of just 7 months. NVIDIA AI chips currently account for over 60% of total compute capacity."

**Attribution:** Epoch AI Research Team (Josh You et al.)  
**Source:** Epoch AI, "Global AI computing capacity is doubling every 7 months"  
**Date:** January 2026  
**Venn:** Pure Hardware

---

### H3 — energy as the binding constraint
> "Data center campuses of 1 to 5 gigawatts are likely possible by 2030. Power is the constraint likely to bind first — followed by the capacity to manufacture enough chips."

**Attribution:** Jaime Sevilla, Tamay Besiroglu et al., Epoch AI  
**Source:** Epoch AI, "Can AI scaling continue through 2030?"  
**Date:** August 2024  
**Venn:** Pure Hardware

---

### H4 — infrastructure capex reality
> "An AI-ready data center costs $10–12M per megawatt to construct — nearly double a conventional facility. Frontier training clusters draw 40–100 MW. Access to gigawatt-scale power has superseded raw silicon as the primary bottleneck."

**Attribution:** SemiAnalysis (via ICN research synthesis)  
**Source:** SemiAnalysis datacenter industry model, Q1 2026  
**Date:** Q1 2026  
**Venn:** Pure Hardware

---

## SOFTWARE / STACK SPHERE

### S1 — bare-metal migration
> "For frontier model pre-training, the hypervisor virtualization tax is no longer acceptable to top-tier AI labs. Enterprises are systematically migrating massive training workloads to bare-metal, AI-native clouds that possess advanced orchestration."

**Attribution:** Hugo Shi, Co-Founder & CTO, Saturn Cloud  
**Source:** AI Engineering Podcast, "GPU Clouds, aggregators, and the new economics of AI compute"  
**Date:** January 2025  
**Venn:** Hardware ↔ Software overlap

---

### S2 — supply chain attack
> "On March 24, 2026, LiteLLM suffered a critical supply chain attack. The malware used a hidden file that executed silently the moment Python started. It scoured systems for SSH keys, cloud credentials, and AI API keys, then attempted to deploy privileged pods across entire Kubernetes clusters."

**Attribution:** TrendMicro Research  
**Source:** TrendMicro, "Inside the LiteLLM Supply Chain Compromise"  
**Date:** March/April 2026  
**Venn:** Pure Software

---

### S3 — egress as lock-in
> "Cloud-native setups tied to proprietary hyperscaler APIs are incredibly difficult to migrate. Kubernetes-native workloads using Helm charts are highly portable. The hyperscaler egress fee model is the real lock-in mechanism — not the hardware."

**Attribution:** Hugo Shi, Co-Founder & CTO, Saturn Cloud  
**Source:** AI Engineering Podcast (same episode as S1)  
**Date:** January 2025  
**Venn:** Hardware ↔ Software overlap

---

### S4 — inference efficiency as revenue battleground
> "The primary revenue battleground is shifting from raw hardware aggregation to inference efficiency. Providers that have successfully architected proprietary L5 model serving platforms will capture the durable, sticky operational expenditures of the global enterprise market."

**Attribution:** SemiAnalysis / BentoML analysis (ICN synthesis)  
**Source:** ICN research, manual_notes/trends.md  
**Date:** Q1 2026  
**Venn:** Software ↔ AI/Models overlap

---

## AI / MODELS SPHERE

### M1 — MoE economics
> "The 26B A4B MoE [Gemma 4] runs at frontier-adjacent quality at the economics of a 4B model. A GPU serving Gemma 4 26B A4B effectively serves at the cost of a 4B model — a massive throughput-per-dollar compression that will reprice inference industry-wide."

**Attribution:** Google / ICN research synthesis  
**Source:** Google Gemma 4 launch blog + ICN manual_notes/trends.md  
**Date:** April 2026  
**Venn:** Hardware ↔ AI/Models overlap

---

### M2 — agentic as new workload class
> "GLM-5.1 sustains 8-hour autonomous coding execution loops — planning, execution, testing, optimisation in a single session. That is qualitatively different from 2-minute completions. It requires fundamentally different infrastructure: job scheduling, checkpointing, long-context KV cache management."

**Attribution:** Z.ai (Zhipu AI) — GLM-5.1 launch documentation  
**Source:** z.ai/blog/glm-5.1 + ICN manual_notes/trends.md  
**Date:** April 2026  
**Venn:** Software ↔ AI/Models overlap

---

### M3 — Jevons (center anchor)
> "As technology increases the efficiency with which a resource is used, the total consumption of that resource increases rather than decreases. Every optimisation across the stack — hardware and software alike — will inevitably lead to more compute consumption, not less."

**Attribution:** William Stanley Jevons (1865), applied to AI compute  
**Source:** ICN manual_notes/trends.md  
**Date:** Documented Q1 2026 research; referenced GTC 2026  
**Venn:** CENTER — all three spheres. Use as fixed anchor callout in Venn center.

---

### M4 — fine-tuning as enterprise moat
> "Knowledge-intensive firms are beginning to hire AI researchers from frontier labs to fine-tune open-weight models on proprietary data. The open-weight model is the baseline; the proprietary dataset and domain-specific fine-tune are the moat."

**Attribution:** Nathan Lambert & Sebastian Raschka (ML researchers)  
**Source:** Lex Fridman Podcast (episode with Lambert + Raschka), cited in ICN notes  
**Date:** 2025  
**Venn:** Software ↔ AI/Models overlap
