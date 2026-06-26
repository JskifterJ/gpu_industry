# Market Intelligence Check-In System -- Technical Specification

**Date:** 2026-06-26
**Purpose:** Structured, recurring market intelligence pipeline that keeps the Strategic Fitness Scoring Tool's evidence base current and surfaces emerging signals before they crystallize into formal trend updates.
**Companion to:** [Strategic Fitness Scoring Tool Spec](2026-06-21-strategic-fitness-scoring-tool-design.md) (2026-06-21)
**Thesis context:** EPFL Master's thesis on GPU cloud infrastructure strategy. The scoring tool scores 7 archetypes against 18 trends; this system provides the operational workflow that feeds it.

---

## 1. System Overview

### 1.1 What the system does

A market intelligence analyst (or AI agent acting on their behalf) runs a structured check-in at a defined cadence. Each check-in scans a tiered source list, synthesizes findings through three analytical lenses (supply, demand, market structure), assesses impact on each of the 7 archetypes, and produces a structured output document. That output feeds directly into the scoring tool's Source Registry (Tab 12, Scoring Tool Spec ss3.12), Decision Journal (Tab 10, ss3.10), and Expansion Guide review cycles (Tab 12, ss3.13).

### 1.2 Relationship to the Scoring Tool

The scoring tool is the analytical framework. This system is the intelligence pipeline that keeps it current.

```
Market Intelligence Check-In System          Scoring Tool
====================================         ============

Tier 1/2/3 Sources                           Source Registry (ss3.12)
       |                                            ^
  Scan + Extract                                    |
       |                                     Citation keys
       v                                            |
Check-In Output Document ------->  Evidence updates + EQ upgrades
       |                                            |
       |                           Scoring Matrix cell updates (ss3.3)
       |                                            |
       v                                            v
Scoring Recommendations -------->  Decision Journal entries (ss3.10)
       |                                            |
       v                                            v
Source Health Report ----------->  Expansion Guide review (ss3.13)
```

The scoring tool's Expansion Guide (ss3.13) specifies 6-month and 12-month review cycles. This system operates at higher frequency (monthly/quarterly) to prevent evidence staleness between those formal reviews.

### 1.3 Scope boundaries

- This system monitors and synthesizes. It does not make scoring decisions autonomously.
- Scoring matrix cell changes require human approval. The system recommends changes with justification; a human analyst confirms or rejects.
- The system does not replace the inter-rater reliability protocol (Scoring Tool Spec ss3.11.1). Multi-rater scoring validation remains a separate process.

---

## 2. Multi-Perspective Synthesis Framework

Each check-in analyzes the market through three analytical lenses plus a per-archetype impact assessment.

### 2.1 Analytical lenses

| Lens | Scope | Trends covered | Key questions |
|------|-------|----------------|---------------|
| **Supply-side** | Silicon cycles, energy constraints, infrastructure buildout, neocloud financials | S-1 through S-6 | What changed in GPU supply, pricing, energy access, or neocloud capital structures? |
| **Demand-side** | Inference vs training mix, enterprise adoption, agentic workloads, edge compute | D-1 through D-6 | What changed in workload composition, adoption rates, or demand economics? |
| **Market structure** | Orchestration, pricing, ASICs, security, financialization | M-1 through M-6, R-1 | What changed in how the market is organized, priced, or regulated? |

### 2.2 Per-archetype impact assessment

After synthesizing through the three lenses, the analyst assesses what changed for each archetype since the last check-in.

**The 7 archetypes:**

| ID | Archetype | One-line definition |
|----|-----------|---------------------|
| A1 | AI Factory | Bare-metal GPU clusters optimized for large-scale training (CoreWeave model) |
| A2 | Full-Stack Neocloud | Vertically integrated L1-L5 provider with proprietary inference/storage (Nebius model) |
| A3 | Orchestrator/PaaS | Multi-cloud abstraction and developer platform (Together AI model) |
| A4 | Aggregator/Clearing House | Supply-side aggregation and marketplace (Vast.ai model) |
| A5 | Compute Financier | GPU-backed financial instruments and structured finance (IREN model) |
| A6 | Energy-First Provider | Power procurement as primary competitive advantage (Crusoe model) |
| A7 | Infrastructure Enabler | Specialized tooling, monitoring, and integration services |

**Impact assessment template per archetype:**

```
Archetype: [name]
Net impact since last check-in: Improved / Unchanged / Deteriorated
Key changes:
  - [Trend ID]: [1-sentence description of what changed and why it matters]
Scoring recommendations:
  - [Cell reference]: [current value] -> [proposed value], because [rationale]
```

### 2.3 The 18+1 trends

For reference, the full trend set monitored by this system:

**Supply-Side:**
- S-1: Energy wall
- S-2: Silicon supply cycles
- S-3: GPU lifecycle and secondary markets
- S-4: CPU-GPU rebalancing
- S-5: KV cache and context infrastructure
- S-6: Neocloud financial bifurcation

**Demand-Side:**
- D-1: Inference dominance
- D-2: Agentic and robotics workloads
- D-3: Enterprise ML proliferation
- D-4: Open-weight economics
- D-5: Edge distribution
- D-6: Hardware Jevons paradox

**Market Structure:**
- M-1: Orchestration commoditization
- M-2: Inference market stratification
- M-3: ASIC diversification
- M-4: Multi-cloud abstraction
- M-5: Supply-chain security
- M-6: Financialization

**Candidate (R-series):**
- R-1: Regulatory and geopolitical fragmentation (under evaluation per Scoring Tool Spec ss8.2)

---

## 3. Tiered Source System

### 3.1 Design principles

Sources are organized in three tiers reflecting scan frequency and reliability. The system distinguishes between sources that analysts check proactively (Tier 1/2) and sources that analysts discover reactively by following leads (Tier 3). Tier 3 provides structured freedom to chase references, citations, and links found in core sources without requiring pre-approval.

### 3.2 Tier definitions

| Tier | Name | Scan frequency | Description |
|------|------|----------------|-------------|
| **Tier 1** | Core | Every monthly check-in | Baseline intelligence feeds. Consistently high signal-to-noise. Skipping these creates evidence gaps. |
| **Tier 2** | Rotating | Quarterly or when signals emerge | Valuable but less frequent or situational. Checked when relevant signals from Tier 1 suggest changes. |
| **Tier 3** | Discovery | Ad-hoc, lead-driven | Found by following references, citations, and links from Tier 1/2 content. No pre-defined list. |

### 3.3 Tier 1: Core Sources

Pre-populated from the research evidence files and known high-value sources.

| Source ID | Source | Type | Primary trends | URL / Access |
|-----------|--------|------|----------------|--------------|
| T1-SA | SemiAnalysis Newsletter | Newsletter | All (S-1, S-6, M-3 strongest) | newsletter.semianalysis.com |
| T1-SA-CM | SemiAnalysis ClusterMAX Reports | Report | All (benchmarking baseline) | newsletter.semianalysis.com |
| T1-HH | Hydra Host (Ariel Deschapell) | Newsletter/Analysis | S-6, M-1, M-2, S-3 | hydrahost.substack.com |
| T1-CW-SEC | CoreWeave SEC Filings (10-K, 10-Q) | Filing | S-6, M-6, D-1 | SEC EDGAR |
| T1-NEB | Nebius Earnings + Press Releases | Filing/PR | S-6, D-1, M-1 | ir.nebius.com |
| T1-NVDA-ER | NVIDIA Earnings Calls | Filing | S-2, M-3, D-1 | investor.nvidia.com |
| T1-NVDA-GTC | NVIDIA GTC Keynotes | Event | S-2, S-4, D-2 | nvidia.com/gtc |
| T1-EPO | Epoch AI Compute Reports | Report | D-6, D-1, S-2 | epochai.org |
| T1-BB | Ben Baldieri "The GPU" Newsletter | Newsletter | S-1, S-6, M-3, M-6 | thecompute.ai / email |
| T1-AINEWS | AINews / swyx (Latent.Space) | Newsletter | D-4, D-2, M-2 | buttondown.com/ainews |
| T1-GS | Goldman Sachs AI Infrastructure Reports | Report | S-1, D-6, M-6 | gs.com (paywall) |
| T1-MS | Morgan Stanley AI/DC Reports | Report | S-1, M-6, S-6 | morganstanley.com (paywall) |
| T1-CW-DC | Cushman and Wakefield DC Market Reports | Report | S-1 | cushmanwakefield.com |
| T1-LAMBDA | Lambda Labs Funding/Strategy Announcements | PR | S-6, S-2 | lambdalabs.com |

### 3.4 Tier 2: Rotating Sources

| Source ID | Source | Type | Primary trends | Rotation trigger |
|-----------|--------|------|----------------|------------------|
| T2-CW-BLOG | CoreWeave Engineering Blog | Blog | M-1, S-5, D-1 | New post published |
| T2-CRUSOE | Crusoe Engineering Blog | Blog | S-1, S-5, L5 | New post on MemoryAlloy or infrastructure |
| T2-SKYPILOT | SkyPilot Releases + Blog | Blog/Release | M-1, M-4 | New version release |
| T2-VLLM | vLLM Releases and Benchmarks | Release | S-5, M-4, D-1 | Major version release |
| T2-ARXIV | arXiv cs.DC / cs.LG / cs.AI | Preprint | D-2, S-5, M-4 | When Tier 1 sources cite new papers |
| T2-HRI | HashRate Index GPU Secondary Market | Report | S-3, S-2 | Quarterly pricing updates |
| T2-SACRA | Sacra Neocloud Revenue Reports | Report | S-6, M-2 | New report published |
| T2-MENLO | Menlo Ventures State of GenAI | Report | D-3, D-1 | Annual release |
| T2-MCKINSEY | McKinsey State of AI | Report | D-3, D-6 | Annual release |
| T2-DELOITTE | Deloitte TMT Predictions / AI Reports | Report | D-6, D-1 | Annual release |
| T2-FERC | FERC Filings (DC interconnection) | Regulatory | S-1 | When energy-related signals emerge |
| T2-ERCOT | ERCOT Large-Load Queue Data | Regulatory | S-1 | Quarterly queue updates |
| T2-SEC-NEO | SEC Filings (other neoclouds: Applied Digital, IREN, Iris Energy) | Filing | S-6, M-6 | Earnings season |
| T2-ORNN | Ornn / ICE Compute Futures Data | Market data | M-6 | New contract announcements or pricing data |
| T2-GROQ | Groq Pricing and Performance | PR/Blog | M-3, M-2 | Pricing changes |
| T2-AMD | AMD MI-series Announcements | PR | M-3 | Product launches |
| T2-UNITREE | Unitree / Robotics Company Filings | Filing | D-2 | Quarterly updates |
| T2-GVR | Grand View Research AI/DC Market Reports | Report | D-5, D-3 | New report published |
| T2-STRAT | Stratechery (Ben Thompson) | Newsletter | Hyperscaler context | When covering AI infrastructure |
| T2-INFER-INV | InferentialInvestor | Newsletter | S-4, D-2 | New posts on CPU-GPU dynamics |
| T2-GETDEPLOY | GetDeploying.com GPU Pricing | Aggregator | S-2 | Monthly pricing updates |

### 3.5 Tier 3: Discovery protocol

Tier 3 has no pre-defined source list. Instead, the analyst follows a structured discovery process:

1. **Reference chasing:** When a Tier 1/2 source cites a primary source the system has not seen, fetch and evaluate the primary source.
2. **Link following:** When a Tier 1/2 source links to a data set, company blog, or regulatory filing with new evidence, read it.
3. **Citation networks:** When an academic paper cited in the evidence base has new citing papers (Google Scholar alerts), check them.
4. **Event-driven discovery:** When a Tier 1 source covers a company or technology not in the source registry, evaluate whether that company/technology warrants ongoing monitoring.

**Discovery logging:** Every Tier 3 source accessed during a check-in is logged in the Discovery Log section of the output document (see ss5.3). Sources that produce usable evidence are candidates for promotion to Tier 2 at the next quarterly review.

### 3.6 Source list format (machine-readable)

The source list is maintained in two formats: the human-readable tables above and a JSON file that AI agents consume directly.

**JSON Schema:**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MarketIntelligenceSourceRegistry",
  "type": "object",
  "properties": {
    "version": { "type": "string" },
    "last_updated": { "type": "string", "format": "date" },
    "sources": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id", "name", "tier", "type", "primary_trends", "status"],
        "properties": {
          "id": {
            "type": "string",
            "description": "Unique source identifier (e.g., T1-SA, T2-CRUSOE)"
          },
          "name": {
            "type": "string",
            "description": "Human-readable source name"
          },
          "tier": {
            "type": "integer",
            "enum": [1, 2, 3],
            "description": "1=Core, 2=Rotating, 3=Discovery"
          },
          "type": {
            "type": "string",
            "enum": ["Newsletter", "Report", "Filing", "Blog", "Release", "Preprint", "Regulatory", "Market_Data", "PR", "Aggregator", "Event", "Interview"]
          },
          "primary_trends": {
            "type": "array",
            "items": { "type": "string" },
            "description": "Trend IDs this source primarily covers (e.g., ['S-1', 'S-6'])"
          },
          "url": {
            "type": "string",
            "format": "uri",
            "description": "Primary URL or access method"
          },
          "access_method": {
            "type": "string",
            "enum": ["public", "paywall", "email", "api", "sec_edgar", "manual"],
            "description": "How to access the source"
          },
          "scan_frequency": {
            "type": "string",
            "enum": ["every_monthly", "quarterly", "event_triggered", "ad_hoc"],
            "description": "How often this source is checked"
          },
          "status": {
            "type": "string",
            "enum": ["active", "under_review", "retired"],
            "description": "Current operational status"
          },
          "last_scanned": {
            "type": "string",
            "format": "date",
            "description": "Date of most recent scan"
          },
          "last_produced_evidence": {
            "type": "string",
            "format": "date",
            "description": "Date this source last yielded a usable data point"
          },
          "notes": {
            "type": "string",
            "description": "Free-text notes (e.g., rotation trigger, access constraints)"
          }
        }
      }
    }
  }
}
```

**Example entry:**

```json
{
  "id": "T1-SA",
  "name": "SemiAnalysis Newsletter",
  "tier": 1,
  "type": "Newsletter",
  "primary_trends": ["S-1", "S-2", "S-6", "M-3", "M-5"],
  "url": "https://newsletter.semianalysis.com",
  "access_method": "email",
  "scan_frequency": "every_monthly",
  "status": "active",
  "last_scanned": "2026-06-22",
  "last_produced_evidence": "2026-06-22",
  "notes": "ClusterMAX reports are the most comprehensive neocloud benchmarking source. Two-tier paywall: free digest + paid deep dives."
}
```

**File location:** `gpu_industry/strategy/tool/configs/intelligence_sources.json`

---

## 4. Check-In Cadence

### 4.1 Three cadence tiers

| Cadence | Scope | Sources scanned | Output depth | Trigger |
|---------|-------|-----------------|--------------|---------|
| **Monthly check-in** | Quick-scan, flag material changes | Tier 1 only | Summary-level (ss5.1 sections A-C) | Calendar: 1st week of each month |
| **Quarterly deep dive** | Full multi-perspective synthesis | Tier 1 + Tier 2 + Tier 3 discovery | Full output document (ss5.1 all sections) | Calendar: January, April, July, October |
| **Ad-hoc alert** | Event-driven rapid assessment | Event-specific sources | Abbreviated (ss5.2) | Triggered by material events |

### 4.2 Monthly check-in protocol

**Time budget:** 2-4 hours (human) or equivalent agent execution time.

**Steps:**

1. Scan all Tier 1 sources for publications since last check-in
2. Extract data points relevant to any of the 18+1 trends
3. Write 1-2 sentence per-trend update (or "No material change")
4. Flag any data points that warrant EQ upgrades or scoring cell changes
5. Note any sources that produced zero useful intelligence (input for source health tracking)
6. Log any Tier 3 discoveries

### 4.3 Quarterly deep dive protocol

**Time budget:** 8-16 hours (human) or equivalent agent execution time.

**Steps:**

1. Complete the full monthly check-in scan (Tier 1)
2. Scan all active Tier 2 sources
3. Follow Tier 3 discovery leads from steps 1-2
4. Write multi-perspective synthesis (supply, demand, market structure)
5. Complete per-archetype impact assessment for all 7 archetypes
6. Produce scoring recommendations with cell references and justification
7. Run quarterly source rotation review (ss6)
8. Update source health metrics
9. Produce the full output document (ss5.1)

### 4.4 Ad-hoc alert triggers

The following events warrant an unscheduled check-in:

| Category | Example events |
|----------|----------------|
| **Major acquisition or funding** | Neocloud acquisition >$1B, funding round >$500M, hyperscaler infrastructure deal |
| **GPU product launch** | New NVIDIA generation (Rubin GA), major ASIC launch (Trainium3, TPU next-gen) |
| **Policy or regulatory change** | Export controls (Fable/Mythos ban), grid regulation (FERC orders), AI regulation (EU AI Act enforcement) |
| **Market structure shift** | Neocloud bankruptcy or exit, major pricing change (>30%), new financial instrument (compute futures listing) |
| **Scoring tool trigger** | Any event that would change 3+ cells in the scoring matrix by 1+ points |

Ad-hoc alerts produce an abbreviated output (ss5.2) and can be promoted to a full quarterly deep dive if the event's implications are broad enough.

---

## 5. Output Format

### 5.1 Full output document structure (quarterly)

Every quarterly check-in produces a Markdown document at:
`gpu_industry/research/check_ins/YYYY-QN-check-in.md`

**Document structure:**

```
# Market Intelligence Check-In -- YYYY-QN

Date: YYYY-MM-DD
Analyst: [name]
Type: Monthly / Quarterly / Ad-hoc
Period covered: [start date] to [end date]

---

## A. Executive Summary

[3-5 bullet points: what changed, what matters, what to watch]

## B. Per-Trend Evidence Updates

### Supply-Side

#### S-1 Energy Wall
- **Status:** New evidence / No change / Trend strengthening / Trend weakening
- **New data points:**
  - [DP-1]: [claim]. Source: [source ID]. EQ: [1-3].
  - [DP-2]: ...
- **EQ change:** [current] -> [proposed], because [rationale]
- **Scoring tool citation key:** [new key to add to Source Registry]

[Repeat for S-2 through S-6]

### Demand-Side

[Repeat pattern for D-1 through D-6]

### Market Structure

[Repeat pattern for M-1 through M-6, R-1]

## C. Archetype Impact Assessment

### A1 AI Factory
- **Net impact:** Improved / Unchanged / Deteriorated
- **Key changes:**
  - [Trend ID]: [1-sentence impact description]
- **Scoring recommendations:**
  - Cell [A1 x S-1 Exp]: [current] -> [proposed], because [rationale]

[Repeat for A2 through A7]

## D. Scoring Matrix Update Recommendations

### Recommended cell changes

| Archetype | Trend | Dimension | Current | Proposed | Rationale | Evidence source |
|-----------|-------|-----------|---------|----------|-----------|-----------------|
| A1 | S-1 | EQ | 2 | 3 | New Goldman Sachs report corroborates energy constraint data | T1-GS |

### Recommended trend-level changes

| Trend | Change type | Detail | Rationale |
|-------|-------------|--------|-----------|
| S-1 | P(Consensus) update | 0.90 -> 0.92 | Additional grid constraint data from SemiAnalysis |
| R-1 | Status change | Under Review -> Active | Fable/Mythos export ban provides discriminating evidence |

### Full re-scoring trigger assessment

- [ ] 3+ cells recommended for change -> consider targeted re-score of affected archetypes
- [ ] New trend proposed -> requires full scoring for all 7 archetypes
- [ ] Trend retirement proposed -> preserve history, zero exposure (per Scoring Tool ss3.13)

## E. Source Health Report

### Source scan results

| Source ID | Issues scanned | Data points extracted | Yield rating |
|-----------|----------------|----------------------|--------------|
| T1-SA | 3 | 11 | High |
| T1-HH | 2 | 4 | Medium |
| T1-CW-SEC | 0 (no new filing) | 0 | N/A (between cycles) |

### Source health flags

- [Source ID]: [concern, e.g., "No new publications in 3 months", "Quality decline", "Paywall access lost"]

## F. Discovery Log

### New sources found this cycle

| Source | Found via | Trend relevance | Tier recommendation |
|--------|----------|-----------------|---------------------|
| [name] | [Tier 1/2 source that led to it] | [trend IDs] | Tier 2 / Watch / Discard |

### Reference threads followed

- [Source] -> cited [paper/report] -> [useful / not useful]

## G. Decision Journal Entries

[Pre-formatted rows for the Scoring Tool's Decision Journal (Tab 10, ss3.10)]

| Date | Analyst | Tab | Cell | Trend | Archetype | Dimension | Old | New | Rationale | Source |
|------|---------|-----|------|-------|-----------|-----------|-----|-----|-----------|--------|
| YYYY-MM-DD | [name] | Scoring Matrix | [ref] | S-1 | A1 | EQ | 2 | 3 | [rationale] | T1-GS |
```

### 5.2 Abbreviated output (monthly and ad-hoc)

Monthly check-ins produce sections A, B, and C only, plus section F (Discovery Log).

Ad-hoc alerts produce:
- Section A (Executive Summary, focused on the triggering event)
- Relevant trend subsections from B
- Affected archetype subsections from C
- Section D (Scoring recommendations for affected cells only)

### 5.3 Machine-readable output

In addition to the Markdown document, each check-in produces a JSON sidecar at:
`gpu_industry/research/check_ins/YYYY-QN-check-in.json`

**Schema:**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CheckInOutput",
  "type": "object",
  "required": ["metadata", "trend_updates", "archetype_impacts", "scoring_recommendations"],
  "properties": {
    "metadata": {
      "type": "object",
      "properties": {
        "date": { "type": "string", "format": "date" },
        "analyst": { "type": "string" },
        "type": { "type": "string", "enum": ["monthly", "quarterly", "ad_hoc"] },
        "period_start": { "type": "string", "format": "date" },
        "period_end": { "type": "string", "format": "date" }
      }
    },
    "trend_updates": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "trend_id": { "type": "string" },
          "status": { "type": "string", "enum": ["new_evidence", "no_change", "strengthening", "weakening"] },
          "data_points": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "id": { "type": "string" },
                "claim": { "type": "string" },
                "source_id": { "type": "string" },
                "evidence_quality": { "type": "integer", "minimum": 1, "maximum": 3 },
                "date_observed": { "type": "string", "format": "date" }
              }
            }
          },
          "eq_change": {
            "type": "object",
            "properties": {
              "current": { "type": "integer" },
              "proposed": { "type": "integer" },
              "rationale": { "type": "string" }
            }
          }
        }
      }
    },
    "archetype_impacts": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "archetype_id": { "type": "string" },
          "net_impact": { "type": "string", "enum": ["improved", "unchanged", "deteriorated"] },
          "key_changes": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "trend_id": { "type": "string" },
                "description": { "type": "string" }
              }
            }
          }
        }
      }
    },
    "scoring_recommendations": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "archetype_id": { "type": "string" },
          "trend_id": { "type": "string" },
          "dimension": { "type": "string", "enum": ["exposure", "alignment", "evidence_quality"] },
          "current_value": { "type": "integer" },
          "proposed_value": { "type": "integer" },
          "rationale": { "type": "string" },
          "evidence_source": { "type": "string" }
        }
      }
    },
    "source_health": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "source_id": { "type": "string" },
          "issues_scanned": { "type": "integer" },
          "data_points_extracted": { "type": "integer" },
          "yield_rating": { "type": "string", "enum": ["high", "medium", "low", "none"] },
          "flags": { "type": "array", "items": { "type": "string" } }
        }
      }
    },
    "discoveries": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "found_via": { "type": "string" },
          "trend_relevance": { "type": "array", "items": { "type": "string" } },
          "tier_recommendation": { "type": "string", "enum": ["promote_tier_1", "promote_tier_2", "watch", "discard"] }
        }
      }
    }
  }
}
```

---

## 6. Quarterly Source Rotation Review

### 6.1 Purpose

Every quarterly deep dive includes a formal evaluation of the source list. The goal is to keep the source list lean, high-signal, and gap-free. Sources that stop producing useful intelligence get demoted or retired. Sources discovered through Tier 3 that consistently produce value get promoted.

### 6.2 Promotion criteria (Tier 3 -> Tier 2, Tier 2 -> Tier 1)

| Criterion | Threshold for promotion |
|-----------|------------------------|
| **Data point yield** | Produced 2+ usable data points in 2+ consecutive check-ins |
| **Trend coverage** | Covers a trend with fewer than 3 existing Tier 1 sources |
| **Uniqueness** | Provides evidence not available from existing Tier 1/2 sources |
| **Reliability** | No retracted claims, consistent methodology, named authors/institutions |
| **Accessibility** | Can be accessed reliably (stable URL, consistent publication schedule) |

A source needs to meet at least 3 of 5 criteria for promotion. The analyst documents the promotion rationale in the check-in output.

### 6.3 Demotion criteria (Tier 1 -> Tier 2, Tier 2 -> Retired)

| Criterion | Threshold for demotion |
|-----------|----------------------|
| **Zero yield** | Produced 0 usable data points across 2 consecutive quarterly check-ins |
| **Ceased publication** | No new content published in 6+ months |
| **Quality decline** | Multiple retracted claims, shift to opinion without evidence, paywall preventing access |
| **Redundancy** | Another source covers the same trends with equal or better quality |
| **Access loss** | Paywall added, API deprecated, or publication discontinued |

A source meeting 2+ demotion criteria is flagged for demotion. Tier 1 sources demote to Tier 2 first (not directly retired) unless they have ceased publication entirely.

### 6.4 Coverage gap analysis

At each quarterly review, the analyst checks whether any trend has insufficient source coverage:

| Gap level | Definition | Action |
|-----------|------------|--------|
| **Critical gap** | Trend has 0 Tier 1 sources | Immediate: promote a Tier 2 source or initiate targeted Tier 3 discovery |
| **Weak coverage** | Trend has only 1 Tier 1 source | Flag: identify backup sources from Tier 2 or discovery |
| **Adequate** | Trend has 2+ Tier 1 sources | No action needed |

**Current coverage assessment (as of 2026-06-26):**

| Trend | Tier 1 sources | Coverage level |
|-------|----------------|----------------|
| S-1 | T1-SA, T1-GS, T1-CW-DC, T1-BB | Adequate |
| S-2 | T1-NVDA-ER, T1-SA | Adequate |
| S-3 | T1-HH | Weak -- add secondary market data source |
| S-4 | T1-NVDA-GTC | Weak -- add CPU-GPU analysis source |
| S-5 | T1-SA | Weak -- add KV cache / inference infrastructure source |
| S-6 | T1-SA, T1-HH, T1-CW-SEC, T1-NEB, T1-LAMBDA, T1-BB | Adequate |
| D-1 | T1-EPO, T1-SA | Adequate |
| D-2 | T1-AINEWS, T1-NVDA-GTC, T1-BB | Adequate |
| D-3 | None at Tier 1 | Critical gap -- promote Menlo Ventures or McKinsey |
| D-4 | T1-AINEWS | Weak -- add open-weight ecosystem source |
| D-5 | None at Tier 1 | Critical gap -- promote edge AI market source |
| D-6 | T1-EPO, T1-GS | Adequate |
| M-1 | T1-SA | Weak -- add orchestration-specific source |
| M-2 | T1-SA, T1-HH | Adequate |
| M-3 | T1-NVDA-ER, T1-SA, T1-BB | Adequate |
| M-4 | None at Tier 1 | Critical gap -- promote SkyPilot or multi-cloud source |
| M-5 | T1-SA-CM | Weak -- add security-specific source |
| M-6 | T1-GS, T1-MS, T1-BB | Adequate |
| R-1 | None at Tier 1 | Critical gap (if trend activated) -- promote regulatory source |

### 6.5 Source rotation review output

The quarterly review produces a structured recommendation table:

| Source ID | Current tier | Proposed action | Rationale |
|-----------|-------------|-----------------|-----------|
| [id] | Tier [n] | Promote / Demote / Retire / No change | [1-sentence rationale] |

All promotion and demotion recommendations require human approval before updating `intelligence_sources.json`.

---

## 7. Scoring Tool Integration

### 7.1 Evidence flow: check-in to Source Registry

When a check-in produces a new data point with EQ >= 2, it should be added to the scoring tool's Source Registry (Tab 12, ss3.12).

**Process:**

1. Assign a citation key following the existing convention (e.g., `SA-DC-2026Q3` for a SemiAnalysis DC report from Q3 2026)
2. Create a Source Registry row: Key, Type, Author, Title, Date, URL, Trends Supported, Key Data Points, Last Verified
3. Add the citation key to the relevant trend's Citation Keys field (Trend Definitions tab, column N)
4. Log the addition in the Decision Journal

### 7.2 When to update scoring matrix cells

Not every new data point warrants a scoring matrix change. The system uses a threshold-based approach:

| Change type | Threshold | Action |
|-------------|-----------|--------|
| **EQ upgrade** | New corroborating source for an existing claim | Update EQ (1->2 or 2->3). Low friction, no re-scoring needed. |
| **EQ downgrade** | Source retracted, data revised, or contradicting evidence found | Update EQ (3->2 or 2->1). Flag for attention. |
| **Exposure change** | Structural shift in how a trend affects an archetype | Rare. Requires strong evidence of structural change (e.g., a trend that was secondary becoming existential). Triggers Decision Journal entry. |
| **Alignment change** | Fundamental shift in whether a trend helps or hurts an archetype | Requires re-evaluation against VRIO criteria (Scoring Tool ss3.11.3). Triggers Decision Journal entry with VRIO reference. |
| **Probability update** | New evidence shifts trend probability | Update P(Consensus), P(Disruption), or P(Regression) on Scenario Weights tab. Low friction. |

### 7.3 Full re-scoring triggers

A full re-scoring (all 7 archetypes for a trend, or all 18 trends for an archetype) is triggered when:

- A new trend is added to the framework (requires scoring all 7 archetypes)
- A trend is retired (zero out all 7 archetypes' exposure)
- 3+ cells for a single archetype are recommended for change in a single check-in
- A structural market event invalidates assumptions across multiple trends (e.g., hyperscaler exits GPU cloud entirely)
- The scoring tool's semi-annual review cycle (Scoring Tool ss3.13) is reached

### 7.4 Decision Journal integration

Every scoring change produced by a check-in is pre-formatted as a Decision Journal row (section G of the output document). The format matches Tab 10 of the scoring tool (ss3.10):

| Field | Source |
|-------|--------|
| Date | Check-in date |
| Analyst | Check-in analyst |
| Tab affected | "Scoring Matrix" or "Trend Definitions" or "Scenario Weights" |
| Cell reference | Excel cell address (populated after generator produces workbook) |
| Trend ID | From check-in trend update |
| Archetype | From check-in archetype impact |
| Dimension changed | Exp / Ali / EQ / Probability |
| Old value | Current value in scoring tool |
| New value | Proposed value from check-in |
| Rationale | From check-in recommendation |
| Evidence source | Citation key(s) |

---

## 8. AI Agent Integration

### 8.1 Design for agent execution

This system is designed so that an AI agent (Claude Code or similar) can execute each check-in with minimal human intervention. The design follows the user's vision: a scoring tool that functions as an AI-collaborative strategic advisor, auto-updating market state from sources.

**Agent capabilities required:**

| Capability | Used for | Tool/method |
|------------|----------|-------------|
| Web search | Finding new publications from Tier 1/2 sources | WebSearch tool |
| Web fetch | Reading source content | WebFetch tool |
| File read/write | Reading prior check-ins, writing output | Read/Write tools |
| JSON manipulation | Updating `intelligence_sources.json`, producing sidecar JSON | Bash/Write tools |
| Structured analysis | Multi-perspective synthesis, archetype impact assessment | Prompt-driven reasoning |

### 8.2 Agent execution prompts

Each check-in type has a structured prompt template the agent receives.

**Monthly check-in prompt template:**

```
You are executing a monthly market intelligence check-in for the GPU cloud
Strategic Fitness Scoring Tool.

Period: [start date] to [end date]
Last check-in: [path to previous check-in output]

INSTRUCTIONS:

1. Read the previous check-in at [path] to understand baseline state.

2. For each Tier 1 source in the source registry at
   gpu_industry/strategy/tool/configs/intelligence_sources.json,
   search for publications since [last check-in date].

3. For each publication found:
   a. Extract data points relevant to any of the 18+1 trends
   b. Assign an Evidence Quality score (1-3) using the scoring tool's rubric:
      1=Inference only, 2=Supported (1+ data point), 3=Corroborated (multiple sources)
   c. Note which archetype(s) are affected

4. Write the output document to:
   gpu_industry/research/check_ins/YYYY-MM-monthly.md
   using the abbreviated template (sections A, B, C, F only).

5. Write the JSON sidecar to:
   gpu_industry/research/check_ins/YYYY-MM-monthly.json

6. If any data point warrants a scoring matrix change, flag it in section B
   with a clear recommendation and rationale.

7. If you discover sources not in the registry (Tier 3 discovery), log them
   in section F with a tier recommendation.

CONSTRAINTS:
- Do not change the scoring matrix directly. Only recommend changes.
- Use the EQ rubric strictly. Do not inflate evidence quality.
- When sources conflict, note the conflict rather than choosing a side.
- Blacklist: [list of already-processed sources from prior check-ins]
```

**Quarterly deep dive prompt template:**

```
You are executing a quarterly deep-dive market intelligence check-in for
the GPU cloud Strategic Fitness Scoring Tool.

Period: [quarter start] to [quarter end]
Previous check-ins this quarter: [paths]
Source registry: gpu_industry/strategy/tool/configs/intelligence_sources.json
Scoring tool spec: docs/superpowers/specs/2026-06-21-strategic-fitness-scoring-tool-design.md

INSTRUCTIONS:

1. Read all monthly check-ins from this quarter at [paths].

2. Scan ALL Tier 1 and Tier 2 sources for publications during this quarter.

3. Follow Tier 3 discovery leads from any new references found.

4. Write the full output document (all sections A-G) to:
   gpu_industry/research/check_ins/YYYY-QN-deep-dive.md

5. Complete the multi-perspective synthesis:
   a. Supply-side: silicon cycles, energy, infrastructure, neocloud financials
   b. Demand-side: inference mix, enterprise adoption, agentic, edge
   c. Market structure: orchestration, pricing, ASICs, security, financialization

6. Complete per-archetype impact assessment for all 7 archetypes.

7. Run source rotation review:
   a. Check each source against promotion/demotion criteria (spec ss6.2, ss6.3)
   b. Run coverage gap analysis (spec ss6.4)
   c. Produce rotation recommendation table (spec ss6.5)

8. Pre-format all scoring recommendations as Decision Journal rows (section G).

9. Write the JSON sidecar.

CONSTRAINTS:
- Same as monthly, plus:
- Source rotation recommendations require human approval. Flag, do not execute.
- Full re-scoring triggers (spec ss7.3) should be flagged explicitly.
- Pass the full blacklist of already-processed sources to avoid re-scanning.
```

### 8.3 Agent-to-scoring-tool pipeline

The long-term vision is a pipeline where check-in outputs feed scoring matrix updates semi-automatically:

```
Phase 1 (current): Agent produces check-in -> Human reviews -> Human updates scoring tool
Phase 2 (near-term): Agent produces check-in + pre-formatted updates -> Human approves -> Agent applies updates
Phase 3 (future): Agent produces check-in -> Agent applies low-risk updates (EQ changes) automatically -> Human reviews
```

**Phase 2 implementation (next milestone):**

The agent reads the JSON sidecar from a check-in, maps each `scoring_recommendation` to a cell in `scores.json`, and prepares a diff. The analyst reviews the diff and approves or rejects each change. Approved changes are written to `scores.json` and the generator re-runs to produce an updated workbook.

```bash
# Agent produces the diff
python -m intelligence.apply_recommendations \
  --check-in research/check_ins/2026-Q3-deep-dive.json \
  --scores strategy/tool/configs/scores.json \
  --output strategy/tool/configs/scores_proposed.json \
  --diff strategy/tool/configs/scores_diff.json

# Human reviews the diff (displayed in terminal or editor)
# Human approves by replacing scores.json with scores_proposed.json

# Generator re-runs
python -m generator.build_workbook
```

### 8.4 Automated source health tracking

The agent maintains a running source health record by comparing source yield across check-ins. After each check-in, the agent appends yield data to a log:

`gpu_industry/research/check_ins/source_health_log.json`

```json
{
  "entries": [
    {
      "check_in": "2026-Q3-deep-dive",
      "date": "2026-10-05",
      "source_yields": [
        { "source_id": "T1-SA", "issues_scanned": 6, "data_points": 23, "yield": "high" },
        { "source_id": "T1-HH", "issues_scanned": 4, "data_points": 8, "yield": "medium" }
      ]
    }
  ]
}
```

At quarterly reviews, the agent reads this log to identify sources with declining yield and proposes demotions per the criteria in ss6.3.

---

## 9. Example Check-In Output

The following is a partial example of what a monthly check-in output looks like, based on the newsletter evidence already compiled on 2026-06-22.

```markdown
# Market Intelligence Check-In -- 2026-06 (Monthly)

Date: 2026-06-26
Analyst: J. Skifter
Type: Monthly
Period covered: 2026-06-01 to 2026-06-26

---

## A. Executive Summary

- Open-weight models reached frontier-adjacent performance: GLM-5.2 (753B, MIT license)
  matched Opus 4.8 on coding benchmarks at 1/4 the cost. D-4 strengthening.
- Fable/Mythos export-control suspension is the strongest evidence yet for R-1 regulatory
  fragmentation as a standalone trend. Discriminating power vs M-5 confirmed.
- SemiAnalysis debunked the "50% DC cancellation" narrative with satellite data,
  but confirmed 7-10 year grid connection lead times. S-1 remains structural.
- IREN issued A-rated GPU-backed debt at 3.31%, validating the Compute Financier
  archetype's financial engineering moat. M-6 EQ upgrade warranted.
- Unitree IPO filing shows 10,000 humanoid robots shipped, $27K unit price, 67% gross
  margins. D-2 robotics sub-trend upgraded from speculative to supported.

## B. Per-Trend Evidence Updates

### Supply-Side

#### S-1 Energy Wall
- **Status:** New evidence (trend strengthening)
- **New data points:**
  - [SA-1]: SemiAnalysis NA hyperscaler self-build forecast moved only ~1% over 6 months;
    "50% canceled" narrative debunked. Source: T1-SA. EQ: 3.
  - [SA-4]: ERCOT >410GW large-load interconnection requests, >87% from DCs.
    Queue is ~5x Texas peak demand. Source: T1-SA. EQ: 3.
  - [SA-7]: Grid connection lead times 7-10 years in multiple metros. Transformer
    bushings 3-5 year quotes. Source: T1-SA. EQ: 2.
  - [BB-5]: European interconnection queues 5-8 years for >100MW facilities. IREN
    acquired Nostrum (490MW Spain). Source: T1-BB. EQ: 2.
  - [BB-7]: DOJ intervened to defend xAI's unpermitted gas turbines, arguing national
    security. Source: T1-BB. EQ: 3.
- **EQ change:** No change (already EQ=3 for most archetypes)
- **Scoring note:** SA-9 (hyperscaler behind-the-meter generation) is new hyperscaler
  implication data for the Trend Definitions tab.

#### S-6 Neocloud Financial Bifurcation
- **Status:** New evidence (trend strengthening)
- **New data points:**
  - [BB-8]: IREN first A-rated securitized GPU debt at 3.31% blended. Source: T1-BB. EQ: 2.
  - [SA-12]: CoreWeave guided 1.7GW Active Power by end-2026, ~$35B RPO. Source: T1-SA. EQ: 3.
  - [BB-9]: Hydra Host raised $100M Series A. Source: T1-BB. EQ: 2.
- **EQ change:** A5 (Compute Financier) x S-6 EQ: 2 -> 3, because IREN securitization
  provides corroborated primary evidence for financial structuring moat.

[... remaining trends ...]

## C. Archetype Impact Assessment

### A5 Compute Financier
- **Net impact:** Improved
- **Key changes:**
  - S-6: IREN A-rated GPU ABS at 3.31% validates the financial engineering moat
    identified in VRIO (V+R+I+O = sustained advantage). Spread compression from
    1,300 to 110 bps continues.
  - M-6: ICE/Ornn compute futures move from announcement to market infrastructure.
- **Scoring recommendations:**
  - Cell [A5 x S-6 EQ]: 2 -> 3, because IREN securitization is primary evidence.
  - Cell [A5 x M-6 EQ]: 2 -> 3, because ICE listing is verifiable primary data.

[... remaining archetypes ...]

## F. Discovery Log

### New sources found this cycle

| Source | Found via | Trend relevance | Tier recommendation |
|--------|----------|-----------------|---------------------|
| InferentialInvestor | BB-4 Graviton analysis | S-4, D-2 | Tier 2 |
| DataGravity (newsletter) | arXiv disaggregated inference references | S-4, S-5, M-4 | Tier 2 |
| Spheron Blog (technical deep dives) | Google results for KV cache data | S-5, S-2, M-3 | Tier 2 |
```

---

## 10. File Structure

### 10.1 Directory layout

```
gpu_industry/
  strategy/tool/configs/
    intelligence_sources.json          # Machine-readable source registry
  research/
    check_ins/
      source_health_log.json           # Running source yield tracker
      2026-06-monthly.md               # Monthly check-in output
      2026-06-monthly.json             # Monthly check-in JSON sidecar
      2026-Q3-deep-dive.md             # Quarterly deep dive output
      2026-Q3-deep-dive.json           # Quarterly deep dive JSON sidecar
    evidence_supply.md                 # Existing supply-side evidence
    evidence_demand.md                 # Existing demand-side evidence
    evidence_market_academic.md        # Existing market structure + academic evidence
    newsletter_evidence.md             # Existing newsletter data points
    competitor_stacks.md               # Existing competitor analysis
```

### 10.2 Naming conventions

| File type | Pattern | Example |
|-----------|---------|---------|
| Monthly check-in | `YYYY-MM-monthly.md` | `2026-07-monthly.md` |
| Quarterly deep dive | `YYYY-QN-deep-dive.md` | `2026-Q3-deep-dive.md` |
| Ad-hoc alert | `YYYY-MM-DD-alert-[topic].md` | `2026-07-15-alert-rubin-ga.md` |
| JSON sidecar | Same as above with `.json` extension | `2026-Q3-deep-dive.json` |

---

## 11. Initial Calibration

### 11.1 Baseline evidence inventory

Before the first check-in, the analyst should establish a baseline by cataloging all evidence already collected across the research directory. As of 2026-06-26, the existing evidence inventory is:

| Evidence file | Trends covered | Data points | Last updated |
|---------------|----------------|-------------|--------------|
| `evidence_supply.md` | S-2, S-3, S-4, S-5 | ~25 | 2026-06-22 |
| `evidence_demand.md` | D-1, D-3, D-5, D-6 | ~25 | 2026-06-22 |
| `evidence_market_academic.md` | M-1 through M-6, academic framework | ~30 + 10 academic | 2026-06-22 |
| `newsletter_evidence.md` | S-1, S-6, D-2, D-4, M-3, M-6 | ~50 | 2026-06-22 |
| `competitor_stacks.md` | Cross-archetype | ~5 competitor profiles | 2026-06-22 |
| `stack_coreweave.md` | A1 archetype | Full stack analysis | 2026-06-22 |
| `stack_nebius.md` | A2 archetype | Full stack analysis | 2026-06-22 |

### 11.2 First check-in: July 2026

The first monthly check-in should:

1. Populate `intelligence_sources.json` with all Tier 1 and Tier 2 sources from ss3.3 and ss3.4
2. Establish the `research/check_ins/` directory
3. Run a full Tier 1 scan for publications from June 1-30, 2026
4. Produce the first monthly output using the template in ss5.1
5. Identify the most critical coverage gaps from ss6.4 and propose source promotions

### 11.3 First quarterly deep dive: Q3 2026 (October)

The first quarterly deep dive consolidates July, August, and September monthly check-ins and runs the first source rotation review.

---

## 12. Limitations and Future Work

### 12.1 Current limitations

- **No automated source scraping.** The agent must use web search and web fetch tools to access sources. There is no RSS/API integration for automatic detection of new publications.
- **Paywall sources require manual access.** Goldman Sachs, Morgan Stanley, and some SemiAnalysis deep dives sit behind paywalls that agents cannot traverse. The analyst must provide content or summaries.
- **No real-time monitoring.** The system operates on a check-in cadence, not continuous monitoring. Material events between check-ins rely on the analyst noticing them and triggering an ad-hoc alert.
- **Single-analyst dependency.** The current design assumes one analyst (or one agent supervised by one analyst). The inter-rater reliability protocol in the scoring tool (ss3.11.1) is not replicated here because check-ins produce recommendations, not final scores.

### 12.2 Future enhancements

- **RSS/API integration:** Automated detection of new publications from Tier 1 sources, triggering agent check-in runs without manual initiation.
- **Scheduled agent execution:** Monthly check-ins run automatically via cron or the Claude Code `/schedule` skill, with human review of output before scoring changes are applied.
- **Source health dashboard:** A simple web view or notebook showing yield trends per source over time, coverage heat map by trend, and upcoming review dates.
- **Cross-check-in trend tracking:** Longitudinal view of how each trend's evidence base has evolved across check-ins, supporting the scoring tool's semi-annual review.
- **Multi-analyst workflow:** Multiple analysts produce independent check-ins; a reconciliation step merges findings, surfacing disagreements for discussion (analogous to the scoring tool's inter-rater protocol).
