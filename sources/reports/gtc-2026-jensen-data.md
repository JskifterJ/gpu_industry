---
title: NVIDIA GTC 2026 Keynote — Key Data Points
author: Jensen Huang, NVIDIA (research compiled by Claude)
date: 2026-03-17
source_type: report
url: https://www.tomshardware.com/news/live/nvidia-gtc-2026-keynote-live-blog-jensen-huang
sphere: hardware
tags: [gtc-2026, jensen-huang, blackwell, vera-rubin, inference, tokens-per-watt, energy, performance]
relevance: high
deck_slide: S5 (GPU Generation Lifecycle), S6 (Energy Wall), S7 (Silicon Diversification), S8 (Memory & Bandwidth)
---

# NVIDIA GTC 2026 — Key Data for Pres3

*Keynote: Jensen Huang, March 17 2026, San Jose. Research compiled April 13 2026.*

## Central thesis
Token = the new commodity. Every company is becoming a "token factory." Inference has overtaken training as the dominant compute workload. KPI: **tokens/watt**.

**Key direct quotes:**
- "Tokens are the new commodity. And like all commodities, once it reaches an inflection, it will segment into different parts."
- "I see through 2027, at least $1 trillion [in computing demand]. Computing demand will be much higher than that."
- "$1 trillion in combined Blackwell and Vera Rubin purchase orders are in place through 2027."

---

## GPU Generation Performance Table

| Spec | H100 SXM5 | B200 (single) | GB200 NVL72 (rack) | GB300 NVL72 | Vera Rubin NVL144 |
|------|-----------|---------------|--------------------|-------------|-------------------|
| FP8 TFLOPS (dense) | 3,958 | 4,500 / 9,000 sparse | — | — | — |
| FP4 TFLOPS | Not supported | 9,000 / 20,000 sparse | ~1 EFLOPS | 1.1 EFLOPS | 3.6 EFLOPS |
| HBM | 80 GB HBM3 | 192 GB HBM3e | 13.5 TB total | — | ~20.7 TB |
| Memory bandwidth | 3.35 TB/s | 8 TB/s | 130 TB/s NVLink | — | 20.5 TB/s/GPU (HBM4) |
| NVLink bandwidth | 900 GB/s (v4) | 1.8 TB/s (v5) | 130 TB/s switch | — | — |
| TDP (single GPU) | 700W | 700W (SXM) / 1,200W (GB200) | — | — | — |
| Rack power | — | — | ~120–132 kW | ~120–132 kW | ~600 kW (projected) |

---

## "X-Times Improvement" Claims Per Generation

| Metric | Blackwell vs H100 | Vera Rubin vs Blackwell |
|--------|-------------------|------------------------|
| LLM training throughput | **4× faster** | ~3.3× |
| LLM inference throughput | **30× faster** (NVL72 vs H100 cluster) | **5× faster** |
| Energy efficiency (inference) | **25×** lower energy | **50× tokens/watt** vs Hopper *(SemiAnalysis verified; Jensen conceded 50× from initial 30× claim)* |
| FP4 rack-level compute | 1.0 EFLOPS (GB200 NVL72) | 3.6 EFLOPS (Rubin NVL144) |
| Cost per token | — | **10× lower** vs Blackwell |

---

## Compute Demand Growth Claims

| Transition | Multiplier |
|-----------|-----------|
| Generative AI → Reasoning models | ~100× more compute |
| Reasoning models → Agentic systems | ~another 100× |
| Combined (verifiable) | **~10,000×** in 2 years |
| As claimed (stacked with usage) | "1,000,000×" — conflates hardware efficiency + demand growth |

**Note for presentation:** Use 10,000× (the auditable hardware-tier claim). The 1,000,000× figure stacks supply-side efficiency with usage growth — cite as Jensen's claim with caveat.

---

## Token Tier Pricing (Huang's segmentation model)

| Tier | Price / M tokens |
|------|----------------|
| Free / commodity | ~$1 |
| Mid-range | $3–$6 |
| Professional/engineering | ~$45 |
| Real-time interactive | ~$150 |

---

## Hardware announced

- **Kyber rack:** NVL144 — 144 GPUs on one NVLink domain, NVL1152 at full scale
- **Vera Rubin NVL144 CPX (2027):** 8 EFLOPS FP4 (11× vs GB200 NVL72)
- **Vera CPU + Groq 3 LPU (LP40):** 35× higher inference throughput for 1T param models at same power, 500MB on-chip SRAM
- **Spectrum X CPO:** 2Tb/s single-port bandwidth, 5× optical power efficiency
- **Neotron 3 Super:** 10× edge SLM performance
- **OpenClaw / NemoClaw:** Agentic OS layer for resource management, scheduling, IO — enterprise-ready with policy engines

---

## On "280× inference price decline" (note)
This figure does NOT appear in GTC 2026 coverage. It likely refers to the GPT-3.5 class: $20/M tokens (Oct 2022) → $0.07/M tokens (Oct 2024) per Epoch AI / Artificial Analysis. Do not attribute to GTC 2026 — source separately.

---

## Sources
- Tom's Hardware GTC 2026 Live Blog: https://www.tomshardware.com/news/live/nvidia-gtc-2026-keynote-live-blog-jensen-huang
- CNBC: https://www.cnbc.com/2026/03/16/nvidia-gtc-2026-ceo-jensen-huang-keynote-blackwell-vera-rubin.html
- Global Data Center Hub (19 takeaways): https://www.globaldatacenterhub.com/p/19-key-takeaways-from-jensen-huangs-f2c
- NVIDIA GB200 NVL72 product page: https://www.nvidia.com/en-us/data-center/gb200-nvl72/
- SemiAnalysis 50× verified: https://aiafterhours.substack.com/p/jensen-huang-just-surrendered-on
- Tech-Insider Rubin analysis: https://tech-insider.org/nvidia-gtc-2026-rubin-gpu-analysis/
