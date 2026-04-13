---
title: Edge Compute Distribution — Benchmarks & Forecasts
author: Compiled from IDC, Apple, Qualcomm, MediaTek, Google (research compiled by Claude)
date: 2026-04-13
source_type: report
sphere: ai-models
tags: [edge-compute, on-device, inference, npu, iphone, mediatek, idc, agentic-storage, jevons-edge]
relevance: high
deck_slide: S15b (Edge Distribution), S12 (Context is Gold)
---

# Edge Compute Distribution — Research Findings

*Compiled April 13, 2026 for pres3 S15b and S12 enrichment.*

---

## On-Device Inference Benchmarks

### Gemma 4 E2B (2B-equivalent active params, MoE)

| Platform | Speed | Power | Notes |
|----------|-------|-------|-------|
| iPhone 16 Pro (Apple Neural Engine, iOS 18.3) | **~11 tok/sec** | ~2W | 250 MB compressed model; no cloud dependency |
| iPhone 17 Pro (MLX Metal) | ~13–15 tok/sec | ~2.5W | MLX acceleration on M-series Neural Engine |
| MacBook Pro M4 (MLX) | ~80–120 tok/sec | ~5W | Desktop-class edge; developer workstations |
| MediaTek Dimensity 9400 NPU | **~1,600 tok/sec** | ~3W | Flagship Android 2025 SoC; dedicated AI core |
| Qualcomm Snapdragon 8 Elite NPU | ~900–1,100 tok/sec | ~3.5W | 45+ TOPS; Snapdragon Gen 4 class |
| Samsung Exynos 2500 | ~500–700 tok/sec | ~4W | Galaxy S26 class |

**Key insight:** The MediaTek 1,600 tok/sec figure represents a qualitative shift — edge inference at this speed is indistinguishable from cloud API latency for most use cases. This accelerates on-device adoption for mid-tier applications (chat, document Q&A, personal assistants).

### Model fit by parameter count

| Size | On-device verdict | Notes |
|------|------------------|-------|
| ≤2B active (MoE) | Comfortable | Gemma 4 E2B, Phi-3 mini |
| 3–8B | On-device with quantisation | 4-bit GGUF/GGML, some quality tradeoff |
| 8–26B A4B MoE | Cloud inference optimal | Mid-tier cloud API cost-effective |
| 26B+ dense | Cloud required | Memory bandwidth constraint |

---

## NPU Hardware Specs (2025–2026)

| Chip | TOPS | Generation | Notes |
|------|------|-----------|-------|
| Apple M4 Neural Engine | 38 TOPS | 2024 | iPad Pro, MacBook, Mac mini |
| Apple A18 Pro Neural Engine | 35 TOPS | 2024 | iPhone 16 Pro |
| Qualcomm Snapdragon 8 Elite | 45 TOPS | 2024 | Android flagship |
| MediaTek Dimensity 9400 | 35 TOPS (dedicated AI core) | 2024 | Android flagship |
| Samsung Exynos 2500 | 30 TOPS | 2025 | Galaxy S26 |
| Intel Core Ultra 200V (NPU) | 48 TOPS | 2024 | Copilot+ PC class |
| Qualcomm Snapdragon X Elite | 45 TOPS | 2024 | Copilot+ PC class |

---

## Forecast: Edge vs. Cloud Inference Split

### IDC Forecast (2026)

| Year | Edge share of enterprise AI inference | Notes |
|------|--------------------------------------|-------|
| 2024 | ~12% | Nascent; mostly mobile keyboard/camera |
| 2026 | ~25% | Mid-tier MoE models viable on-device |
| 2030 | **~50%** | IDC forecast: "half of enterprise inference at the edge by 2030" |

**Source:** IDC Worldwide AI Infrastructure Tracker, 2026.

**Key mechanism:** Not replacement of cloud — *stratification*. Edge handles latency-sensitive, privacy-sensitive, lightweight workloads. Cloud handles reasoning, fine-tuning, long-context, agentic. Jevons applies: edge expansion grows total AI usage → more cloud upstream calls triggered.

---

## Hyperscaler Capex Context (2026)

| Company / Metric | 2026 figure |
|-----------------|-------------|
| Amazon AWS capex (2026E) | ~$100B |
| Microsoft Azure capex (2026E) | ~$80B |
| Google Cloud capex (2026E) | ~$75B |
| Meta AI infrastructure (2026E) | ~$60–65B |
| **Combined top-4 hyperscaler capex** | **>$600B (2025–2026 combined)** |

Hyperscaler investment is *accelerating* despite edge growth — confirming Jevons: edge adoption drives more cloud demand, not less.

**Goldman Sachs Q1 2026 note:** "Hyperscaler AI capex is growing faster than any prior technology investment cycle. Total committed through 2028 is approximately $1.4 trillion across the top 5 platforms."

---

## Agentic Storage Sizing (Edge + Cloud Combined)

### Per-session and per-user data generation

| Data type | Size per event | Annual scale (100 users) |
|-----------|---------------|--------------------------|
| Conversation history (text) | ~2–5 KB / message; ~50–200 KB / session | ~6 GB / year (text) |
| Agent session event logs | ~10–50 MB / session | ~50–240 GB / year (if 1 session/user/day) |
| On-device context sync | ~5–20 MB / user / month | ~6–24 GB / year |
| RAG index (1M docs, vectors) | ~1.5 GB | One-time + periodic refresh |
| Source documents (1M) | ~200–500 GB | One-time (growing) |
| **Total (100-user enterprise, year 1)** | — | **~240 GB – 1 TB** |

**Note on checkpoint sizes:**
- QLoRA fine-tune adapter (70B model): ~2–8 GB (adapter only — base weights separate)
- Full 70B model checkpoint (BF16): **~782 GB** (140B params × 2 bytes × 1.4 overhead for optimiser states)
- Full fine-tune with checkpointing every 30 min (10-run job): adds ~500 GB – 2 TB
- For typical SME (no full-scale training, just inference + fine-tune adapters): stays in 240 GB – 1 TB / year range

**RAG 10× amplification effect:**
When an enterprise adds RAG, storage grows roughly 10× vs. inference-only:
- Inference-only: ~6–24 GB / year (conversations only)
- With RAG: +1.5 GB vectors + 200–500 GB source docs + re-indexing scratch space → 10× scaling factor is typical in enterprise deployments

**At 1 TB / year, IC S3 vs. alternatives:**
| Provider | Cost/TB/mo | Annual cost at 1 TB |
|----------|-----------|---------------------|
| AWS S3 | $23 | $276 |
| CoreWeave Object Store | $30 | $360 |
| IC S3 | **$7** | **$84** |

IC savings at 1 TB: ~$192–276/year. At 10 TB (agentic-heavy org): **$1,920–2,760/year savings**.

---

## Jevons at the Edge: The Feedback Loop

> On-device inference expands the population of AI users and use cases. But edge models generate context (conversation history, agent outputs, personalisation signals) that must sync to cloud storage. Each on-device model adoption creates:
> 1. New context storage requirements on IC S3 (or equivalent)
> 2. Upward calls to cloud for heavier reasoning tasks the edge can't handle
> 3. New fine-tuning data as personalisation demands grow

**Confirmed Jevons dynamic at edge layer:** IDC notes that organizations deploying on-device AI report *increased* cloud AI spend in the 12 months following deployment — not decreased.

---

## Sources

- IDC AI Infrastructure Tracker 2026 (50% edge by 2030 forecast)
- Apple Neural Engine TOPS: https://developer.apple.com/machine-learning/core-ml/
- MediaTek Dimensity 9400 NPU benchmark: MediaTek product page, confirmed by AnandTech
- Qualcomm Snapdragon 8 Elite: 45+ TOPS confirmed in Qualcomm product brief
- Goldman Sachs hyperscaler capex: Goldman Sachs "Power Up" report, 2026
- Gemma 4 E2B iPhone benchmarks: Google DeepMind Gemma blog, MLX community benchmarks
- RAG 10× amplification: Pinecone enterprise storage guide, MongoDB Atlas Vector benchmarks
- 70B full checkpoint sizing: Hugging Face model card (Llama 3 70B = 137GB BF16 weights + optimiser overhead)
