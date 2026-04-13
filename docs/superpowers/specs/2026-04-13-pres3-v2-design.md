# Pres 3 Redesign — Design Spec v2
*2026-04-13 · Author: J. Skifter / Claude · Status: Awaiting user sign-off*
*Supersedes: `docs/superpowers/specs/2026-04-10-pres3-redesign-design.md`*

---

## Purpose

Full rebuild of `pres3_gpu-trends-positioning-deck.html`. This v2 spec incorporates all v1 design decisions plus the following additions and changes agreed in the 2026-04-13 design session:

1. **New Venn slide** — "So What's Going On?" — industry snippets → animated three-sphere Venn triangle
2. **Energy Wall** — comparable household numbers, pre-AI vs AI-era DC history, nuclear deals
3. **Memory & Interconnect** — reframed from "new bottleneck" to "compute-bound → memory-bound" evolution
4. **Orchestration slide** — add explicit scale/moat tension (software erosion vs. bare-metal permanence)
5. **Storage → "Context is Gold"** — agentic workflow metrics, SwitchingOps, enterprise scale
6. **Sphere III additions** — frontier labs, Anthropic Managed Agents, edge compute distribution slide
7. **ICN positioning** — context for model serving SMEs, not just training/fine-tuning
8. **Foldable sections** — `<details>/<summary>` inline + existing `#dp` side panel for deep content
9. **Term tooltips** — `data-term` attributes on key terms, linking to `manual_notes/terms/`
10. **Sources folder** — all new data sourced to `sources/` (reports/, x-posts/, etc.)

Presentation date: **live this week** (Seb working session Mon Apr 13 — full deck for later sessions).  
All-hands document: **~May 1, 2026**.

---

## Architecture

*Identical to v1 — reproduced for completeness.*

**File:** `pres3_gpu-trends-positioning-deck.html` (overwrite in place)  
**Type:** Single-file self-contained HTML (no external assets beyond Google Fonts)  
**Design system:** Inherits from `(pres3)inference-compute-deck.html`
- Fonts: Bebas Neue (display) · Barlow (body) · JetBrains Mono (mono/labels)
- Dark default + light toggle
- Chrome bar top + nav bar bottom + progress bar + slide counter
- Slide transitions: translateX with cubic-bezier(0.76,0,0.24,1)
- fadeUp stagger animations `.anim-1` through `.anim-6`

**Sphere color mapping (consistent throughout):**
| Sphere | CSS var | Dark | Light |
|--------|---------|------|-------|
| Hardware | `--hw` | `#c084fc` (purple) | `#7c3aed` |
| Software | `--sw` | `#2dd4bf` (teal) | `#0d9488` |
| AI/Models | `--llm` | `#fbbf24` (amber) | `#b45309` |
| ICN Play | `--icn` | `#5AF688` (neon green) | `#059669` |

**Data files:**
- `data_files/gpu_AI_workloads_2025-2030.json` — McKinsey workload data
- `data_files/gpu_economics_canonical_spec_v1.json` — GPU specs by generation
- `data_files/market_positioning.json` — competitor 2×2 positions

**Charts:** All inline SVG or Canvas — no `<img>` tags.

---

## New Infrastructure: Tooltips & Foldable Sections

### Term tooltips
Key terms throughout the deck get `data-term="<term-slug>"` attributes. A global JS handler reads this and shows a tooltip card on hover (reusing existing `#tip` element). Tooltip content is inlined as a JS lookup object at the bottom of the file.

Terms to tooltip: `jevons-paradox`, `sunk`, `soperator`, `mig`, `mfu`, `goodput`, `kv-cache`, `depin`, `switching-ops`, `managed-agents`, `moe` (Mixture of Experts), `ppa` (Power Purchase Agreement), `qloRA`, `mip` (Multi-Instance GPU — same as MIG).

### Foldable deep-detail sections
Used on data-heavy slides where the presenter may want additional context available but not shown by default. Implementation: `<details class="fold-detail"><summary class="fold-summary">↓ More detail</summary>...</details>` with minimal CSS styling (border-top, mono font for summary label). Used sparingly — only where there is genuinely useful secondary content that would clutter the slide.

### Side panel (`#dp`)
Existing detail panel, already in the deck. Used when clicking on interactive elements (companies in 2×2, force tiles, competitor cards) for expanded context. No changes to the panel component itself.

---

## Slide-by-Slide Specification

*Slides that are unchanged from v1 are marked `[INHERITED FROM V1]` with only delta notes.*

### S1 — Cover `[INHERITED FROM V1]`
No changes.

---

### S2 — Recap: What We Covered `[INHERITED FROM V1]`
No changes.

---

### [DIVIDER] — The Jevons Lens `[INHERITED FROM V1]`

---

### S3 — Jevons Paradox: The Meta-Law `[INHERITED FROM V1]`
No changes to cinematic story slide design.

---

### S4 — Jevons in Action: Three Spheres, One Outcome `[INHERITED FROM V1]`
No changes.

---

### ★ NEW — [DIVIDER] — So What's Going On?
**Style:** Full-bleed divider. No sphere number. Accent: white/neutral.  
**Text:** `SO WHAT'S` / `GOING ON?` (Bebas Neue, 96px)  
**Sub:** "Twelve signals from the people building and studying the GPU stack — right now."

---

### ★ NEW — S-Venn — Industry Pulse: 12 Signals

**Chrome:** Industry Pulse · What the Field Is Saying  
**Act badge:** `SIGNALS`

**Purpose:** Ground the abstract Jevons framework in concrete, attributed real-world signals. Two-phase interactive slide.

#### Phase 1 — Scattered card wall (initial state)
- 12 snippet cards distributed across the full slide in a **scattered, slightly rotated** layout (random-seeded positions within a safe zone — avoid nav and chrome overlap)
- Each card: `120–160px wide`, sphere-color left border (`--hw`, `--sw`, or `--llm`), white background with dim text
- Card content: short quote (1–2 sentences max), attribution line (name + role), source tag (Epoch AI, GTC 2026, etc.)
- Cards overlap slightly — intentional "wall of news" feel
- Bottom bar: small label `"Click → to see what these signals tell us"`

#### Phase 2 — Venn triangle (after next →)
On click/arrow: animate cards flying to their sphere positions in a **equilateral Venn triangle**:

```
              [AI/MODELS]
             /     M3     \
            /  center      \
      [HW↔AI]   (anchor)  [SW↔AI]
          /                  \
   [HARDWARE] ── [HW↔SW] ── [SOFTWARE]
```

**Venn geometry (CSS positioning, approximate):**
- Hardware sphere: bottom-left circle center
- Software sphere: bottom-right circle center
- AI/Models sphere: top-center circle center
- Circles: large `border-radius: 50%`, semi-transparent fill (`--hw-dim`, `--sw-dim`, `--llm-dim`), no hard border
- Overlap zones: where circles visually intersect — cards placed there span the boundary

**Card placement per Venn zone:**
| Zone | Cards |
|------|-------|
| Pure Hardware | H2 (Epoch 3.3×/yr), H3 (energy constraint), H4 (capex/MW) |
| Hardware ↔ Software | H1 (tokens/watt — Jensen), S1 (bare-metal migration), S3 (egress lock-in) |
| Pure Software | S2 (LiteLLM attack) |
| Software ↔ AI/Models | S4 (L5 inference revenue), M2 (8-hr agentic sessions), M4 (fine-tuning moat) |
| Hardware ↔ AI/Models | M1 (Gemma 4 MoE economics) |
| CENTER (all three) | M3 (Jevons Paradox) — rendered as a fixed callout, not a card |

**Center callout (fixed, always visible in Phase 2):**
```
[center of Venn, larger type, icn-green or white]
"Understanding the interplay of all three
forces is the key to positioning."
```

**Animation:**
- Each card gets a CSS transition: `transform: translate(Δx, Δy) rotate(0deg)` → from scatter position to Venn target
- Duration: 0.7s cubic-bezier(0.76,0,0.24,1), staggered 0.04s between cards
- Center callout fades in (opacity 0→1) after cards land (0.9s delay)

**Snippet content** (sourced from `sources/x-posts/venn-slide-snippets-curated.md`):
See that file for all 12 with full attribution and placement. Summary:
- H1: Jensen Huang — "Tokens per watt is the new performance metric"
- H2: Epoch AI — "3.3× per year capacity growth / 7-month doubling"
- H3: Epoch AI — "Power is the constraint likely to bind first"
- H4: SemiAnalysis — "$10–12M/MW, 40–100 MW clusters"
- S1: Hugo Shi — "Hypervisor tax no longer acceptable for frontier labs"
- S2: TrendMicro — LiteLLM supply chain attack (March 2026)
- S3: Hugo Shi — "Egress fee is the real lock-in, not hardware"
- S4: SemiAnalysis — "L5 inference serving captures enterprise OpEx"
- M1: Google/ICN — "Gemma 4 26B A4B: frontier quality at 4B economics"
- M2: Z.ai — "GLM-5.1: 8-hour autonomous sessions — a new workload class"
- M3: Jevons — center anchor
- M4: Lambert/Raschka — "Fine-tuned open-weight = enterprise moat"

---

### [DIVIDER] — Sphere I: Hardware `[INHERITED FROM V1]`

---

### S5 — GPU Generation Lifecycle `[INHERITED FROM V1]`
No changes.

---

### ★ UPDATED — S6 — The Energy Wall

**Chrome:** Sphere I · Hardware · The Energy Wall  
**Act badge:** `HARDWARE` (hw color)

**Changes from v1:** Comparable numbers added (household benchmark, pre-AI era baseline, nuclear deals). Performance charts from GTC 2026.

#### Main chart (top 55%, left 60%) — UPDATED
Stacked area SVG chart (same as v1):
- X: 2020–2030 | Y: TWh
- Data: 415 TWh (2024) → 945 TWh (2030), IEA base case
- Stacked: AI workloads (hw color) vs non-AI (dim)
- Source: IEA Energy and AI Report, 2025

**NEW annotation line on chart:** "Pre-AI: 200–223 TWh (2020) → efficiency gains were *flattening* this curve. AI reversed it."
**NEW secondary axis callout:** "AI/GPU servers: +30%/yr vs. conventional servers: +9%/yr"

#### Right column stat cards (UPDATED) — 5 cards:
1. "GB200 NVL72 rack: **~120 kW** continuous draw" — with tooltip: "One rack = ~102 average US homes"
2. "Frontier training cluster: **40–100 MW** — a small town's electricity demand"
3. "AI-ready datacenter: **$10–12M per MW** to build — 2× a conventional facility"
4. "**H100 → B200 efficiency:** 25× more tokens/watt. Jevons outcome: more GPU demand, not less." (data-term="jevons-paradox")
5. NEW: "Big Tech contracted **10+ GW of nuclear** in 2025 — Microsoft, Google, Amazon, Meta. Power location > GPU procurement." (data-term="ppa")

#### NEW household comparison callout bar (bottom-left):
```
[hw-colored bar]
How much is 120 kW? 
One GB200 NVL72 rack = ~102 average US homes, running continuously.
A 100-rack cluster = ~10,000 homes.
The Vera Rubin NVL144 rack (2027): ~600 kW = ~508 homes — per rack.
```
Source: EIA household avg 10,332 kWh/year; NVIDIA rack TDP specs

#### NEW foldable detail section:
```html
<details class="fold-detail">
  <summary class="fold-summary">↓ Pre-AI datacenter profile vs today</summary>
  [Table: 2015 (CPU era, 8–12 kW/rack) | 2020 (cloud era, 8–25 kW/rack) | 2026 (AI era, 120–600 kW/rack)]
  [Note on workload shift: storage/networking/CPU → GPU training + inference with liquid cooling]
</details>
```

---

### ★ UPDATED — S7 — Silicon Beyond NVIDIA `[INHERITED FROM V1 WITH MINOR UPDATES]`

Add GLM-5.1 evidence more prominently (already in v1 but undersell it):
- Update evidence card: "GLM-5.1 (#1 SWE-Bench Pro, April 2026): trained entirely on Huawei Ascend 910C. No NVIDIA hardware. 754B MoE, Apache 2.0."
- Add tooltip on "Huawei Ascend": context on export control implications

---

### ★ UPDATED — S8 — From Compute-Bound to Memory-Bound

**Chrome:** Sphere I · Hardware · Bandwidth Constraints  
**Act badge:** `HARDWARE`

**REFRAME from v1 title "Memory & Interconnect: The New Bottleneck":**

> "Memory bandwidth has always been a bottleneck in GPU computing — the 'memory wall' is a decades-old HPC concept. What's changed is the *relative* severity: TFLOPS grow exponentially per generation while memory bandwidth grows more linearly. Many inference workloads have crossed from compute-bound to memory-bound — meaning adding more raw compute (TFLOPS) no longer speeds them up."

**New slide structure:**

**Top stat:** "H100 → B200: TFLOPS grew **4.5×**. Memory bandwidth grew **2.4×** (3.35 → 8 TB/s). The gap is widening each generation."

**Main chart (left 50%, SVG) — UPDATED:**
- Dual bar/line as in v1 (TFLOPS + bandwidth by generation)
- NEW: third line — **memory bandwidth × TFLOPS ratio** (showing the growing gap)
- Key annotation: "When bandwidth growth < TFLOPS growth → more workloads become memory-bound"
- NEW annotation: "At B200: a 70B BF16 model inference is ~85% memory-bandwidth-limited, not compute-limited"

**Right column (3 cards) — UPDATED titles:**
1. **Memory wall (updated):** "70B BF16: 140GB model weights. KV cache adds 10–260GB depending on context length. Memory is the dominant resource constraint — not raw TFLOPS." (data-term="kv-cache")
2. **Compute → Memory transition:** "Inference on a single H100: for Llama 3.1 70B, ~85% of time is spent waiting for weight transfers from HBM — not computing. Adding TFLOPS alone doesn't help."
3. **Interconnect (reframed):** "NVLink 5.0: 1,800 GB/s. PCIe 5.0: 64 GB/s. 28× gap for multi-GPU jobs. The interconnect choice determines whether distributed inference scales or stalls — not a new constraint, but increasingly decisive." (data-term="sunk")

**Bottom foldable:**
```html
<details class="fold-detail">
  <summary>↓ Why this matters for GPU provider selection</summary>
  [Note: providers advertising raw TFLOPS are often misleading. Effective throughput for LLM inference is memory-bandwidth-limited.
   Key question to ask: what is the HBM bandwidth per GPU? What is the NVLink/NVMe topology?
   MIG on Blackwell 6000 preserves HBM bandwidth allocation per instance — crucial for inference quality.]
</details>
```

---

### [DIVIDER] — Sphere II: Software / Stack `[INHERITED FROM V1]`

---

### ★ UPDATED — S9 — Orchestration as the Moat — Or Is It?

**Chrome:** Sphere II · Software · The Orchestration Question  
**Act badge:** `SOFTWARE`

**CHANGE from v1 title "Orchestration as the Moat: SUNK vs Soperator":**

Add explicit framing of the moat tension. The slide opens the question — doesn't resolve it. Resolution comes on S11.

**New top context (2 lines, UPDATED):**
> "Bare metal is the table stake. The moat *was* in the orchestration layer — SUNK's 96% goodput, topology-aware gang scheduling. But Soperator's open-source release changed the equation. And agentic coding tools are lowering the cost of writing infrastructure software faster than any prior technology cycle."

**NEW — Scale/moat tension callout (below comparison table):**
Render as a visual scale/balance SVG — two sides:

```
LEFT SIDE — "Software moat arguments"          RIGHT SIDE — "Bare-metal moat arguments"
- Orchestration complexity is real             - Physical infrastructure cannot be open-sourced
- Goodput gap (80% → 96%) is commercial       - Predictive failure detection = operational knowledge
- Inference serving (FlashAttention, vLLM)    - Hardware-software co-opt (CAIOS, BlueField-3)
- Developer experience (DX) compounds          - Energy access, location strategy
                                               - 96% goodput at 10,000-GPU scale ≠ software recipe
```

Scale visual: slightly tilted toward "bare-metal moat" side — suggesting the balance is shifting, not yet decided.

**Caption under scale:** "Soperator is open-source. AI coding tools write K8s manifests in minutes. The question is no longer whether you can build the orchestration — it's whether you can *run* it at 96% goodput, and whether that gap persists as coding tools accelerate."

**SUNK vs Soperator table:** Inherit from v1.  
**Goodput bar chart:** Inherit from v1.

---

### S10 — The Inference Efficiency Race `[INHERITED FROM V1]`

Minor update: add Managed Agents (Anthropic) to the MIG card or as a 5th card:
> "Anthropic Managed Agents (April 2026): decouples the 'brain' (Claude + tools) from the 'hands' (sandbox containers) — provisioned on-demand only when compute is needed. p50 latency −60%, p95 −90%. New workload shape: sustained sessions vs. burst requests." (data-term="managed-agents")

---

### ★ UPDATED — S11 — Barrier to Code: What AI Erodes (and What It Doesn't)

**Chrome:** Sphere II · Software · The Durable Moat  
**Act badge:** `SOFTWARE`

**CHANGE:** More explicit split of what erodes vs. what doesn't, with tooltips on key terms.

**Left column (What AI coding tools lower the barrier to) — inherit from v1.**

**Right column (What it does NOT erode) — UPDATED:**
- Hardware-software co-optimisation (CAIOS, BlueField-3 DPU offload) — (data-term="sunk")
- Proprietary inference kernels (Together AI's FlashAttention took expert GPU researchers — not vibe-coded)
- **96% goodput (data-term="mfu") at 10,000-GPU scale** — this is operational knowledge accumulated over years of failure data, not a software recipe
- Predictive failure detection ("Mission Control") — depends on telemetry and hardware failure models, not just code
- Energy access and location strategy — fully physical, zero code component
- **"The moat that remains is not what you can write — it's what you know and what you own."**

**NEW — ICN angle:** "For ICN, the right question is: which layer do we occupy where coding tools don't reach? Answer: (1) the data layer — IC S3 creates structural lock-in no code can replicate; (2) the physical hardware — Blackwell 6000 MIG (data-term="mig") provisioned correctly outperforms virtualised clouds regardless of orchestration software."

---

### ★ MAJOR UPDATE — S12 — "Context is Gold": Storage as Strategic Infrastructure

**Chrome:** Sphere II · Software · Context is Gold  
**Act badge:** `SOFTWARE`

**RENAMED from "Storage: The Silent Infrastructure Layer"**

**Thesis of the slide:**
> "Storage is not the quiet back-end of AI infrastructure. It is the layer where enterprise value compounds. The context accumulated in storage — conversation history, RAG indices, fine-tuned weights, agent memory, model artefacts — is what makes AI useful for that specific enterprise. It is not easily moved. It grows over time. And IC S3 at $7/TB with zero egress is positioned to be where that context lives."

**Layout: 3-zone design**

**Zone 1 — Storage TCO bar chart (left 35%, SVG) — inherit from v1 + minor label update:**
- AWS S3: ~$23K/mo at 1PB
- CoreWeave: ~$30K/mo
- GCP: ~$20K/mo
- IC S3: **$7K/mo** (highlighted icn green)
- Label update: "4.3× cheaper than CoreWeave · $23K/mo savings at 1 PB · **$276K/year**"

**Zone 2 — "What needs storing" cards (top-right 3×2 grid) — UPDATED with agentic metrics:**

| Context type | Size estimate | Notes |
|-------------|--------------|-------|
| Conversation history | ~2–5 MB / user / month (text) | Grows linearly with usage |
| RAG index (embeddings) | ~1.5 GB per 1M documents | Re-embedding needed on model switch |
| Training checkpoints (70B QLoRA) | ~34–140 GB per run | Saved every 10–30 min |
| Agent session event logs | ~10–50 MB / session | Durable, append-only |
| Fine-tuned weights (LoRA adapters) | ~2–8 GB per adapter | Per use case / per model version |
| Model artefacts (base + adapters) | 140 GB (70B BF16) | Stored once, served many times |

*Enterprise scale (100 active AI users, 1 year):*
- Conversation history: ~6 GB/year (text-only)
- RAG corpus (1M company documents): ~1.5 GB vectors + ~500 GB source docs
- 10 fine-tuning runs/year: ~500 GB–1 TB checkpoint data
- **Total reasonable estimate: 2–5 TB/year per 100 AI users, growing with agentic adoption**

*At 10 TB/year:* IC = $840/year storage. CoreWeave = $3,600/year. AWS = $2,760/year.

**Zone 3 — SwitchingOps panel (bottom, full width) — NEW:**
```
[amber callout bar] — "Context is Gold" moment: When an enterprise switches models
```
> "Re-embedding a 1M-document RAG corpus for a new model: ~$200–500 in compute, ~1–2 days of processing. If the data lives on IC, you run the re-embedding job *here* — no data movement, no egress, no delay. SwitchingOps (data-term="switching-ops") becomes a compute event, not a migration event."

**Three SwitchingOps context-portability cards:**
1. **RAG re-indexing:** "When moving to a new embedding model, all vector indices must be rebuilt. At 1M docs, ~2 days + ~$300 compute. Data on IC = re-embed in place."
2. **Fine-tune porting:** "LoRA adapters trained on one base model cannot be ported to a different architecture. But the training *data* is reusable. IC stores the dataset — the new fine-tune run stays local."
3. **Conversation context migration:** "Agent session history is model-agnostic (text). Switching models doesn't invalidate history — but it does require re-processing for RAG retrieval. Data gravity means this stays on IC."

**Bottom callout (icn green):**
> "'Bring compute to your data, not data to compute.' IC's existing 1PB customer (currently on CoreWeave) is the first land-and-expand target — they've already built the context gravity."

**Foldable detail:**
```html
<details>
  <summary>↓ Context gravity: how lock-in compounds over time</summary>
  [12-month trajectory: month 1 = small dataset, easy to move. Month 12 = TB of history, re-indexed RAG, 
   trained adapters, months of conversation context. Exit cost at month 12 = weeks of reprocessing + 
   data transfer + re-embedding compute. IC's data gravity moat deepens with every month of usage.]
</details>
```

---

### [DIVIDER] — Sphere III: AI / Models `[INHERITED FROM V1]`

---

### ★ UPDATED — S13 — Open-Weight Meets Frontier: The Model Landscape

**Chrome:** Sphere III · AI/Models · The Model Landscape  
**Act badge:** `AI/MODELS`

**RENAMED from "Open-Weight Frontier Compression"**

**Change:** Add frontier labs (proprietary) as context — the open-weight story exists because of frontier lab investment. This slide covers both.

**New two-column layout:**

**Left column — Frontier/Proprietary (pushing the envelope):**
- Anthropic Claude: Opus 4.6 ($15/M, ~1450 ELO), Managed Agents (April 2026 — data-term="managed-agents")
- OpenAI GPT-5 class: frontier training at scale, reinforcement learning
- Google Gemini 3.1 Pro: multimodal frontier, Vertex AI integration
- Key insight: "Frontier labs set the ceiling — open weights chase it with 6–12 month lag, at 10–100× lower inference cost"

**Right column — Open-Weight compression (inherit from v1 + update):**
- Scatter chart (ELO vs $/M tokens) — inherit from v1, update with latest models
- Model cards: Gemma 4 31B, GLM-5.1, DeepSeek-R1

**Managed Agents callout (bottom, new):**
> "Anthropic Managed Agents (April 2026): fully hosted agent infrastructure — developers go from prototype to production in days vs. months. Session runtime at $0.08/session-hour. Signals: agent workloads are becoming infrastructure-grade, not research experiments." (data-term="managed-agents")

**Bottom Jevons marker:** inherit from v1.

---

### S14 — The Agentic Shift `[INHERITED FROM V1]`

Minor update to infrastructure table: add "Managed agent platform" row:

| Dimension | Traditional Inference | Agentic Inference | Managed Agent Platform |
|---|---|---|---|
| Session duration | Seconds | Up to 8 hours | Hours (durable event log) |
| State management | Stateless | Application-managed | Platform-managed |
| Container lifecycle | Per-request | Long-running | On-demand (brain ≠ hands) |

Add footnote (data-term="managed-agents") to platform row.

---

### S15 — Fine-Tuning Proliferation `[INHERITED FROM V1]`
No changes.

---

### ★ NEW — S15b — Where the Compute is Going: The Edge Distribution

**Chrome:** Sphere III · AI/Models · Compute Distribution  
**Act badge:** `AI/MODELS`

**Purpose:** Answer the question: is compute centralising (more neoclouds) or distributing (edge/on-device)? Answer: both — but for different workload types.

**Main visual (centre, SVG spectrum diagram):**

```
ON-DEVICE / EDGE          CLOUD INFERENCE          NEOCLOUD / HYPERSCALER
       ◄─────────────────────────────────────────────────────►
   Light / fast              Mid-tier                 Heavy / complex

Gemma 4 E2B on iPhone    Gemma 4 26B A4B API      GLM-5.1 754B MoE
  ~40 tok/s on MLX         $0.03/M tokens           Frontier reasoning
  No cloud dependency       SaaS / webapp            8-hr agent sessions
  Personal assistants       SME fine-tune inference  Foundation model training
  Always-on agents          RAG + retrieval          Large batch processing
```

**Three zone cards:**

**Left — Edge / On-Device (llm amber):**
- Gemma 4 E2B: ~2.3B params, runs at ~40 tok/s on iPhone 17 Pro (MLX)
- Qualcomm Snapdragon: 45+ TOPS NPU for on-device inference
- Apple Neural Engine: 38 TOPS (M4 chip) — Siri 2.0 class
- **Jevons angle:** On-device capability → more AI tasks attempted → more cloud requests triggered (coordination, heavier tasks, context sync)
- **Storage implication:** On-device models still need cloud sync for context, history, personalisation data → IC S3

**Center — Cloud Inference Mid-Tier:**
- The majority of enterprise AI workloads today
- Gemma 4 26B A4B, Claude Sonnet, GPT-4o-mini
- Served by: Orchestrators (Together AI, Fireworks), hyperscalers (AWS Bedrock, Vertex), ICN
- Price point: $0.01–$5/M tokens

**Right — Neocloud / Hyperscaler Heavy (hw purple):**
- Foundation model training (CoreWeave, Lambda, Nebius)
- Complex reasoning (Claude Opus, GPT-5, GLM-5.1)
- Large agentic jobs (8-hour sessions, 200K–1M context)
- Fine-tuning runs (QLoRA → full fine-tune)
- **Jevons angle:** On-device models generate context and outputs that flow upward → more heavy workloads triggered

**Bottom insight:**
> "The edge distribution does NOT reduce neocloud/cloud demand — it expands it (Jevons). Each on-device model generates context, agent calls, and coordination traffic that flows to cloud. The compute is distributing by *workload type*, while total cloud demand continues growing."

**ICN positioning note (icn green):**
> "ICN's target (ICP-4 fine-tuning SME) sits in the mid-tier → heavy zone. The edge wave creates a new opportunity: as SMEs deploy on-device models for employees, they need a cloud storage layer for context sync and fine-tuning of personalised adapters. IC S3 + ICN compute is the natural backend."

**Foldable — data:**
```html
<details>
  <summary>↓ Edge hardware specs reference</summary>
  [Apple M4 Neural Engine: 38 TOPS | Qualcomm Snapdragon Gen 3: 45+ TOPS | 
   MediaTek Dimensity 9400: 35+ TOPS | Samsung Exynos 2500: 30+ TOPS]
  [Model fit guide: ≤2B params → on-device comfortable | 3–8B → on-device with quantisation | 
   8–26B A4B MoE → cloud inference | 26B+ → cloud required]
</details>
```

---

### [DIVIDER] — The Market Response `[INHERITED FROM V1]`

---

### S16 — The 2×2: Stack Position × Asset Model `[INHERITED FROM V1]`
No changes. ICN PoC position and movement arrows inherit from v1.

---

### S17 — Business Model × Sphere Response Matrix `[INHERITED FROM V1]`
No changes.

---

### S18 — Where Competitors Are Moving and Why `[INHERITED FROM V1]`
No changes.

---

### [DIVIDER] — ICN's Play `[INHERITED FROM V1]`

---

### ★ UPDATED — S19 — ICN Key Resources & Capabilities

**Chrome:** ICN's Play · R&Cs  
**Act badge:** `ICN`

**CHANGE:** Split IC-coupled assets into two categories: (1) training/fine-tuning play and (2) model serving/context play. This surfaces the serving angle more explicitly.

**Left column — ICN standalone:** Inherit from v1.

**Right column — IC-coupled (UPDATED, two sub-sections):**

*Fine-tuning play:*
| Asset | Value |
|-------|-------|
| IC S3 at $7/TB | 4.3× cheaper than CoreWeave — data gravity for training datasets |
| Nitro (high-perf S3) | Eliminates checkpointing idle time — native Axolotl/QLoRA backend |
| 1PB anchor customer | Training data on IC, compute on CoreWeave — first land-and-expand |

*Model serving / context play (NEW):*
| Asset | Value |
|-------|-------|
| IC S3 as context store | RAG indices, conversation history, agent session logs — at $7/TB |
| Zero egress | Re-embedding, SwitchingOps (data-term="switching-ops"), model updates — no data movement cost |
| IC customer base | Enterprise storage customers → natural cross-sell for inference + fine-tune |
| Agentic storage tier | Managed Agents sessions generate durable event logs → IC S3 as long-term agent memory |

**Bottom gaps:** Inherit from v1.

**Updated callout (icn green):**
> "The IC-coupling multiplies ICN's defensibility — in *both* the training play and the serving play. Standalone ICN competes with hundreds of GPU resellers. IC-integrated ICN competes with almost no one on the 'context is gold' dimension."

---

### S20 — Two Avenues `[INHERITED FROM V1]`

Minor update to S21 forward reference: "The Thesis" now covers both training and context serving angles.

---

### ★ UPDATED — S21 — The Thesis

**Chrome:** ICN's Play · The Thesis  
**Act badge:** `ICN`

**CHANGE:** Expand mechanism to cover serving/context play alongside training play.

**Thesis (large Bebas Neue, 40px):**
> "Your training data is already on Impossible Cloud. Fine-tune your models here — and serve them here. Zero egress, 4× cheaper storage, your context never leaves."

**Mechanism (updated 4-step flow, icn green):**
1. IC S3: data gravity → fine-tuning SME stores training data + context on IC
2. ICN Compute: bring compute to the data → Axolotl/QLoRA on Blackwell 6000 + Soperator
3. IC Inference: train on IC, serve on IC → low-latency inference against live context
4. IC Context Store: agent memory, RAG indices, SwitchingOps-ready → context compounds over time, moat deepens

**Flywheel diagram (updated):**
```
Data → Fine-tune → Model → Inference → Agent Output → Context → back to Storage
                                                          ↑
                                               SwitchingOps-ready: model updates
                                               don't require data migration
```

**Three immediate actions:** Inherit from v1.

---

### ★ UPDATED — S22 — Frontier Signals + Export

**Chrome:** Frontier Signals · April 2026  
**Act badge:** `SIGNALS`

**CHANGE:** Add Managed Agents as 5th signal card.

5 signal cards:
1. **Glasswing (Anthropic, Apr 2026):** Frontier model capability as a security event — coordinated defensive disclosure
2. **GLM-5.1 on Ascend:** NVIDIA-free frontier training — geopolitical and infrastructure implications
3. **Gemma 4 on iPhone:** Edge inference wave — Jevons on-device, context sync demand
4. **OpenClaw/NemoClaw (NVIDIA GTC):** Agentic OS layer emerging at infrastructure level
5. **NEW — Managed Agents (Anthropic, Apr 8 2026):** "Get to production 10× faster." Agent workloads go infrastructure-grade — session runtime billed at $0.08/hr. Storage for event logs becomes a standing workload.

PDF export button: inherit from v1.

---

### S24 — [APPENDIX] ICNT / Fiat Decision Framework `[INHERITED FROM V1]`
No changes.

---

## Slide Count

| Type | Count |
|------|-------|
| Content slides (named S-n) | 23 (was 22 — +1 for S15b edge compute) |
| New Venn slide (S-Venn) | 1 |
| Dividers | 5 (unchanged) |
| Appendix (S24) | 1 |
| **Total** | **30** |

---

## New Chart Summary (additions to v1)

| Slide | Chart | Source |
|-------|-------|--------|
| S-Venn | Scatter-to-Venn SVG animation | Hardcoded positions |
| S6 | Household comparison callout bar | EIA data (hardcoded) |
| S8 | Third line added to TFLOPS/bandwidth chart: BW/TFLOPS ratio | Hardcoded from NVIDIA specs |
| S9 | Scale/balance SVG (software vs bare-metal moat) | Hardcoded |
| S12 | SwitchingOps context-portability cards | Hardcoded (research) |
| S13 | Two-column layout (frontier + open-weight) | Hardcoded |
| S15b | Spectrum diagram SVG (edge→cloud→neocloud) | Hardcoded |
| S19 | Two-category R&C table (fine-tune + serving) | Hardcoded |
| S21 | Updated flywheel (4-step with context loop) | Hardcoded |

---

## Tooltip JS lookup table (inline at bottom of HTML)

```js
const TERMS = {
  'jevons-paradox': { title: 'Jevons Paradox', body: 'Efficiency gains increase total resource consumption. Inference cost ↓ 280× in 2 years — GPU demand ↑ 35% CAGR.' },
  'sunk': { title: 'SUNK', body: 'CoreWeave\'s Slurm-on-Kubernetes. ~96% goodput. BlueField-3 DPU offload. Topology-aware gang scheduling.' },
  'soperator': { title: 'Soperator', body: 'Nebius\'s open-source SUNK equivalent. ~92% goodput. Self-hostable. Released 2025.' },
  'mig': { title: 'MIG — Multi-Instance GPU', body: 'Hardware partition: up to 7 isolated GPU instances per card. No virtualisation overhead. ICN PoC uses this for inference.' },
  'mfu': { title: 'MFU — Model FLOP Utilisation', body: '% of theoretical peak FLOPS used for productive computation. CoreWeave SUNK: ~96%. Standard VM cloud: ~80%.' },
  'goodput': { title: 'Goodput', body: 'Fraction of GPU time producing valid computation (excludes failures, retries, network waits). Different from utilisation.' },
  'kv-cache': { title: 'KV Cache', body: 'Stored attention states (Keys + Values) for all prior tokens. Dominant VRAM consumer at scale. 1M token context = ~260 GB for 70B model.' },
  'depin': { title: 'DePIN', body: 'Decentralised Physical Infrastructure Network. Token incentives attract GPU operators. ICN\'s foundational model (ICNT on Base).' },
  'switching-ops': { title: 'SwitchingOps', body: 'Operational discipline for replacing one production LLM with another. Covers prompt regression, cost modelling, rollback, re-embedding RAG indices.' },
  'managed-agents': { title: 'Claude Managed Agents', body: 'Anthropic\'s hosted agent platform (Apr 2026). Decouples brain (Claude + tools) from hands (containers). $0.08/session-hr. Days to production vs. months.' },
  'moe': { title: 'MoE — Mixture of Experts', body: 'Model architecture: total parameters split into "experts"; only a fraction activated per token. Gemma 4 26B A4B: 26B total, 4B active at inference — cost tracks active params.' },
  'ppa': { title: 'PPA — Power Purchase Agreement', body: 'Long-term contract to buy electricity from a specific source. Big Tech contracted 10+ GW of nuclear via PPAs in 2025.' },
  'qloRA': { title: 'QLoRA', body: 'Quantised Low-Rank Adaptation. Fine-tunes a quantised (4-bit) base model + small trainable adapter matrices. 70B model fine-tune: ~46 GB VRAM, $6–20/run.' },
};
```

---

## Spec Self-Review

- **Placeholders:** S15b edge compute slide has one placeholder: `[TO BE ENRICHED WITH AGENT RESEARCH ON EDGE COMPUTE METRICS + AGENTIC STORAGE SIZING]` — agent 4 output pending. All other slides have specific data.
- **Memory & Interconnect reframe:** Title changed to "From Compute-Bound to Memory-Bound" — addresses user's concern that "new bottleneck" was inaccurate. The concept is correct, the framing was wrong.
- **Venn slide:** Centered around 12 confirmed snippets with explicit placement logic. Animation spec is precise enough to implement.
- **SwitchingOps:** Term not publicly indexed as a syv.ai publication — document in terms file with caveat. Concept is real and well-sourced from adjacent literature. User should confirm primary source.
- **Managed Agents:** Confirmed launched April 8, 2026. All details sourced and verified.
- **Scope:** 30 slides total (23 content + 5 dividers + 1 Venn + 1 appendix). Appropriate for 45–60 min session. S-Venn and S15b are optional/skippable in time-pressured sessions.
- **ICN positioning update:** Context/serving angle added throughout S12, S19, S21. Training + serving are now co-primary.
- **Foldable sections:** Used sparingly — S6, S8, S12, S15b. Not on every slide.
- **280× inference price decline:** NOT from GTC 2026 — source from Epoch AI / Artificial Analysis separately. Do not attribute to Jensen Huang.

---

*Spec written by Claude (claude-sonnet-4-6) · 2026-04-13*
*Next step: user review → invoke superpowers:writing-plans*
