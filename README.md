# GPU Industry Intelligence Repo

Owner: Johannes Skifter (ICN / IC)

Purpose: market research, competitive intelligence, and strategic positioning for ICN's GPU compute work. The repo combines presentation decks, dashboards, source notes, and structured data into a browser-first research workspace.

## Start Here

If you want the fastest path through the repo, open [index.html](index.html). It is the launch page and groups the main materials by narrative track.

Featured deep dives:

1. [learning_inference_compute_master.html](learning_inference_compute_master.html) for the most visual inference and memory explainer.
2. [strategy_value_chain_master.html](strategy_value_chain_master.html) for the stack, ownership, and margin map.
3. [strategy_competitor_intelligence.html](strategy_competitor_intelligence.html) for the provider comparison and scoring sheet.

Recommended order:

1. [pres1_gpu_foundations.html](pres1_gpu_foundations.html) for GPU and inference basics.
2. [pres2_gpu-competitive-landscape-deck.html](pres2_gpu-competitive-landscape-deck.html) for the competitive map.
3. [pres3_gpu-trends-positioning-deck.html](pres3_gpu-trends-positioning-deck.html) for market forces and ICN positioning.
4. [strategy_inference_economics.html](strategy_inference_economics.html) and [strategy_competitor_intelligence.html](strategy_competitor_intelligence.html) for deeper analysis.

## What This Repo Is

- Not a software product.
- Not a generic notes dump.
- Actively maintained research material, so treat content as current as of the last commit.

## Core Question

Where is the GPU cloud market heading, who are the key players, how do their business models compare, and where should ICN enter and position?

## Repository Map

### Presentation decks

- [index.html](index.html) — browser hub / launch surface.
- [pres1_gpu_foundations.html](pres1_gpu_foundations.html) — GPU fundamentals, compute stack, hardware generations.
- [pres2_gpu-competitive-landscape-deck.html](pres2_gpu-competitive-landscape-deck.html) — competitive landscape and ClusterMAX-style comparisons.
- [pres3_gpu-trends-positioning-deck.html](pres3_gpu-trends-positioning-deck.html) — market forces, trends, and ICN strategic positioning.

### Strategy dashboards

- [strategy_value_chain_master.html](strategy_value_chain_master.html) — full GPU stack / value chain mapping.
- [strategy_competitor_intelligence.html](strategy_competitor_intelligence.html) — competitor scoring and provider comparisons.
- [strategy_inference_economics.html](strategy_inference_economics.html) — unit economics, utilization, and cost framing.
- [strategy_abstraction_taxonomy.html](strategy_abstraction_taxonomy.html) — abstraction layers and provider categories.

### Learning / explainer pages

- [learning_inference_compute_master.html](learning_inference_compute_master.html) — the deepest inference compute explainer.
- [learning_stack_inference_matrix.html](learning_stack_inference_matrix.html) — stack × workload matrix.
- [learning_prompt_to_answer.html](learning_prompt_to_answer.html) — prompt-to-answer flow and model behavior.

### Standalone materials

- [gpu-compute-intelligence-brief.html](gpu-compute-intelligence-brief.html) — concise shareable brief.
- [gpu_cloud_business_model_comparison.html](gpu_cloud_business_model_comparison.html) — business model comparison.
- [(pres3)inference-compute-deck.html](%28pres3%29inference-compute-deck.html) — earlier pres3 draft, superseded.

### Supporting data and notes

- [data_files/](data_files/) — structured JSON used by the charts and interactive views.
- [manual_notes/](manual_notes/) — primary working notes and source synthesis.
- [gemini_research_outputs/](gemini_research_outputs/) — secondary research outputs and analyses.
- [industry_analyst_charts/](industry_analyst_charts/) — supporting visuals from reports.
- [sources/](sources/) — consolidated research library with structured metadata (reports, interviews, podcasts, academic papers)
- [claude_chat_md_files/](claude_chat_md_files/) — historical chat exports.
- [0ld/](0ld/) — archived superseded files.

## How To Read The Work

The repo is organized around a few recurring questions:

- What do GPUs actually do, and what constrains them?
- Which workloads are driving demand?
- How do providers differ on hardware, orchestration, and business model?
- Where is ICN differentiated by storage, positioning, and economics?

The decks answer those questions visually. The strategy dashboards and notes provide the source trail.

## Presentation Series

| Deck | File | Purpose |
|------|------|---------|
| Pres 1 | [pres1_gpu_foundations.html](pres1_gpu_foundations.html) | GPU fundamentals for a non-technical audience |
| Pres 2 | [pres2_gpu-competitive-landscape-deck.html](pres2_gpu-competitive-landscape-deck.html) | Competitive landscape and provider comparisons |
| Pres 3 | [pres3_gpu-trends-positioning-deck.html](pres3_gpu-trends-positioning-deck.html) | Market forces and ICN positioning |

All three decks are:

- single-file HTML,
- self-contained,
- designed for browser use,
- and exportable to PDF with print.

## Data Freshness

| Source | Coverage |
|--------|----------|
| `manual_notes/trends *.md` | Active hardware, software, and LLM trend tracking |
| `gemini_research_outputs/` | Competitor profiles and business model analysis |
| `data_files/gpu_AI_workloads_2025-2030.json` | Demand model and workload forecasts |
| `industry_analyst_charts/` | Epoch AI, McKinsey, and similar visuals |
| `sources/` | Consolidated research library: reports, interviews, podcasts, analyses (all with metadata) |

## Hosting

GitHub Pages deploys from `main` via [.github/workflows/deploy-pages.yml](.github/workflows/deploy-pages.yml).

Set GitHub Pages source to GitHub Actions, then push to `main`.

For controlled sharing, use access controls such as Cloudflare Access or Azure Static Web Apps with Entra auth.
