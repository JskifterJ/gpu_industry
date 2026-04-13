# Sources

Structured source library for GPU compute market research.

## Folder structure

| Folder | Contents |
|--------|----------|
| `reports/` | Industry reports, analyst notes (SemiAnalysis, Epoch AI, McKinsey, Goldman Sachs, IEA, etc.) |
| `news/` | News articles, press releases, announcements |
| `x-posts/` | X (Twitter) posts — curated quotes from key voices |
| `linkedin/` | LinkedIn posts from industry leaders |
| `interviews/` | Interview transcripts, podcast notes, talk summaries |
| `academic/` | Academic papers, research preprints |
| `podcasts/` | Podcast episode notes (see also legacy: industry_reports_and_sources/) |

## File format

Each source file uses this frontmatter:

```markdown
---
title: <title or description>
author: <name / organisation>
date: YYYY-MM-DD
source_type: report | news | x-post | linkedin | interview | academic | podcast
url: <url if available>
sphere: hardware | software | ai-models | cross-sphere | icn-positioning
tags: [inference, energy, open-weight, orchestration, ...]
relevance: high | medium | low
deck_slide: S3 | S6 | S12 | ...   # which pres3 slide this feeds
---
```

## Sphere mapping

- `hardware` — GPU generations, energy wall, silicon, memory/interconnect
- `software` — Orchestration, inference efficiency, stack, barrier to code
- `ai-models` — Open-weight, frontier labs, agentic workloads, fine-tuning, edge
- `cross-sphere` — Spans two or more spheres (lands in Venn overlap)
- `icn-positioning` — ICN/IC strategy, competitive positioning, ICNT/fiat

## Legacy content

Older research lives in `industry_reports_and_sources/` and `gemini_research_outputs/`. Not reorganised — use as reference. New sources go here.
