# GPU Industry Intelligence Repo

**Owner:** Johannes Skifter (ICN / IC)
**Purpose:** Competitive intelligence, market analysis, and strategic positioning for ICN's GPU compute offering. Covers the full GPU cloud stack: hardware economics, business model comparison, competitor landscape, AI workload trends, and ICN's positioning against the market.

---

## Agent / AI Reading Guide

This section is for AI agents parsing this repo. The repo has several distinct workstreams. Read this guide before deciding which files to open.

### What this repo is NOT
- Not a software product. There is no application to build or deploy.
- Not a generic research dump. Every file has a specific analytical purpose.
- Not complete — it is actively maintained. Treat all data as current as of the file's last commit date.

### Core question this repo answers
> *Where is the GPU cloud market heading, who are the key players, how do their business models compare, and where should ICN enter and position?*

---

## Repository Map

```
gpu_industry/
│
├── index.html                          ← Dashboard hub (entry point for browser)
│
├── PRESENTATIONS (pres*.html)
│   ├── pres1_gpu_foundations.html      ← Deck 1: GPU fundamentals, compute stack, hardware generations
│   ├── pres2_gpu-competitive-landscape-deck.html  ← Deck 2: Competitive landscape, 40+ providers, ClusterMAX ratings
│   └── pres3_gpu-trends-positioning-deck.html     ← Deck 3: Market forces, trends, ICN strategic positioning
│
├── STRATEGY TOOLS (strategy_*.html)
│   ├── strategy_value_chain_master.html       ← Full GPU stack L1–L7 value chain with company mapping
│   ├── strategy_competitor_intelligence.html  ← Competitor scoring dashboard (37 rated, 84 reviewed)
│   ├── strategy_inference_economics.html      ← Unit economics: $/GPU-hr, MFU, margin analysis
│   └── strategy_abstraction_taxonomy.html     ← Taxonomy of abstraction layers and provider categories
│
├── LEARNING TOOLS (learning_*.html)
│   ├── learning_inference_compute_master_v10.html  ← Deep-dive: inference compute (most comprehensive)
│   ├── learning_stack_inference_matrix.html        ← Stack × workload matrix
│   └── learning_prompt_to_answer.html              ← Prompt engineering for GPU/AI research
│
├── STANDALONE TOOLS
│   ├── gpu-compute-intelligence-brief.html    ← Concise intelligence brief (shareable one-pager)
│   ├── gpu_cloud_business_model_comparison.html ← BM comparison (standalone, pre-pres2)
│   └── (pres3)inference-compute-deck.html     ← Earlier pres3 draft (superseded by pres3_gpu-trends-positioning-deck.html)
│
├── data_files/                         ← Structured JSON data powering interactive visualizations
│   ├── gpu_AI_workloads_2025-2030.json      ← McKinsey DC demand model: inference vs training GW by year
│   ├── deck2_differentiator_and_descriptions.json  ← Company descriptions for pres2 competitive cards
│   ├── market_positioning.json               ← Company positions for BM trajectory scatter (pres3 S8)
│   └── gpu_economics_canonical_spec_v1.json  ← GPU hardware specs and economics (canonical reference)
│
├── manual_notes/                       ← Human-authored analytical notes (highest-signal content)
│   ├── trends (hardware, software, LLM usage_development).md  ← Running trends log (actively updated)
│   ├── notes.md                        ← General working notes
│   ├── NVIDIA_GTC_2026_notes.md        ← GTC 2026 keynote notes (April 2026)
│   ├── SUNK_&_Soperator_notes.md       ← CoreWeave SUNK / Nebius Soperator analysis
│   ├── web3_competitors_notes.md       ← Decentralized / web3 GPU compute players
│   └── interview_notes/
│       └── Darran __ Johannes (GPU __ IC product) - 2026_03_13 12_00 CET - Notes by Gemini.md
│           ← Key interview: IC storage × ICN GPU strategy (Darran Soothill × Johannes Skifter, 2026-03-13)
│
├── gemini_research_outputs/            ← Gemini Deep Research outputs (treat as secondary sources)
│   ├── GPU_architectures_workloads_ICPs.md   ← GPU gen specs, workload fit, ICP analysis
│   ├── GPU_cloud_bm_trajectories.md          ← Business model trajectory analysis (source for pres3 S8)
│   ├── GPU Compute Competitor Analysis.md    ← Broad competitor analysis
│   ├── ICN_GPU_Competitor_landscape.md       ← ICN-specific competitor framing
│   ├── competitor_analysis.md                ← Detailed competitor profiles
│   ├── competitor_analysis_bronze_and_unranked.md  ← Bronze/unranked tier providers
│   ├── competitor_descriptions_(clustermax)_use_for_input.md  ← ClusterMAX descriptions (pres2 input)
│   └── GPU Compute Business Model Comparison Framework (1).md  ← BM framework (early draft)
│
├── industry_analyst_charts/            ← Charts from Epoch AI, McKinsey, SemiAnalysis etc.
│   ├── MCK251181-AI-workload-placement-strategy-v4_Exh1.png  ← McKinsey inference vs training chart
│   ├── training-compute-trend.png      ← Epoch AI: training compute 5× per year
│   ├── flop-s-per-dollar-trend.png     ← FLOP/$ trend (Epoch AI)
│   ├── llm-inference-price-trends.png  ← Inference cost halving every ~2 months
│   └── Scaling of peak HW FLOPS and interconnect bandwidth.jpg
│
├── industry_reports_and_sources/       ← PDFs and structured notes from analyst reports
│   ├── ClusterMAX2.0/                  ← SemiAnalysis ClusterMAX 2.0 report materials
│   ├── epoch.ai_can_scaling_continue_through_2030/
│   ├── epoch.ai_Global AI computing capacity is doubling every 7 months/
│   └── podcast_notes_ai_eng_gpu_industry.json  ← Structured podcast notes
│
├── claude_chat_md_files/               ← Exported Claude conversation logs (historical reference)
│   └── claude_gpu_foundations_chat_history.md
│
└── 0ld/                                ← Archived/superseded files (do not use as current source)
    ├── icn-ai-presentation.html        ← Early ICN brand deck (superseded)
    ├── gpu_deck_1_compute_foundations.html  ← pres1 predecessor
    └── learning_inference_compute_master.html  ← v9 of learning master
```

---

## Workstream Overview

### 1. Competitive Intelligence
**Goal:** Score and rank GPU cloud providers across 10 criteria.
**Key files:**
- `strategy_competitor_intelligence.html` — live scoring dashboard
- `gemini_research_outputs/competitor_analysis.md` — profiles
- `data_files/deck2_differentiator_and_descriptions.json` — structured company data
- `pres2_gpu-competitive-landscape-deck.html` — presentation layer (ClusterMAX framework)

### 2. Business Model Analysis
**Goal:** Map how different provider archetypes (Hyperscaler / Neocloud / Orchestrator / Aggregator) compete, where moats are forming, and how each responds to market forces.
**Key files:**
- `strategy_value_chain_master.html` — L1–L7 stack ownership map
- `gemini_research_outputs/GPU_cloud_bm_trajectories.md` — trajectory analysis
- `data_files/market_positioning.json` — company positions
- `pres3_gpu-trends-positioning-deck.html` S8–S11 — BM trajectory scatter, ICP matrix, BM×Forces heatmap

### 3. Market Trends & Forces
**Goal:** Identify and quantify the structural forces reshaping the GPU compute market (energy, inference growth, GPU generation cycles, agentic demand, etc.).
**Key files:**
- `manual_notes/trends (hardware, software, LLM usage_development).md` — primary running log
- `manual_notes/NVIDIA_GTC_2026_notes.md` — latest NVIDIA strategy
- `data_files/gpu_AI_workloads_2025-2030.json` — McKinsey demand model
- `pres3_gpu-trends-positioning-deck.html` S1–S6 — Nine Forces framework + visualizations

### 4. ICN Strategic Positioning
**Goal:** Define ICN's entry strategy into GPU compute, leveraging IC's storage moat.
**Key files:**
- `manual_notes/interview_notes/Darran __ Johannes ... Notes by Gemini.md` — founding interview
- `pres3_gpu-trends-positioning-deck.html` S12–S14 — ICN's Assets, Scenarios, Thesis
- `manual_notes/SUNK_&_Soperator_notes.md` — reference cases (CoreWeave, Nebius)

### 5. Hardware & Economics
**Goal:** Understand GPU generation economics, MFU benchmarks, $/GPU-hr trends, and procurement timing.
**Key files:**
- `data_files/gpu_economics_canonical_spec_v1.json` — canonical specs
- `gemini_research_outputs/GPU_architectures_workloads_ICPs.md` — architecture deep-dive
- `strategy_inference_economics.html` — unit economics
- `pres1_gpu_foundations.html` — hardware foundations presentation

---

## Key Analytical Frameworks

| Framework | Location | Description |
|-----------|----------|-------------|
| ClusterMAX scoring | pres2 + `strategy_competitor_intelligence.html` | 10-category, 50pt provider scoring |
| Value chain L1–L7 | `strategy_value_chain_master.html` + pres2 S3 | Stack ownership map |
| BM archetypes (H/N/O/A) | pres2 + pres3 S9–S10 | Hyperscaler / Neocloud / Orchestrator / Aggregator |
| Nine Forces grid | pres3 S3 | Nine structural forces driving market change |
| BM × Forces heatmap | pres3 S10 | How each BM archetype responds to each force |
| Jevons Frame | pres3 S2 | Foundational lens: efficiency → more consumption |
| ICN Scenarios A/B/C | pres3 S13 | Clean Slate / Storage Synergy / Full Stack |

---

## Presentation Series

| Deck | File | Slides | Purpose |
|------|------|--------|---------|
| Pres 1 | `pres1_gpu_foundations.html` | 13 | GPU fundamentals for non-technical audience |
| Pres 2 | `pres2_gpu-competitive-landscape-deck.html` | 18 | Competitive landscape deep-dive |
| Pres 3 | `pres3_gpu-trends-positioning-deck.html` | 14+1 | Market forces + ICN positioning |

All three decks:
- Single-file HTML (fully self-contained, no server needed)
- Dark mode default, light mode toggle
- PDF export button on final slide (`window.print()`)
- ClusterMAX design system (Bebas Neue + Barlow + JetBrains Mono)

---

## Data Freshness

| Source | Last updated | Coverage |
|--------|-------------|---------|
| `manual_notes/trends *.md` | Active (Apr 2026) | Hardware, software, LLM trends |
| `gemini_research_outputs/` | Mar 2026 | Competitor profiles, BM analysis |
| `data_files/gpu_AI_workloads_2025-2030.json` | Mar 2026 | McKinsey 2025–2030 demand model |
| `industry_analyst_charts/` | Mar 2026 | Epoch AI, McKinsey charts |
| `industry_reports_and_sources/` | Mar 2026 | ClusterMAX 2.0, Epoch AI reports |

---

## Hosting

GitHub Pages deploys from `main` branch via `.github/workflows/deploy-pages.yml`.

Setup: **Settings → Pages → Source: GitHub Actions** → push to `main`.

> Note: GitHub Pages on a public repo is public-by-link. For access-controlled sharing, use Cloudflare Access or Azure Static Web Apps with Entra auth.
