# Terms

One file per key term. Each file is a self-contained definition card — used for:
- Deck tooltips (linked via `data-term` attributes in the HTML)
- Model context / agent reading
- Onboarding new team members

## File format

```markdown
---
term: <Term Name>
sphere: hardware | software | ai-models | cross-sphere
source: <original source or who coined it>
tldr: <one sentence definition>
---

## Definition

<Full explanation, 2-5 paragraphs>

## Why it matters for ICN

<Specific relevance to ICN/IC positioning>

## Key sources

- <source 1>
- <source 2>
```

## Index

| Term | Sphere | TL;DR |
|------|--------|-------|
| [SUNK](sunk.md) | software | CoreWeave's Slurm-on-Kubernetes stack — delivers 96% goodput on bare metal |
| [Soperator](soperator.md) | software | Nebius's open-source SUNK equivalent — democratises gang scheduling |
| [MIG](mig.md) | hardware | Multi-Instance GPU — slices one GPU into up to 7 isolated instances |
| [SwitchingOps](switchingops.md) | ai-models | Enterprise process for replacing one LLM with another in production |
| [Managed Agents](managed-agents.md) | ai-models | Anthropic's hosted agent infrastructure — removes session/state management burden |
| [Jevons Paradox](jevons-paradox.md) | cross-sphere | Efficiency gains increase total resource consumption, not decrease it |
| [MFU](mfu.md) | hardware | Model FLOP Utilisation — % of theoretical peak FLOPS actually used in training |
| [KV Cache](kv-cache.md) | software | Key-value cache storing attention states — major VRAM consumer at scale |
| [DePIN](depin.md) | software | Decentralised Physical Infrastructure Network — ICN's token network model |
