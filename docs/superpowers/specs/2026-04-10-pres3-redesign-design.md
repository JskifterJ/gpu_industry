# Pres 3 Redesign — Design Spec
*2026-04-10 · Author: J. Skifter / Claude · Status: Awaiting user sign-off*

---

## Purpose

Replace `pres3_gpu-trends-positioning-deck.html` with a rebuilt deck that:
- Reasons through the GPU compute market systematically using the **three-sphere framework** (Hardware / Software / AI/Models)
- Uses **Jevons Paradox** as the unifying backbone/meta-law — opened as a visually captivating historical story beat
- Includes **data-driven charts** on every substantive slide
- Uses **divider slides** (like pres2) to create a clear red thread for live presentation
- Adds **SUNK vs Soperator** comparison (Software sphere)
- Concludes with a **2×2 position map + BM response matrix** before the ICN play
- ICN's Play structured as **two avenues** (clean-slate vs IC-integrated) with a **full comparison table** (10 criteria incl. AI moat with definitions)
- ICNT/fiat decision kept out of main deck → moved to **Appendix S24**
- Serves as the ~Apr 16–17 Seb presentation AND as the pre–all-hands analytical deck (distribute as PDF ~May 1)

Presentation date: **late next week (~Apr 16–17)**. All-hands document: **~May 1**.

---

## Architecture

**File:** `pres3_gpu-trends-positioning-deck.html` (overwrite in place)

**Type:** Single-file self-contained HTML (no external assets beyond Google Fonts)

**Design system:** Inherit from `(pres3)inference-compute-deck.html`:
- Fonts: Bebas Neue (display) · Barlow (body) · JetBrains Mono (mono/labels)
- Dark default + light toggle
- Chrome bar top + nav bar bottom + progress bar + slide counter
- Slide transitions: translateX with cubic-bezier(0.76,0,0.24,1)
- fadeUp stagger animations on `.anim-1` through `.anim-6`

**Sphere color mapping** (consistent throughout):
| Sphere | CSS var | Dark | Light |
|--------|---------|------|-------|
| Hardware | `--hw` | `#c084fc` (purple) | `#7c3aed` |
| Software | `--sw` | `#2dd4bf` (teal) | `#0d9488` |
| AI/Models | `--llm` | `#fbbf24` (amber) | `#b45309` |
| ICN Play | `--icn` | `#5AF688` (neon green, ICN brand) | `#059669` |

**Data files loaded at runtime (fetch):**
- `data_files/gpu_AI_workloads_2025-2030.json`
- `data_files/gpu_economics_canonical_spec_v1.json`
- `data_files/market_positioning.json`

**Charts:** All charts are inline SVG or Canvas — no `<img>` tags (ensures dark/light theming + self-contained). Epoch AI source charts are recreated as styled SVG plots.

---

## Slide-by-Slide Specification

### S1 — Cover
**Chrome:** ClusterMAX™ · GPU Compute: Trends & Positioning
**Act badge:** none (cover)
**Content:**
- Eyebrow: `ClusterMAX Intelligence Series · Deck 3 of 3`
- H1: `GPU COMPUTE` / `TRENDS &` / `POSITIONING`
- Subtitle: "A structured analysis of the forces reshaping the GPU stack — and where ICN fits."
- Pill row: `Hardware` (hw) · `Software` (sw) · `AI/Models` (llm)
- Meta: Date `April 2026` · Author `J. Skifter, ICN` · Series `Pres 3 / 3`
- Background: grid overlay + 3 sphere glows (purple top-right, teal bottom-left, amber center-right)

---

### S2 — Recap: What We Covered
**Chrome:** Previously · Pres 1 + 2
**Act badge:** `RECAP`
**Content:** Two-column recap (mirror pres2's "deck 3 preview" style):
- **Pres 1 — GPU Foundations** (left column, bullet list):
  - GPU vs CPU architecture
  - How LLMs work (token prediction, KV cache)
  - Model size → VRAM requirements
  - Inference economics: cost/token calculator
  - GPU generations: V100 → A100 → H100 → B200
- **Pres 2 — Competitive Landscape** (right column, bullet list):
  - 9-layer stack model (L-1 to L7)
  - 4 business model archetypes (Hyperscaler / Neocloud / Orchestrator / Aggregator)
  - ClusterMAX scoring (50pt, 10 criteria)
  - Deep dives: CoreWeave, Nebius, Lambda, Together AI
  - ICP archetypes and provider fit matrix
  - DePIN / Web3 GPU networks
- Bottom callout: "Pres 3 builds on this — we reason through where the market is heading."

---

### [DIVIDER] — The Jevons Lens
**Style:** Full-bleed divider. Large sphere number `00`. Sphere accent: neutral (white/dim).
**Text:** `THE` / `JEVONS` / `LENS` (Bebas Neue, 96px)
**Sub:** "The meta-law that connects every trend in this deck."

---

### S3 — Jevons Paradox: The Meta-Law
**Chrome:** The Jevons Lens · Foundational Frame
**Act badge:** `JEVONS`
**Design intent:** This is a story slide, not a data slide. Atmosphere first. The visual should arrest attention before the words do.

**Content:**
- **Atmospheric background:** Full-slide SVG — a dark, almost cinematic scene. Faint crosshatch texture suggesting coal mine timbering. A single light source (top-center). Mood: 1865 Britain.
- **Large centered quote** (Bebas Neue, 56px, white, max-width 70%):
  > "As technology increases the efficiency with which a resource is used,
  > the total consumption of that resource **increases** — not decreases."
  > — William Stanley Jevons, *The Coal Question*, 1865
- **Below the quote — horizontal historical arc (SVG timeline, full width):**
  Each node has an icon silhouette + label + short annotation in small type:

  | Era | Efficiency gain | Paradox outcome |
  |-----|----------------|----------------|
  | Coal 1865 | Steam engines 3× more efficient | Coal consumption doubled |
  | Oil 1970s | Fuel injection 30% more efficient | Car ownership tripled |
  | Internet 2000s | Bandwidth 100× cheaper | Data consumption 10,000× |
  | AI Compute today | Inference cost ↓ 280× (2yr) | GPU demand ↑ 35% CAGR |

  - Timeline arrows connect each era → next. Final node (AI Compute) is highlighted in `--icn` green.
  - Each node uses a minimal icon: coal cart · petrol pump · server rack · GPU chip

- **Bottom anchor stat** (large, icn green): "Inference prices fell **280× in 2 years.** GPU demand grew **35% per year.**"
- **Bottom-right footnote:** Source: Epoch AI / McKinsey

---

### S4 — Jevons in Action: Three Spheres, One Outcome
**Chrome:** The Jevons Lens · How It Propagates
**Act badge:** `JEVONS`
**Content:** Flow diagram (SVG) — this slide is the analytical bridge from history to the three spheres:
```
[HARDWARE]  cheaper FLOPs/$ ──┐
                               ├──▶ Lower cost/token ──▶ More consumption ──▶ More GPU demand
[SOFTWARE]  better inference ──┤
                               │
[AI/MODELS] smaller models ───┘
```
- Each sphere box uses its color (`--hw`, `--sw`, `--llm`). Arrow flow in white/dim.
- **Real evidence cards below each sphere** (3 cards, one per sphere, sphere-colored border):
  - Hardware: "B200 = 4,500 TFLOPS vs H100 = 989 TFLOPS. 4.5× jump. Inference throughput explodes → more jobs per dollar → developers build more AI products."
  - Software: "FlashAttention: 2–5× throughput vs stock vLLM. Speculative decoding: 2–3×. Cost/token halved — again. More agentic calls triggered."
  - AI/Models: "Gemma 4 26B A4B: 4B active params at inference. Frontier-adjacent quality on a laptop GPU. 1B new inference endpoints by 2027."
- **Dual-line SVG chart (right 40%):**
  - Line 1 (sw/teal): FLOP/$ improvement — 1.4× per year (Epoch AI, 2013–2024)
  - Line 2 (hw/purple): Training compute demand — 5× per year (Epoch AI, 2020–2025)
  - Gap between lines = Jevons effect. Annotated: "Efficiency ↑ → Demand ↑↑"
  - Source: Epoch AI
- Bottom stat: McKinsey — total DC demand: **82 GW (2025) → 219 GW (2030)** · +166% despite efficiency gains

---

### [DIVIDER] — Sphere I: Hardware
**Style:** Full-bleed divider. Large sphere number `01`. Sphere accent: `--hw` purple.
**Text:** `SPHERE I` / `HARDWARE` (Bebas Neue, 96px)
**Sub:** "Physical constraints, generation cycles, and the limits of silicon."

---

### S5 — GPU Generation Lifecycle: Acquisition → Lock → Flood
**Chrome:** Sphere I · Hardware · GPU Generation Lifecycle
**Act badge:** `HARDWARE` (hw color)
**Content:**
- **Main chart (top 60%):** Custom SVG horizontal timeline showing 4 GPU generations as lifecycle bands:

| Generation | Launch | Hyperscaler acquisition | Contract lock (4-6yr) | Secondary market flood |
|------------|--------|------------------------|----------------------|----------------------|
| H100 | 2022 | 2022–2023 | 2022–2027 | 2026–2027 |
| H200 | 2024 | 2024 | 2024–2029 | 2027–2028 |
| B200 | 2025 | 2025 | 2025–2030 | 2028–2029 |
| B300 | 2026 | 2026 | 2026–2031 | 2029–2030 |

  - Bands: color-coded by phase (acquisition=hw-dim, lock=hw-mid, flood=hw bright)
  - Overlay: where Orchestrators and Aggregators typically offtake (during lock phase and flood phase respectively)
  - Current date marker (Apr 2026): H100 approaching flood, B200 in acquisition

- **Bottom row (3 stat cards):**
  - "CoreWeave contract length: 4–6 years"
  - "H100 secondary market discount: est. 30–40% below launch price by 2027"
  - "B200: 4,500 TFLOPS · H100: 989 TFLOPS · 4.5× generational jump"

---

### S6 — The Energy Wall
**Chrome:** Sphere I · Hardware · The Energy Wall
**Act badge:** `HARDWARE`
**Content:**
- **Main chart (left 55%):** Stacked area SVG chart:
  - X: 2024–2030
  - Y: TWh
  - Data: 415 TWh (2024) → 945 TWh (2030), IEA forecast
  - Stacked: AI workloads (hw color) vs non-AI (dim)
  - Annotated: "AI drives ~60% of growth"
  - Source: IEA / Goldman Sachs

- **Right column (4 stat cards, stacked):**
  - "40–100 MW per frontier training cluster"
  - "$10–12M per MW to build AI-ready datacenter"
  - "2× cost of conventional datacenter"
  - "Location strategy > GPU procurement strategy"

- **Bottom insight box:** "Future expansion will decouple from fibre hubs → tether to isolated GW-scale power. Sovereignty becomes a primary procurement criterion."

---

### S7 — Silicon Beyond NVIDIA
**Chrome:** Sphere I · Hardware · Silicon Diversification
**Act badge:** `HARDWARE`
**Content:**
- **Top stat:** "NVIDIA controls 92% of GPU market (H1 2025). But the assumption that frontier AI requires NVIDIA has been broken."
- **Evidence card (highlighted):** Z.ai GLM-5.1 — #1 SWE-Bench Pro, trained entirely on Huawei Ascend. No NVIDIA hardware.
- **Comparison table (4 rows, 5 cols):**

| Vendor | Chip | TFLOPS (BF16) | Target workload | Status |
|--------|------|---------------|-----------------|--------|
| NVIDIA | B200 SXM | 4,500 | All (training + inference) | Market standard |
| AMD | MI300X | 1,307 | Training + large-model inference | 8% share, growing |
| Google | TPU v7p | N/A (proprietary) | Google-internal training | Lock-in strategy |
| AWS | Trainium2 | ~3,500 (est.) | Training (AWS-only) | Lock-in strategy |
| Huawei | Ascend 910C | ~2,000 (est.) | China sovereign AI | Geopolitical moat |

- **Bottom insight:** "Custom silicon = proprietary lock-in. Hyperscalers escape NVIDIA pricing; ecosystem exit becomes painful. Open GPU compute (NVIDIA/AMD) remains the default for third-party providers."

---

### S8 — Memory & Interconnect: The New Bottleneck
**Chrome:** Sphere I · Hardware · Bandwidth Constraints
**Act badge:** `HARDWARE`
**Content:**
- **Main chart (left 50%):** Dual bar/line SVG from gpu_economics_canonical_spec + SemiAnalysis data:
  - Primary bars: HBM bandwidth per generation (GB/s)
    - V100: 900 · A100: 2,000 · H100: 3,350 · H200: 4,800 · B200: 8,000
  - Secondary line: TFLOPS per generation
    - V100: 112 · A100: 312 · H100: 989 · H200: 1,979 · B200: 4,500
  - Key annotation: "TFLOPS growing 4.5× per generation; HBM bandwidth growing 8.9× (V100→B200) — bandwidth catching up but still lagging. Memory-bound inference is the bottleneck."
  - Source: NVIDIA specs + SemiAnalysis

- **Right column (3 insight cards):**
  - **Memory wall:** "At 70B BF16: 140GB model weights. A single H100 (80GB) can't fit it — 2 GPUs minimum. KV cache compounds this at scale."
  - **Interconnect gap:** "NVLink 5.0: 1,800 GB/s. PCIe 5.0: 64 GB/s. 28× gap. Distributed training on PCIe is ~7% efficient vs NVLink at 32 GPUs."
  - **Kyber rack:** "NVL144 (144 GPUs on one NVLink domain) → single job sees 144 GPUs as one virtual accelerator."

---

### [DIVIDER] — Sphere II: Software / Stack
**Style:** Full-bleed divider. Sphere number `02`. Accent: `--sw` teal.
**Text:** `SPHERE II` / `SOFTWARE` / `& STACK` (Bebas Neue, 96px)
**Sub:** "Orchestration, inference efficiency, and whether AI coding erodes the moat."

---

### S9 — Orchestration as the Moat: SUNK vs Soperator
**Chrome:** Sphere II · Software · Orchestration Layer
**Act badge:** `SOFTWARE` (sw color)
**Content:**
- **Top context (2 lines):** "Bare metal is the table stake. The moat is in the orchestration layer above it — specifically, the Slurm + Kubernetes integration that delivers gang scheduling without the hypervisor tax."
- **Main comparison table:**

| Feature | CoreWeave SUNK | Nebius Soperator | Commodity K8s |
|---------|---------------|-----------------|---------------|
| Philosophy | Proprietary, vertically integrated | Open-source, hardware-agnostic | Generic |
| Networking | BlueField-3 DPU offload (100% bare-metal) | Standard InfiniBand/RDMA | Ethernet/RoCE |
| Storage | CAIOS (disaggregated, DPU-linked) | Shared root filesystem (Lustre/NFS) | Varies |
| Scheduling | Topology-aware + predictive failure ("Mission Control") | Topology-aware + Slurm native | Basic |
| Gang scheduling | ✓ | ✓ | ✗ |
| Open source | ✗ | ✓ (GitHub) | ✓ |

- **Goodput bar chart (bottom-right, SVG):**
  - DePIN/consumer: ~70%
  - Commodity cloud/VMs: ~80%
  - Industry average bare metal: ~90%
  - Nebius Soperator: ~92%
  - CoreWeave SUNK: 96% (benchmark)
  - Source: CoreWeave whitepaper / benchmarks

- **Key insight callout:** "Soperator's open-source release changes the equation — any provider with the right hardware can now deploy SUNK-grade orchestration. The moat shifts from 'can you build it' to 'can you run it at 96% goodput.'"

---

### S10 — The Inference Efficiency Race
**Chrome:** Sphere II · Software · Inference Optimisation
**Act badge:** `SOFTWARE`
**Content:**
- **Main chart (top 55%, SVG log-scale):** Inference price per million tokens over time (recreate Epoch AI chart):
  - X: Oct 2021 – Apr 2025
  - Y: $/M tokens (log scale)
  - 3 lines: GPT-3.5 class (9×/yr decline) · GPT-4 class (40×/yr) · Fastest class (900×/yr)
  - Annotated: Oct 2022 GPT-3.5 class: $20/M → Oct 2024: $0.07/M (280× in 2 years)
  - Source: Epoch AI / Artificial Analysis

- **4 innovation cards (bottom, 2×2 grid):**
  - **FlashAttention** (Together AI): "2–5× inference throughput vs stock vLLM. Custom CUDA kernels rewrite memory access patterns."
  - **NVFP4 quantization** (NVIDIA GTC 2026): "4× memory reduction vs FP16. 4,500 TFLOPS accessible to more workloads."
  - **Speculative decoding:** "2–3× throughput for constrained outputs. Small draft model predicts; large model verifies."
  - **MIG (Multi-Instance GPU):** "8 parallel inference workloads on one B200. ICN PoC uses this — directly improves utilisation and per-customer economics."

- **Jevons callout (bottom):** "Every efficiency gain → lower cost/token → more API calls → more demand. The race has no finish line."

---

### S11 — Barrier to Code: Does AI Erode the Software Moat?
**Chrome:** Sphere II · Software · Barrier to Code
**Act badge:** `SOFTWARE`
**Content:**
- **Framing question:** "AI coding tools (Claude Code, Cursor, GitHub Copilot) are lowering the cost of writing infrastructure code. What happens to orchestration moats?"
- **Two-column split:**
  - **Left — What AI lowers the barrier to:**
    - Writing Kubernetes manifests and Slurm configs
    - Building custom inference APIs (FastAPI + vLLM wrappers)
    - Creating data pipeline tooling and CLI tools
    - Deploying standard open-source stacks (Axolotl, ComfyUI, OpenWebUI)
    - → "Tier 2–3 orchestrators become commoditised faster"
  - **Right — What it does NOT erode:**
    - Hardware-software co-optimization (CAIOS, BlueField-3 integration)
    - Proprietary CUDA kernels (Together AI's FlashAttention took expert researchers)
    - 96% goodput at 10,000-GPU scale (operational knowledge, not just code)
    - Predictive failure detection at hardware level
    - → "The moat shifts from proprietary code → proprietary systems knowledge + hardware-software co-optimization"

- **IC angle insight box (sw accent):** "As barrier to code drops, more developers build custom AI pipelines → more demand for cheap, flexible object storage for training data, RAG indices, model checkpoints, and agent memory. IC S3 at $7/TB is a direct beneficiary."

---

### S12 — Storage: The Silent Infrastructure Layer
**Chrome:** Sphere II · Software · Storage Economics
**Act badge:** `SOFTWARE`
**Content:**
- **TCO bar chart (left 40%, SVG):** Monthly storage cost at 1 PB:
  - AWS S3: ~$23,000/mo
  - CoreWeave (CAIOS): ~$30,000/mo
  - GCP Storage: ~$20,000/mo
  - IC S3: **$7,000/mo** (highlighted, ICN brand green)
  - Annotated: "4.3× cheaper than CoreWeave. $23K/month savings at 1 PB."

- **Right column — 3 agentic storage requirement cards:**
  - **Training checkpoints:** "Frontier model training saves checkpoints every 10–30 min. At 70B BF16: ~140GB per save. 1,000 saves = 140TB."
  - **KV cache / RAG index:** "Agentic workflows write context + history to persistent storage. Vector DBs (LanceDB, LlamaIndex) write to S3 backends natively."
  - **Model artefacts:** "Fine-tuned model weights + LoRA adapters. IC stores both the training data AND the output — no egress to move between training and serving."

- **IC reverse playbook callout (icn green):** "'Bring compute to your data, not data to compute.' IC's existing 1PB customer (currently on CoreWeave) is the first land-and-expand target."

---

### [DIVIDER] — Sphere III: AI / Models
**Style:** Full-bleed divider. Sphere number `03`. Accent: `--llm` amber.
**Text:** `SPHERE III` / `AI / MODELS` (Bebas Neue, 96px)
**Sub:** "Open-weight compression, reasoning models, agentic workloads, and fine-tuning proliferation."

---

### S13 — Open-Weight Frontier Compression
**Chrome:** Sphere III · AI/Models · Open-Weight Race
**Act badge:** `AI/MODELS` (llm color)
**Content:**
- **Main chart (left 50%, SVG scatter):** Model ELO score vs inference cost ($/M tokens):
  - X: cost per million output tokens (log scale, $0.01–$100)
  - Y: Arena ELO (1350–1460)
  - Plotted points: GPT-4o ($5, ~1420), Claude Opus 4.6 ($15, ~1450), Gemma 4 31B ($0.10, ~1452), Gemma 4 26B A4B ($0.03, ~1440), DeepSeek-R1 ($2.19, ~1430), GLM-5.1 ($1.00, ~1451)
  - Trend arrow: "Frontier quality at commodity cost" (amber diagonal arrow)
  - Key callout: "Gemma 4 26B A4B: 4B active params at inference. Frontier-adjacent quality at ~$0.03/M tokens."

- **3 model highlight cards (bottom):**
  - **Gemma 4 31B** (Google, Apache 2.0): "Top open-weight at launch. AIME 2026: 89.2%. Runs on 2× H100 at BF16. Coordination: HF, vLLM, Ollama, Unsloth — day-0."
  - **GLM-5.1** (Z.ai, Apache 2.0): "754B MoE, 40B active. #1 SWE-Bench Pro. Trained on Huawei Ascend — no NVIDIA. $1.00/M input tokens."
  - **DeepSeek-R1 / Qwen 2.5** (open weights): "Chinese labs closing gap. Compressing margins on proprietary APIs industry-wide."

- **Jevons marker:** "Open-weight access → inference cost → $0.03/M. More developers build AI products → more GPU demand."

---

### S14 — The Agentic Shift: A New Infrastructure Class
**Chrome:** Sphere III · AI/Models · Agentic Workloads
**Act badge:** `AI/MODELS`
**Content:**
- **McKinsey chart (top 45%, rebuilt as SVG stacked bar from JSON):**
  - Stacked bars 2025–2030: Non-AI (dim) / AI Training (hw) / AI Inference (llm amber)
  - Key labels: Inference CAGR 35% vs Training CAGR 22% vs Non-AI 11%
  - Annotated: "Inference overtakes training as primary compute consumer"
  - Source: McKinsey Data Center Demand Model

- **Infrastructure requirements table (bottom):**

| Dimension | Traditional Inference | Agentic Inference |
|-----------|--------------------|------------------|
| Session duration | Seconds | Up to 8 hours (Z.ai GLM-5.1) |
| Context window | 4K–32K tokens | 200K–1M tokens |
| CPU/GPU ratio | 1:8 | 1:2 to 1:4 |
| KV cache | Small, ephemeral | Large, persistent |
| Storage access | Minimal | Frequent (checkpoints, tool output) |
| Job scheduling | Stateless REST | Gang scheduling, checkpointing |
| New requirement | — | Adjacent high-perf object storage |

- **Bottom insight:** "8-hour autonomous execution loops (Z.ai) are not just a longer chat session. They are a qualitatively different workload class — more like HPC jobs than API calls."

---

### S15 — Fine-Tuning Proliferation: The ICP-4 Signal
**Chrome:** Sphere III · AI/Models · Fine-Tuning Proliferation
**Act badge:** `AI/MODELS`
**Content:**
- **Top stat row (3 cards):**
  - "QLoRA 70B: $6–20 per training run (12–20h on A100)"
  - "Full fine-tune 70B: $4,000–$9,750 (672GB VRAM required)"
  - "QLoRA memory: 46GB — fits on single H100"

- **ICP-4 profile card (left 45%, amber border):**
  - **Who:** Knowledge-intensive firms (legal, pharma, finance, enterprise software)
  - **Pain:** Proprietary data can't leave firm. Hyperscaler storage costs prohibitive. Egress fees on training data movement.
  - **What they need:** 8–64 GPUs (not 10,000). High-speed storage adjacent to compute. Simple setup. Data residency.
  - **Why now:** Open-weight models (Llama 4, Qwen 2.5, Gemma 4) make proprietary fine-tuning accessible. Hiring ML researchers from frontier labs.

- **Right column — cost comparison waterfall (SVG):**
  - Stacked bar: IC compute (est.) + IC storage $7K/mo vs CoreWeave compute + CoreWeave storage $30K/mo
  - Label: "At 1PB training data: $23K/month savings on storage alone — before any egress fees"

- **Bottom tension box (amber):** "ICP-4 enterprises require fiat pricing — they will not use crypto rails. The avenue comparison table (S20) includes this as a criterion. See Appendix S24 for the full ICNT/fiat framework."

---

### [DIVIDER] — The Market Response
**Style:** Full-bleed divider. No sphere number. Accent: white/neutral.
**Text:** `THE` / `MARKET` / `RESPONSE` (Bebas Neue, 96px)
**Sub:** "How each business model is responding to the three spheres — and where everyone is moving."

---

### S16 — The 2×2: Stack Position × Asset Model
**Chrome:** The Market Response · Position Map
**Act badge:** `MARKET MAP`
**Content:**
- **Main chart (2×2 SVG position map, 65% of slide):**
  - X-axis: Stack position — Infrastructure (L1–L3) ← → Software (L5–L7)
  - Y-axis: Asset model — Asset-Heavy ↑ · Asset-Light ↓
  - Quadrant labels (dim, background): top-left = "Neocloud", top-right = "Hyperscaler", bottom-left = "Aggregator", bottom-right = "Orchestrator"
  - **Plotted companies** (from market_positioning.json, colored by archetype):
    - CoreWeave: top-left (asset-heavy, infrastructure)
    - Lambda Labs: mid-left (asset-heavy, slight software tilt)
    - Nebius: top-center-left (asset-heavy, moving right with Token Factory)
    - AWS/GCP/Azure/OCI: top-right (asset-heavy, full stack)
    - Together AI: center-right (asset-medium, software-heavy — moving left with data center)
    - Shadeform/Vast.ai: bottom-left (asset-light, aggregator)
    - Run:ai/Anyscale: bottom-right (asset-light, orchestrator)
    - ICN PoC: mid-left with arrow showing B2 target position (teal/icn)

  - **Movement arrows** (key narrative):
    - Neoclouds → right: building Token Factory (Nebius), inference serving (CoreWeave)
    - Orchestrators → left: Together AI building own data centers
    - Hyperscalers → down-left: custom silicon (TPU/Trainium) moving toward infrastructure ownership
    - DePIN → up: trying to move from consumer to enterprise

- **Right legend (4 archetype cards, compact):** Hyperscaler / Neocloud / Orchestrator / Aggregator with 1-line moat description

---

### S17 — Business Model × Sphere Response Matrix
**Chrome:** The Market Response · BM Heatmap
**Act badge:** `MARKET MAP`
**Content:**
- **Full-slide heatmap (SVG):** 4 rows (BMs) × 9 columns (3 trends per sphere):

|  | HW: GPU lifecycle | HW: Energy wall | HW: Non-NVIDIA | SW: Orchestration | SW: Inference eff. | SW: Barrier to code | AI: Open-weight | AI: Agentic | AI: Fine-tuning |
|--|---|---|---|---|---|---|---|---|---|
| **Hyperscaler** | ✓ (absorbs flood) | ✓ (own power) | ✓ (custom silicon) | ~ (hypervisor tax) | ✓ (resources) | ~ | ~ | ✓ | ~ |
| **Neocloud** | ~ (lock-in risk) | ~ (dependent) | ✗ (NVIDIA dependent) | ✓✓ (SUNK/Soperator) | ✓ (hardware opt.) | ~ | ✓ | ✓ | ✓ |
| **Orchestrator** | ✓ (hardware-agnostic) | ✓ (light, agnostic) | ✓ (any silicon) | ✓ (software moat) | ✓✓ (FlashAttention) | ✗ (moat erosion risk) | ✓✓ | ✓✓ | ✓✓ |
| **Aggregator** | ✓ (flood = supply) | ✓ (asset-light) | ✓ (any source) | ✗ | ✗ | ✓ (low bar) | ~ | ✗ | ✗ |

- Color scale: ✓✓ = green (strong response) · ✓ = teal dim · ~ = yellow (exposed) · ✗ = red (vulnerable)
- Bottom insight: "Orchestrators are best-positioned for software sphere trends. Neoclouds own the hardware sphere. Neither archetype dominates all three. ICN's play straddles the gap."

---

### S18 — Where Competitors Are Moving and Why
**Chrome:** The Market Response · Competitor Trajectories
**Act badge:** `MARKET MAP`
**Content:**
- **6 trajectory cards (2×3 grid), one per key player movement:**
  - **CoreWeave → Software**: Launched inference serving + CAIOS storage. Moving from "GPU for rent" to "AI Factory as a Service." Revenue driver: inference OpEx stickiness.
  - **Nebius → Open + EU**: Token Factory (60+ open models, fine-tuning, distillation). Soperator released open-source. EU-first positioning for GDPR tailwind. Raising $4.3B.
  - **Together AI → Hardware**: Own data centers + FlashAttention custom kernels. Hybrid (software moat + hardware control). Target: inference-first enterprises.
  - **Hyperscalers → Custom Silicon**: TPU v7, Trainium2, MTIA. Vertical integration to escape NVIDIA pricing. Creates ecosystem lock-in — open-source community hostile to proprietary silicon.
  - **Orchestrators → Inference specialization**: Groq (ASIC inference), Fireworks AI, Baseten. Proprietary inference serving becomes a product category.
  - **DePIN → Enterprise**: Aethir, Salad moving toward enterprise compliance. ICN is in this transition zone.

- **Synthesis callout (bottom):** "Convergence is the story — neoclouds building software, orchestrators building hardware. The market is collapsing toward full-stack 'AI Factory' providers. The gap being vacated: mid-tier, storage-adjacent, developer-focused compute."

---

### [DIVIDER] — ICN's Play
**Style:** Full-bleed divider. Accent: `--icn` neon green (#5AF688 on black).
**Text:** `ICN'S` / `PLAY` (Bebas Neue, 96px)
**Sub:** "Where we are, where we fit, and the decision that needs to be made."

---

### S19 — ICN Key Resources & Capabilities
**Chrome:** ICN's Play · R&Cs
**Act badge:** `ICN`
**Content:** Two-column R&C split — what ICN brings standalone vs. what becomes available when coupled with IC.

**Left — ICN standalone:**
| Asset | Detail | Value |
|-------|--------|-------|
| PoC Hardware | Blackwell 6000, MIG-capable | Inference + QLoRA (8–64 GPU range) |
| ICNT Token | On-chain settlement, Base/Ethereum | DePIN positioning (also a GTM constraint — see Appendix) |
| ICN team | Strategy + technical PoC | Speed of execution |

**Right — IC-coupled (ICN + Impossible Cloud):**
| Asset | Detail | Value |
|-------|--------|-------|
| IC S3 Storage | ~$7/TB, zero egress, petabyte-scale | 4.3× cheaper than CoreWeave; data gravity moat |
| Nitro | High-performance S3 for GPU workloads | Eliminates checkpointing idle time; native vLLM/Axolotl backend |
| 1PB anchor customer | Training data on IC, compute currently on CoreWeave | First land-and-expand target — "bring compute to the data" |
| IC customer base | Established enterprise storage relationships | Warm pipeline for B1/B2 avenues |

**Bottom — Gaps (apply to both):**
- InfiniBand / NVLink high-speed interconnect (required for >32 GPU training)
- Managed orchestration (Soperator is now open-source — gap is deployment, not licensing)
- Compliance posture (SOC 2 Type II: 12–18 months)
- Developer experience layer (vLLM, Axolotl, ComfyUI pre-configured)
- Fiat pricing rails (required for ICP-4 enterprise; see Appendix S24)

**Callout (icn green):** "The IC-coupling multiplies ICN's defensibility. Standalone ICN competes with hundreds of GPU resellers. IC-integrated ICN competes with almost no one."

---

### S20 — Two Avenues into the Market
**Chrome:** ICN's Play · Strategic Options
**Act badge:** `ICN`
**Content:** Two avenue panels (top) + full comparison table (bottom).

**Top — Avenue 1: Clean-Slate Entry** (neutral border, left panel ~50%):
Two sub-options, horizontal within the panel:
- **A1 — Orchestrator-only** (sw teal accent):
  - "Software-only layer on third-party GPU inventory. Asset-light. Fast."
  - ICP-2 / ICP-3 (AI startups, MLOps engineers)
  - No hardware capex. Competes on DX. Soperator + managed APIs.
- **A2 — EU Neocloud-lite** (hw purple accent):
  - "ML-native bare-metal cloud for European AI builders. GDPR by default."
  - ICP-2 + ICP-4 (AI startups + fine-tuning SMEs)
  - Requires: InfiniBand, compliance posture, own hardware fleet

**Top — Avenue 2: IC-Integrated (Reverse Playbook)** (icn green border, right panel ~50%):
Three sub-options, vertical within the panel:
- **B1 — Storage-adjacent inference** (sw teal accent):
  - "Run inference on data already stored in IC. Zero egress."
  - ICP-3 / ICP-4 (active IC customers with inference workloads)
  - Lowest friction — no new acquisition needed
- **B2 — Fine-tuning on IC storage** (icn green accent, **RECOMMENDED**):
  - "Your training data is already on IC. Fine-tune here — zero egress, 4× cheaper storage."
  - ICP-4 (fine-tuning SMEs with training data on IC)
  - Highest defensibility (data gravity moat + IC synergy). Fast time to revenue.
- **B4 — Sovereign EU compute** (amber accent):
  - "Data residency + GDPR + EU AI Act compliance stack for regulated industries."
  - ICP-5 (enterprise/regulated — pharma, finance, legal)
  - High regulatory tailwind. Slower sales cycle. High capital.

**Bottom — Full comparison table (10 criteria):**

| Criterion | *Definition* | A1 Orch. | A2 EU Neo | B1 Inf. | B2 FineTune | B4 Sovereign |
|-----------|-------------|----------|-----------|---------|------------|-------------|
| Capital intensity | Upfront hardware + compliance capex required | Low | High | Medium | Low–Med | High |
| Time to revenue | Months to first paying customer | 3–6 mo | 12–18 mo | 3–6 mo | 3–6 mo | 12–18 mo |
| Gross margin ceiling | At scale, net revenue after COGS | ~50% | ~35% | ~45% | ~50% | ~30% |
| Competitive pressure | Number of credible competitors today | High | Medium | Medium | Low | Low |
| AI moat — external ¹ | How AI market trends (Jevons, cost deflation, open-weight) affect this position | Low | Medium | Med–High | High | Medium |
| AI moat — internal ² | How ICN's own AI use creates compounding operational advantage | Low | Medium | High | High | Medium |
| Data gravity | Whether data already on IC creates structural lock-in | None | None | High | Very high | Medium |
| Regulatory tailwind | GDPR, EU AI Act, data residency as a GTM driver | Medium | High | Medium | Medium | Very high |
| NVIDIA dependency | Exposure to NVIDIA pricing / supply constraints | Medium | High | High | Medium | High |
| IC synergy | How much IC assets amplify ICN's competitive position | None | Low | Very high | Very high | Medium |

**Footnote definitions:**
> ¹ **AI moat — external:** How the AI market's structural trends (Jevons-driven cost deflation, open-weight model proliferation, Jevons demand explosion) erode or reinforce ICN's competitive position in this avenue. *High = trends reinforce; Low = trends erode.*
> ² **AI moat — internal:** How ICN's own deployment of AI (fine-tuning on proprietary workload data, agentic infrastructure management, AI-optimized DX tooling) creates an operational advantage that compounds over time and is hard to replicate. *High = strong compounding advantage; Low = minimal differentiation from AI use.*

---

### S21 — The Thesis
**Chrome:** ICN's Play · The Thesis
**Act badge:** `ICN`
**Content:** Clean, high-impact thesis slide (minimal text, large type):
- **Perspective B2 Thesis** (large Bebas Neue, 40px):
  > "Your training data is already on Impossible Cloud. Fine-tune your models here — zero egress, 4× cheaper storage, your data never leaves."
- **Mechanism** (3-step flow, icn green):
  1. IC S3: data gravity → fine-tuning SME stores training data on IC
  2. ICN Compute: bring compute to the data → Axolotl/QLoRA on Blackwell 6000 + Soperator
  3. IC Inference: "train on IC, serve on IC" flywheel → agentic storage long-term moat
- **Flywheel diagram:** Data → Compute → Output → Agent Memory → back to Storage
- **Three immediate actions (icn green, bottom):**
  1. Fiat pricing decision (prereq — see Appendix S24)
  2. Soperator deployment on PoC hardware
  3. Outreach to the 1PB IC anchor customer

---

### S22 — Frontier Signals + Export
**Chrome:** Frontier Signals · April 2026
**Act badge:** `SIGNALS`
**Content:** 4 signal cards + PDF export button:
- Glasswing (Anthropic Apr 2026): frontier model capability as a security event
- GLM-5.1 on Ascend: NVIDIA-free frontier training
- Gemma 4 on iPhone: edge inference wave + Jevons on-device
- OpenClaw/NemoClaw (NVIDIA GTC): agentic OS layer emerging at infrastructure level
- PDF export button (window.print())

---

### S24 — [APPENDIX] ICNT / Fiat Decision Framework
**Chrome:** Appendix · ICNT / Fiat Decision
**Act badge:** `APPENDIX`
**Note:** This slide is outside the main deck flow. It is included for the all-hands document distribution and for leadership discussions in Zug. Not presented in the live Seb session unless specifically requested.

**Content:**
- **Header:** "Before any enterprise sales motion: one decision must be made."
- **ICP compatibility table:**

| ICP | Token preference | Fiat preference | Compatible with ICNT-only? |
|-----|-----------------|-----------------|---------------------------|
| ICP-2 AI startup | Neutral (prefers fiat) | Yes | No |
| ICP-4 Fine-tuning SME | Refuses crypto rails | Yes | No |
| ICP-5 Enterprise regulated | Refuses crypto rails | Yes | No |
| DePIN / Web3 native | Requires ICNT | No | Yes |

- **Three options (compact cards):**
  - **Option 1 — Dual-track:** Fiat pricing for enterprise ICPs + ICNT for DePIN native. Requires product segmentation. Increases GTM complexity.
  - **Option 2 — Enterprise-first fiat:** Launch with fiat for ICP-2/ICP-4. ICNT as optional settlement layer, not required. Broadest addressable market.
  - **Option 3 — DePIN-first:** ICNT-native. Natural ICNT token utility. Excludes ICP-2/ICP-4 until fiat added. Narrower near-term market.
- **Recommendation callout (amber):** "For B2 (highest-fit avenue), fiat pricing is a prerequisite. This is not a technical decision — it is a leadership alignment between ICN and IC. Recommend Option 2 as the default unless there is a specific reason to prioritize ICNT token utility over the enterprise pipeline."
- **Timeline:** "Decision deadline: before all-hands in Zug (~May 1, 2026)"

---

## Chart Summary

| Slide | Chart type | Data source |
|-------|-----------|-------------|
| S3 | Historical arc SVG timeline (coal→cars→computing→AI) | Hardcoded + Epoch AI |
| S4 | Flow diagram SVG + dual-line chart (FLOP/$ vs training compute) | Epoch AI (hardcoded from charts) |
| S4 | McKinsey stacked bar (82→219 GW) | `gpu_AI_workloads_2025-2030.json` |
| S5 | Horizontal timeline SVG (GPU lifecycle) | Hardcoded (from notes) |
| S6 | Stacked area SVG (DC power TWh) | IEA/GS data (hardcoded) |
| S7 | Comparison table | Hardcoded |
| S8 | Dual bar/line SVG (VRAM + TFLOPS by generation) | `gpu_economics_canonical_spec_v1.json` |
| S9 | Goodput bar chart SVG | Hardcoded (from notes) |
| S10 | Log-scale line chart SVG (inference prices) | Hardcoded (from Epoch AI chart) |
| S12 | Bar chart SVG (storage TCO) | Hardcoded |
| S13 | Scatter SVG (ELO vs $/M tokens) | Hardcoded (from notes + web) |
| S14 | Stacked bar SVG (McKinsey workloads) | `gpu_AI_workloads_2025-2030.json` |
| S14 | Infrastructure requirements table | Hardcoded |
| S15 | Cost waterfall bar SVG | Hardcoded |
| S16 | 2×2 position map SVG (interactive, hover) | `market_positioning.json` |
| S17 | Heatmap SVG | Hardcoded |
| S20 | 5-option comparison table (10 criteria) | Hardcoded |

---

## Component Reuse from (pres3)inference-compute-deck.html

| Component | Used in |
|-----------|---------|
| `.scenario-col` / `.scenario-tag` | S20 (positioning options) |
| `.force-point` / `.fp-num` / `.fp-body` | S6, S7 stat cards |
| `.tl-band` / `.tl-col` / `.tl-year` / `.tl-events` | S5 (GPU lifecycle timeline) |
| `.comp-card` / `.comp-archetype` / `.stack-strip` | S16 (position map legend) |
| `.ornn-card` / `.ornn-grid` | S18 (competitor trajectories) |
| `.sw-gain-box` / `.sw-gain-num` | S3, S6, S12 stat callouts |
| `.wl-card` / `.wl-metric-bar` | S14 (infrastructure requirements) |
| Divider slide CSS (new, modelled on pres2) | S dividers × 5 |

---

## Spec Self-Review

- **Placeholders:** None. All data sources are specified (JSON files or hardcoded from confirmed numbers). AI moat criteria have explicit definitions in S20 footnotes.
- **Consistency:** Sphere colors consistent throughout (hw=purple, sw=teal, llm=amber, icn=green). Act badges match sphere naming. ICNT/fiat removed from main flow — referenced only as footnote in S15 and full treatment in S24 Appendix.
- **Scope:** 22 content slides + 1 appendix (S24) + 5 dividers = 28 total visual slides. S3 is now a story/atmosphere slide (lighter data load); S4 carries the data. Appropriate for a 45–60 min session — S4 can be skipped in a time-pressured live session if needed.
- **Ambiguity:** S16 2×2 company positions are approximate from market_positioning.json — not pixel-perfect coordinates but representative positioning. Noted in chart spec. S20 comparison table ratings (Low/Medium/High) are directional judgements, not quantitative scores.
- **ICN brand color:** `#5AF688` (neon green from icn-deck skill) used for ICN-specific slides and callouts.

---

*Spec written by Claude (claude-sonnet-4-6) · 2026-04-10*
*Next step: user review → invoke superpowers:writing-plans*
