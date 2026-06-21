# Strategic Fitness Scoring Tool — Technical Specification

**Date:** 2026-06-21
**Purpose:** Python-generated Excel workbook that scores GPU cloud business model archetypes against industry trends, with a dynamic self-assessment feature for market entrants.
**Thesis context:** EPFL Master's thesis contribution — a generalizable strategic playbook grounded in Resource-Based View (Barney 1991, Teece/Pisano/Shuen 1997). Not ICN-specific.
**Architecture:** Approach B — Python generator script reads JSON configs, produces `.xlsx` with live formulas, conditional formatting, and data validation.

---

## 1. System Overview

### 1.1 What the tool does

A market entrant (energy company, cloud-native software firm, financial institution, sovereign entity, etc.) opens the Excel workbook, goes to the Self-Assessment tab, selects their primary business model archetype, maps their stack coverage across L0-L7 sub-components, and inputs their key resources and team capabilities. The tool computes:

- **Archetype fit scores** for all 7 archetypes based on their profile
- **Gap analysis** showing what they'd need to build or acquire, with acquisition difficulty ratings
- **Personalized trend exposure profile** — top tailwinds and headwinds for their position
- **Concentration risk** — how diversified or concentrated their strategic bet is
- **Time-discounted TRS** — weighted toward trends that matter on their planning horizon
- **Strategic direction recommendation** — which archetype to pursue, what partnerships to consider

### 1.2 What the tool is NOT

- Not a financial model (no revenue/cost projections)
- Not a competitive intelligence dashboard (no real-time data feeds)
- Not ICN-specific (the framework is archetype-generic)

### 1.3 Architecture

```
configs/ (JSON, version-controlled)
    ↓
generator/ (Python + openpyxl)
    ↓
output/ (.xlsx with live formulas)
```

Users interact only with the `.xlsx`. The generator is run when configs change (new trend, new archetype, updated scores). The Excel contains all formulas as cell references — no macros, no VBA.

---

## 2. Tab Architecture

12 tabs, each with a defined role and data flow.

### Tab map

| # | Tab Name | Role | User interaction |
|---|----------|------|-----------------|
| 1 | Dashboard | Executive summary: composite TRS, scenario-weighted TRS, concentration risk, rank order | Read-only (auto-computed) |
| 2 | Self-Assessment | User inputs company profile → gets archetype fit, gap analysis, personalized exposure | Input tab (dropdowns, checklists) |
| 3 | Scoring Matrix | 7×18 matrix with Exp/Ali/EQ per cell, formula-computed scores | Editable (update scores directly) |
| 4 | Trend Definitions | 18 trends with metadata, evidence, probability weights, scoring questions | Reference + editable probabilities |
| 5 | Archetype Profiles | 7 archetypes with stack requirements, resources, exemplars | Reference |
| 6 | Stack Taxonomy | ~30 L0-L7 components with per-archetype importance weights and acquisition difficulty | Reference |
| 7 | Scenario Weights | Per-trend probabilities and impact weights for 3 scenarios + time-discount multipliers | Editable |
| 8 | Trend Interactions | Top 15-20 trend pairs with reinforcement/cancellation effects (+1/−1) | Reference + editable |
| 9 | Resource Playbook | 7 entrant profiles mapped to archetypes with rationale | Reference |
| 10 | Decision Journal | Log of scoring changes with date, cell, old/new values, rationale | Append-only log |
| 11 | Methodology | Formal rubrics, decision trees, worked examples | Reference |
| 12 | Expansion Guide | Update protocol, source registry, cadence guidance | Reference |

### Data flow

```
Stack Taxonomy ──→ Self-Assessment (user inputs)
                        │
Archetype Profiles ─→ Fit Score computation
                        │
Scoring Matrix ←── Trend Definitions + Scenario Weights + Trend Interactions
      │
      ├──→ Dashboard (composite TRS, concentration risk, time-discounted TRS)
      └──→ Self-Assessment (personalized exposure profile)
```

---

## 3. Tab Specifications

### 3.1 Dashboard

**Purpose:** One-screen executive summary. The first thing a reader sees.

**Layout:**

Row 1-4: Title block
- "GPU Cloud Strategic Fitness Scoring Matrix"
- Subtitle: "Resource-Based Playbook for Market Entrants — 7 Archetypes × 18 Industry Trends"
- Grounding: "Barney 1991, Teece et al. 1997, ClusterMAX 2.0, Hydra Host bifurcation thesis"
- Last updated date

Row 6-14: **Composite Score Table**

| Column | Content | Source |
|--------|---------|--------|
| A | Archetype name | Archetype Profiles tab |
| B | Raw TRS | SUM of Scoring Matrix raw scores (Exp × Ali) |
| C | Evidence-Adjusted TRS | SUM of Scoring Matrix adjusted scores (Exp × Ali × EQ/3) |
| D | Time-Discounted TRS | SUM of adjusted scores × time-discount multiplier per trend |
| E | Consensus-Weighted TRS | SUM of adjusted scores × P(Consensus) × Impact(Consensus) |
| F | Disruption-Weighted TRS | SUM of adjusted scores × P(Disruption) × Impact(Disruption) |
| G | Regression-Weighted TRS | SUM of adjusted scores × P(Regression) × Impact(Regression) |
| H | Concentration Risk (HHI) | Herfindahl index on positive cell scores |
| I | Rank (by Adj. TRS) | RANK formula |
| J | Strongest tailwind | INDEX/MATCH to max positive cell score |
| K | Worst headwind | INDEX/MATCH to min negative cell score |

Row 16+: **Interpretation Guide** — how to read the numbers, what each metric means.

**Conditional formatting:**
- TRS columns: green gradient for positive, red gradient for negative
- HHI column: yellow-orange for concentrated (>0.25), green for diversified (<0.15)
- Rank column: gold/silver/bronze fill for top 3

**Formulas (all cross-tab references):**
- Raw TRS: `=SUM('Scoring Matrix'!score_column_for_archetype)`
- Time-Discounted: `=SUMPRODUCT('Scoring Matrix'!scores, 'Scenario Weights'!time_discount)`
- Scenario-Weighted: `=SUMPRODUCT('Scoring Matrix'!scores, 'Scenario Weights'!prob_col, 'Scenario Weights'!impact_col)`
- HHI: `=SUMPRODUCT((positive_scores/SUM(positive_scores))^2)` — computed only over cells where score > 0

### 3.2 Self-Assessment

**Purpose:** The user inputs their company profile and gets a personalized strategic recommendation.

**Section A: Company Profile (rows 1-8)**

| Row | Input | Method | Values |
|-----|-------|--------|--------|
| 2 | Company name | Free text | — |
| 3 | Primary archetype (current) | Dropdown | 7 archetypes + "Unsure" |
| 4 | Secondary archetype | Dropdown | 7 archetypes + "None" |
| 5 | Planning horizon | Dropdown | "1-2 years", "3-5 years", "5-10 years" |
| 6 | Capital available | Dropdown | "Light (<$10M)", "Medium ($10-100M)", "Heavy ($100M+)", "Sovereign" |
| 7 | Geographic base | Dropdown | "North America", "Europe", "Asia-Pacific", "Middle East", "Other" |
| 8 | Regulatory environment | Dropdown | "Light touch", "Moderate", "Strict (SOC2/ISO mandatory)", "Sovereign requirements" |

**Section B: Stack Coverage (rows 10-42)**

Each row is a stack component from the Stack Taxonomy tab. User selects coverage level per component.

| Column | Content |
|--------|---------|
| A | Layer (L0-L7) |
| B | Component name |
| C | Component description (from Stack Taxonomy) |
| D | **User input: coverage level** — dropdown: Own / Control / Use / Outsource / N/A |
| E | Numeric value (auto-computed from D): Own=3, Control=2, Use=1, Outsource=0.5, N/A=0 |
| F-L | Per-archetype importance weight (hidden, from Stack Taxonomy) |

Data validation on column D: list validation from a named range.

**Section C: Key Resources (rows 44-56)**

| Column | Content |
|--------|---------|
| A | Resource category |
| B | Description |
| C | **User input: strength** — dropdown: "Deep (core competency)", "Competent", "Nascent", "Absent" |
| D | Numeric value: Deep=3, Competent=2, Nascent=1, Absent=0 |

12 resource categories:
1. Energy access / generation capacity
2. Capital structure / balance sheet
3. Hardware relationships (NVIDIA allocation tier, OEM)
4. Software engineering depth (ML, kernel, infra)
5. Compliance posture (SOC2, ISO27001, GDPR)
6. Existing customer base (enterprise, developer)
7. Demand network / sales channel
8. Operational expertise (DC ops, cluster management)
9. Proprietary IP (orchestration, inference optimization)
10. Financial structuring capability
11. Industry credibility / brand
12. Geographic / jurisdictional positioning

**Section D: Team Capabilities (rows 58-64)**

| Capability | Dropdown |
|------------|----------|
| ML / AI engineering | 1-Nascent / 2-Competent / 3-Deep |
| Infrastructure operations | 1 / 2 / 3 |
| CUDA kernel / low-level development | 1 / 2 / 3 |
| Sales / business development | 1 / 2 / 3 |
| Financial structuring / deal-making | 1 / 2 / 3 |
| Regulatory / compliance | 1 / 2 / 3 |

**Section E: Outputs (rows 68+)**

**E1. Archetype Fit Scores (rows 68-76)**

For each of the 7 archetypes, compute:

```
Stack_Fit = SUMPRODUCT(user_coverage_values, archetype_component_weights)
          / SUMPRODUCT(max_coverage_values, archetype_component_weights)

Resource_Fit = SUMPRODUCT(user_resource_values, archetype_resource_weights)
             / SUMPRODUCT(max_resource_values, archetype_resource_weights)

Overall_Fit = (Stack_Fit × 0.5) + (Resource_Fit × 0.3) + (Capability_Fit × 0.2)
```

Displayed as:
| Archetype | Stack Fit % | Resource Fit % | Capability Fit % | Overall Fit % | Rank |

Conditional formatting: top 2 archetypes highlighted green.

**E2. Gap Analysis (rows 78-95)**

For the top-ranked archetype, list all stack components where:
- Archetype importance weight ≥ 2 (beneficial or essential) AND
- User coverage ≤ 1 (Use or below)

Each gap row shows:
| Component | Required level | Current level | Gap severity | Acquisition difficulty (1-3) | Bridgeable? |

Acquisition difficulty comes from the Stack Taxonomy tab:
- 1 = Acquirable in 6-12 months (hire, license, partner)
- 2 = Requires 1-3 years of investment (build team, establish relationships)
- 3 = Structural barrier (energy assets, NVIDIA allocation, jurisdiction) — requires M&A or partnership

**E3. Personalized Trend Exposure (rows 97-108)**

Pulls the Scoring Matrix scores for the user's primary archetype (or best-fit if "Unsure"). Shows:
- Top 5 tailwinds (highest positive cell scores) with trend name and score
- Top 5 headwinds (most negative cell scores) with trend name and score
- Net exposure statement

**E4. Concentration Risk (rows 110-114)**

HHI computed on the archetype's positive scores. Interpretation:
- HHI < 0.15: Diversified — resilient to individual trend reversals
- HHI 0.15-0.25: Moderate concentration — monitor top 2-3 drivers
- HHI > 0.25: Concentrated — high sensitivity to specific trends

**E5. Strategic Direction (rows 116-125)**

Formula-driven conditional text blocks using nested `IF` statements:

```
IF Overall_Fit_Rank1 > 0.7 AND user_archetype = Rank1_archetype:
  "Your current positioning aligns well with [archetype]. Focus on..."
IF Overall_Fit_Rank1 > 0.7 AND user_archetype ≠ Rank1_archetype:
  "Consider pivoting toward [archetype]. Your stack and resources are a stronger fit..."
IF Overall_Fit_Rank1 < 0.5:
  "No single archetype is a strong fit. Consider the Infrastructure Enabler model..."
```

Plus: partnership recommendations from Resource Playbook cross-reference.

### 3.3 Scoring Matrix

**Purpose:** The core analytical engine — 7 archetypes × 18 trends.

**Layout:**

- Row 1: Archetype names (merged across 4 sub-columns each)
- Row 2: Sub-headers — Exp | Ali | EQ | Score — repeated 7 times
- Column A: Trend ID
- Column B: Trend name
- Rows 3+: Data rows, grouped by category with section headers (Supply-Side, Demand-Side, Market Structure)
- Final row: Composite TRS (SUM of all score cells per archetype)

**Per-cell structure (4 columns per archetype):**

| Sub-col | Content | Input/Formula | Validation |
|---------|---------|---------------|------------|
| Exp | Exposure | User input | Integer 0-3, data validation |
| Ali | Alignment | User input | Integer -2 to +2, data validation |
| EQ | Evidence Quality | User input | Integer 1-3, data validation |
| Score | Cell score | Formula | `=Exp*Ali*(EQ/3)` |

**Data validation prompts:**
- Exposure: "0=Not material, 1=Secondary, 2=Primary, 3=Existential"
- Alignment: "-2=Structural threat, -1=Headwind, 0=Neutral, +1=Tailwind, +2=Structural enabler"
- Evidence Quality: "1=Inference only, 2=Supported (1+ data point), 3=Corroborated (multiple sources)"

**Interaction-adjusted score (additional column per archetype):**

After the base Score column, an optional "Adj. Score" column that incorporates trend interactions:

```
Adj_Score = Base_Score + Σ(interaction_effect_with_trend_j × base_score_trend_j)
```

Where interaction effects come from the Trend Interactions tab. This is the v1.1 enhancement — for v1, the base Score column is the primary metric and Adj. Score is included but can be ignored.

**Conditional formatting on Score columns:**
- Score > 3: green fill (`DCFCE7`)
- Score < -3: red fill (`FEE2E2`)
- Score = 0: light gray fill

**Total row:**
- Raw TRS: `=SUM(all Exp*Ali for this archetype)`
- Adj. TRS: `=SUM(all Score cells for this archetype)`
- Time-Discounted TRS: `=SUMPRODUCT(score_range, time_discount_range)`

### 3.4 Trend Definitions

**Purpose:** Master reference for all 18 trends. Feeds the Scoring Matrix and Scenario Weights.

**Columns:**

| Col | Header | Content |
|-----|--------|---------|
| A | ID | S-1, S-2, ..., D-1, ..., M-1, ... |
| B | Category | Supply-Side / Demand-Side / Market Structure |
| C | Trend Name | Short name |
| D | Description | 1-2 sentence description of the trend and its direction |
| E | Key Evidence | Specific data points, sources, dates |
| F | Time Horizon | Near-term (1-3yr) / Medium (3-5yr) / Structural (5-10yr) |
| G | Time Discount Multiplier | Auto-mapped: Near=1.0, Medium=0.8, Structural=0.6 (overridable) |
| H | P(Consensus) | 0.00-1.00, formatted as % |
| I | P(Disruption) | 0.00-1.00 |
| J | P(Regression) | 0.00-1.00 |
| K | Scoring Q: Exposure | 2-4 discriminating questions for scorers |
| L | Scoring Q: Alignment | 2-4 discriminating questions for scorers |
| M | Last Updated | Date of most recent evidence refresh |
| N | Status | Active / Under Review / Retired |

**Data validation:**
- Time Horizon: list validation (Near-term, Medium, Structural)
- Probabilities: decimal 0.00-1.00
- Status: list validation

### 3.5 Archetype Profiles

**Purpose:** Define each archetype with its required stack, resources, and exemplars.

**Columns:**

| Col | Header |
|-----|--------|
| A | ID (A1-A7) |
| B | Archetype Name |
| C | Exemplars |
| D | Stack Coverage (L-levels) |
| E | Capital Intensity |
| F | Description |
| G | Required Resources (comma-separated) |
| H | Hydra Host Framing |
| I-T | Per-resource importance weight (1=optional, 2=beneficial, 3=essential) — 12 columns for 12 resource categories |
| U | Natural transition target (which archetype this one evolves into) |

### 3.6 Stack Taxonomy

**Purpose:** Granular L0-L7 component checklist. Powers the Self-Assessment.

**Columns:**

| Col | Header |
|-----|--------|
| A | Layer (L0-L7) |
| B | Component ID (e.g., L1.1, L1.2) |
| C | Component Name |
| D | Description |
| E | Example implementations (e.g., "NVIDIA Quantum-2 InfiniBand, Arista 7800R") |
| F | Acquisition Difficulty (1-3) |
| G | Acquisition Difficulty Rationale |
| H-N | Per-archetype importance weight (0=irrelevant, 1=optional, 2=beneficial, 3=essential) — 7 columns |

**Initial component list (~30 items):**

```
L0 - Silicon & Hardware Design
  L0.1  GPU/ASIC design IP                          Difficulty: 3
  L0.2  Custom CUDA/ROCm kernel development          Difficulty: 2

L1 - Physical Infrastructure
  L1.1  Data center ownership or long-term lease      Difficulty: 3
  L1.2  Power procurement (PPA / self-generation)     Difficulty: 3
  L1.3  Cooling systems (liquid / immersion)          Difficulty: 2
  L1.4  Rack & server design (custom / OEM)           Difficulty: 2
  L1.5  InfiniBand / high-speed networking HW         Difficulty: 2
  L1.6  Storage hardware (NVMe, object storage)       Difficulty: 1

L2 - Low-Level Software
  L2.1  GPU driver & firmware management              Difficulty: 1
  L2.2  Bare-metal provisioning system                Difficulty: 2
  L2.3  Hypervisor / virtualization layer             Difficulty: 1
  L2.4  Network fabric software (SDN / UFM)           Difficulty: 2
  L2.5  NVLink / NVSwitch configuration mgmt          Difficulty: 2
  L2.6  RDMA / RoCE / InfiniBand software stack       Difficulty: 2

L3 - Orchestration & Resource Management
  L3.1  Container orchestration (K8s / custom)        Difficulty: 1
  L3.2  HPC job scheduling (Slurm / custom)           Difficulty: 2
  L3.3  GPU health monitoring & checkpointing         Difficulty: 2
  L3.4  GPU slicing (MIG / MPS / time-sharing)        Difficulty: 1
  L3.5  Autoscaling / capacity management             Difficulty: 2
  L3.6  Hybrid orchestration (SUNK / Soperator)       Difficulty: 2

L4 - Frameworks & Platform
  L4.1  Distributed training framework support        Difficulty: 1
  L4.2  Developer portal / API platform               Difficulty: 1
  L4.3  Marketplace / capacity aggregation            Difficulty: 2
  L4.4  CLI tooling / SDK                             Difficulty: 1

L5 - Model Serving & Data
  L5.1  Inference engine (vLLM / TensorRT / custom)   Difficulty: 2
  L5.2  Fine-tuning platform                          Difficulty: 2
  L5.3  Model registry & serving infrastructure       Difficulty: 1
  L5.4  KV cache management system                    Difficulty: 2
  L5.5  High-performance object storage               Difficulty: 2
  L5.6  Serverless inference endpoints                Difficulty: 2

L6-L7 - Middleware & Applications
  L6.1  Multi-cloud routing / abstraction             Difficulty: 2
  L6.2  Agentic workflow platform                     Difficulty: 2
  L7.1  End-user AI applications                      Difficulty: 1
```

Per-archetype importance weights will be populated based on the archetype definitions and competitor research.

### 3.7 Scenario Weights

**Purpose:** Adjustable probability and impact weights for three market scenarios, plus time-discount multipliers.

**Section A: Time Discount Multipliers (rows 1-5)**

| Time Horizon | Default Multiplier | Editable? |
|---|---|---|
| Near-term (1-3yr) | 1.00 | Yes |
| Medium (3-5yr) | 0.80 | Yes |
| Structural (5-10yr) | 0.60 | Yes |

The planning horizon selected on the Self-Assessment tab can override these defaults:
- "1-2 years" → Near=1.0, Medium=0.5, Structural=0.3
- "3-5 years" → Near=1.0, Medium=1.0, Structural=0.6
- "5-10 years" → Near=0.8, Medium=1.0, Structural=1.0

**Section B: Per-Trend Weights (rows 7+)**

| Col | Content |
|-----|---------|
| A | Trend ID |
| B | Trend Name |
| C | Time Horizon (from Trend Definitions) |
| D | Time Discount (looked up from Section A based on horizon) |
| E | P(Consensus) |
| F | P(Disruption) |
| G | P(Regression) |
| H | Impact Weight (Consensus) — default 1.0 |
| I | Impact Weight (Disruption) — default 1.0 |
| J | Impact Weight (Regression) — default 1.0 |

**Section C: Scenario Definitions (rows 30+)**

Prose definitions of each scenario for context:
- **Consensus:** Industry trends continue on current trajectory. Most probable baseline.
- **Disruption:** Accelerated change — ASIC diversification faster, neocloud consolidation deeper, energy wall higher, agentic workloads mature faster.
- **Regression:** Slowdown — enterprise AI adoption stalls, GPU demand growth decelerates, financing tightens, open-weight momentum slows.

### 3.8 Trend Interactions

**Purpose:** Capture second-order effects where trends reinforce or cancel each other.

**Layout:**

| Col | Content |
|-----|---------|
| A | Trend A (ID) |
| B | Trend A (Name) |
| C | Trend B (ID) |
| D | Trend B (Name) |
| E | Interaction Effect | +1 (reinforcing) or -1 (cancelling) |
| F | Mechanism | 1-sentence explanation of how A affects B |
| G | Confidence | High / Medium / Low |

**Initial interaction pairs (15-20):**

| Trend A | Trend B | Effect | Mechanism |
|---------|---------|--------|-----------|
| S-1 Energy wall | S-6 Neocloud bifurcation | +1 | Energy scarcity raises capex, accelerating financial bifurcation among neoclouds |
| D-2 Agentic workloads | S-5 KV cache infra | +1 | Long-running agentic sessions drive massive KV cache requirements |
| D-1 Inference dominance | D-4 Open-weight economics | +1 | Inference demand growth × cheap open-weight models = volume explosion |
| M-1 Orchestration commoditization | M-4 Multi-cloud abstraction | +1 | Commoditized orchestration makes abstraction layers more viable |
| D-4 Open-weight economics | M-2 Inference stratification | +1 | Cheaper open-weight models deepen the commodity vs frontier price gap |
| S-2 Silicon supply cycles | S-3 GPU lifecycle & secondary | +1 | Generation transitions directly create secondary market supply waves |
| D-6 Jevons paradox | S-1 Energy wall | +1 | More total compute demand intensifies energy constraints |
| M-3 ASIC diversification | M-1 Orchestration commoditization | +1 | Multi-silicon support requires more abstraction, commoditizing orchestration |
| D-5 Edge distribution | D-1 Inference dominance | -1 | Edge inference substitutes for some centralized cloud inference demand |
| M-6 Financialization | S-6 Neocloud bifurcation | +1 | Financial structuring separates well-capitalized from undercapitalized neoclouds |
| D-3 Enterprise ML proliferation | M-5 Supply-chain security | +1 | Regulated enterprise demand raises security requirements |
| S-4 CPU-GPU rebalancing | D-2 Agentic workloads | +1 | Agentic CPU:GPU ratio shift drives CPU-GPU rebalancing |
| D-4 Open-weight economics | D-6 Jevons paradox | +1 | Cheaper models enable more use cases, increasing total compute demand |
| S-5 KV cache infra | D-3 Enterprise ML proliferation | +1 | Enterprises running fine-tuned models with long context drive storage demand |
| M-4 Multi-cloud abstraction | S-3 GPU lifecycle & secondary | +1 | Abstraction makes it easier to use older/secondary market GPUs |

**How interactions feed into scoring:**

The interaction effect is applied as a small adjustment to the base cell score. For each cell (archetype × trend_A), the interaction-adjusted score is:

```
Adj_Score(A, trend_i) = Base_Score(A, trend_i) + Σ_j(interaction(i,j) × sign(Base_Score(A, trend_j)) × 0.5)
```

The 0.5 multiplier keeps interactions as secondary effects (max ±0.5 per interaction pair) rather than dominating the base score.

### 3.9 Resource Playbook

**Purpose:** Maps 7 entrant profiles to recommended archetypes with strategic rationale.

**Columns:**

| Col | Content |
|-----|---------|
| A | Entrant Profile (Energy Company, Cloud-Native Software, etc.) |
| B | Has (Resources) |
| C | Lacks |
| D | Recommended Archetype(s) |
| E | TRS Score |
| F | Key Trend Rationale |
| G | Pairing Recommendation |
| H | Reference Transaction |

7 entrant profiles:
1. Energy Company / Utility
2. Cloud-Native Software Company
3. Storage / Data Infrastructure Provider
4. Financial Institution / Fund
5. Telecom / Colocation Provider
6. Sovereign Entity / Government
7. GPU Operations Specialist

### 3.10 Decision Journal

**Purpose:** Audit trail for scoring changes over time.

**Columns:**

| Col | Content |
|-----|---------|
| A | Date |
| B | Analyst |
| C | Tab affected |
| D | Cell reference |
| E | Trend ID |
| F | Archetype |
| G | Dimension changed (Exp / Ali / EQ / Probability / New trend / Retired) |
| H | Old value |
| I | New value |
| J | Rationale |
| K | Evidence source |

Generator pre-populates headers and formatting. Users append rows manually as they update scores.

### 3.11 Methodology

**Purpose:** Formal rubrics for thesis methodology chapter and appendix.

**Content (plain text in merged cells, formatted for readability):**

1. Scoring formula: `Cell Score = Exposure × Alignment × (EQ ÷ 3)`
2. Exposure rubric (0-3) with decision tree
3. Alignment rubric (-2 to +2) with decision tree
4. Evidence Quality rubric (1-3) with discount rationale
5. Concentration risk formula (HHI)
6. Time-discount methodology
7. Trend interaction methodology
8. Reproducibility standard
9. Three worked examples:
   - Energy-First × S-1 Energy Wall (high Exp, high Ali, high EQ)
   - Aggregator × M-4 Multi-Cloud Abstraction (high Exp, negative Ali)
   - Infrastructure Enabler × S-6 Neocloud Bifurcation (medium EQ discount)

### 3.12 Expansion Guide

**Purpose:** How to maintain and evolve the tool.

**Content sections:**

1. **Recommended cadence** — 6-month and 12-month review cycles
2. **Adding a new trend** — step-by-step with config file edits
3. **Retiring a trend** — preserve history, zero out exposure
4. **Adding a new archetype** — config edits + stack weight assignment
5. **Merging overlapping trends** — correlation threshold and merge protocol
6. **Updating scores** — EQ-first principle
7. **Semi-annual review protocol** — the 4-phase checklist (Evidence refresh → Trend assessment → Scoring update → Validation)
8. **Source registry** — 10+ key sources with URLs and what to look for
9. **Version history** — changelog

---

## 4. Formula Specification

### 4.1 Named ranges

The generator creates Excel named ranges for clean cross-tab references:

| Named Range | Scope | Points to |
|-------------|-------|-----------|
| `Trends_All` | Workbook | Trend Definitions!A2:A19 |
| `Trends_TimeDiscount` | Workbook | Trend Definitions!G2:G19 |
| `Trends_P_Consensus` | Workbook | Scenario Weights!E8:E25 |
| `Trends_P_Disruption` | Workbook | Scenario Weights!F8:F25 |
| `Trends_P_Regression` | Workbook | Scenario Weights!G8:G25 |
| `Trends_Impact_Consensus` | Workbook | Scenario Weights!H8:H25 |
| `Trends_Impact_Disruption` | Workbook | Scenario Weights!I8:I25 |
| `Trends_Impact_Regression` | Workbook | Scenario Weights!J8:J25 |
| `SA_StackCoverage` | Workbook | Self-Assessment!E11:E42 |
| `SA_Resources` | Workbook | Self-Assessment!D45:D56 |
| `SA_Capabilities` | Workbook | Self-Assessment!D59:D64 |
| `Arch_{A1-A7}_StackWeights` | Workbook | Stack Taxonomy!H2:H33 (per archetype column) |
| `Arch_{A1-A7}_ResourceWeights` | Workbook | Archetype Profiles!I{row}:T{row} |
| `Scores_{A1-A7}` | Workbook | Scoring Matrix score columns per archetype |

### 4.2 Core formulas

**Cell Score (Scoring Matrix):**
```excel
=C{r}*D{r}*(E{r}/3)
```

**Archetype TRS (Scoring Matrix total row):**
```excel
=SUM(score_column_range)
```

**Time-Discounted TRS (Dashboard):**
```excel
=SUMPRODUCT(Scores_A1, Trends_TimeDiscount)
```

**Scenario-Weighted TRS (Dashboard):**
```excel
=SUMPRODUCT(Scores_A1, Trends_P_Consensus, Trends_Impact_Consensus)
```

**Concentration Risk HHI (Dashboard):**
```excel
=SUMPRODUCT(IF(Scores_A1>0, (Scores_A1/SUMPRODUCT((Scores_A1>0)*Scores_A1))^2, 0))
```
(Entered as array formula with Ctrl+Shift+Enter, or using SUMPRODUCT trick for non-CSE)

**Archetype Fit Score (Self-Assessment):**
```excel
Stack_Fit = SUMPRODUCT(SA_StackCoverage, Arch_A1_StackWeights)
          / SUMPRODUCT(IF(Arch_A1_StackWeights>0, 3, 0), Arch_A1_StackWeights)
```

**Gap identification (Self-Assessment):**
```excel
=IF(AND(Arch_A1_StackWeights_i >= 2, SA_StackCoverage_i <= 1), "GAP", "")
```

### 4.3 Conditional formatting rules

Applied by the generator to all relevant ranges:

| Tab | Range | Rule |
|-----|-------|------|
| Scoring Matrix | Score columns | >3 → green fill; <-3 → red fill |
| Dashboard | TRS columns | Color scale green-white-red |
| Dashboard | HHI column | >0.25 → orange; 0.15-0.25 → yellow; <0.15 → green |
| Self-Assessment | Fit % | >70% → green; 50-70% → yellow; <50% → red |
| Self-Assessment | Gap column | "GAP" → red fill |
| Trend Definitions | Status | "Retired" → gray fill, strikethrough |

---

## 5. Generator Script Architecture

### 5.1 Project structure

```
gpu_industry/strategy/tool/
├── configs/
│   ├── trends.json
│   ├── archetypes.json
│   ├── scores.json
│   ├── scenarios.json
│   ├── stack_taxonomy.json
│   ├── resources.json
│   ├── trend_interactions.json
│   └── playbook.json
├── generator/
│   ├── __init__.py
│   ├── build_workbook.py        # CLI entry point
│   ├── config_loader.py         # Reads + validates JSON configs
│   ├── named_ranges.py          # Creates and manages named ranges
│   ├── styles.py                # Color palette, fonts, borders, fills
│   └── tabs/
│       ├── __init__.py
│       ├── dashboard.py
│       ├── self_assessment.py
│       ├── scoring_matrix.py
│       ├── trend_definitions.py
│       ├── archetype_profiles.py
│       ├── stack_taxonomy.py
│       ├── scenario_weights.py
│       ├── trend_interactions.py
│       ├── resource_playbook.py
│       ├── decision_journal.py
│       ├── methodology.py
│       └── expansion_guide.py
├── output/
│   └── strategic_fitness_scoring_matrix.xlsx
├── tests/
│   └── test_formulas.py         # Validates formula correctness
└── README.md
```

### 5.2 Config file schemas

**trends.json:**
```json
[
  {
    "id": "S-1",
    "category": "Supply-Side",
    "name": "Energy wall",
    "description": "Power procurement > silicon as long-term binding constraint.",
    "key_evidence": "Cushman & Wakefield 2025; Goldman Sachs 160% DC power surge by 2030",
    "time_horizon": "Structural",
    "probabilities": {
      "consensus": 0.90,
      "disruption": 0.95,
      "regression": 0.60
    },
    "scoring_questions": {
      "exposure": "Does this archetype own/operate physical DC infrastructure? Is power a top-3 cost driver?",
      "alignment": "Does the trend create entry barriers that protect this archetype? Can it monetize energy scarcity?"
    },
    "status": "Active",
    "last_updated": "2026-06-21"
  }
]
```

**scores.json:**
```json
{
  "S-1": {
    "A1": {"exposure": 3, "alignment": 1, "evidence_quality": 3},
    "A2": {"exposure": 2, "alignment": 1, "evidence_quality": 3},
    ...
  }
}
```

**stack_taxonomy.json:**
```json
[
  {
    "id": "L1.1",
    "layer": "L1",
    "name": "Data center ownership or long-term lease",
    "description": "Physical facility housing GPU clusters",
    "examples": "Equinix colo, self-built DC, modular containerized DC",
    "acquisition_difficulty": 3,
    "difficulty_rationale": "Requires capital, permits, power contracts. 18-36 month lead time.",
    "archetype_weights": {
      "A1": 3, "A2": 2, "A3": 0, "A4": 0, "A5": 0, "A6": 3, "A7": 0
    }
  }
]
```

### 5.3 Build process

```bash
cd gpu_industry/strategy/tool
python -m generator.build_workbook

# Options:
#   --output path/to/output.xlsx    (default: output/strategic_fitness_scoring_matrix.xlsx)
#   --validate-only                 (check configs without generating)
#   --no-formatting                 (skip conditional formatting for speed)
```

The generator:
1. Loads and validates all config files against schemas
2. Creates workbook with tabs in order
3. Populates reference data (trends, archetypes, stack, interactions)
4. Writes scoring matrix with input values + formulas
5. Creates Self-Assessment with dropdowns and output formulas
6. Builds Dashboard with cross-tab formulas
7. Creates named ranges
8. Applies conditional formatting
9. Sets column widths and print areas
10. Saves to output path

### 5.4 Dependencies

- Python 3.9+
- openpyxl >= 3.1.0 (only dependency)
- No other packages required

---

## 6. Data Population

### 6.1 Pre-populated data

The v1 workbook ships with:
- 18 trends fully defined with evidence and scoring questions
- 7 archetypes with profiles and resource weights
- 126 scored cells (7×18) with Exposure, Alignment, Evidence Quality
- ~30 stack components with acquisition difficulty and archetype weights
- 15 trend interaction pairs
- 7 entrant profiles in the Resource Playbook
- 3 worked examples in Methodology

### 6.2 Data sources for initial population

- Scoring Matrix: carried over from `strategy/scoring_matrix.html` (validated in prior session)
- Trend Definitions: from `manual_notes/pres3_scenario_trend_synthesis.md` + deep research output
- Archetype Profiles: from prior session analysis + `gemini_research_outputs/GPU_cloud_bm_trajectories.md`
- Stack Taxonomy: from `data_files/market_positioning.json` value chain ownership structure + competitor research (pending agent re-run)
- Trend Interactions: new analysis based on trend definitions

---

## 7. Validation & Testing

### 7.1 Formula validation

`tests/test_formulas.py` generates the workbook, opens it with openpyxl in read-only mode, and verifies:
- All Score cells contain formulas (not static values)
- TRS totals = sum of score cells
- Named ranges resolve correctly
- Data validation lists match config entries
- Conditional formatting rules are applied to correct ranges

### 7.2 Analytical validation

- Composite TRS rank order matches the validated rankings from prior session:
  1. Full-Stack Neocloud (+45 raw)
  2. Infrastructure Enabler (+35)
  3. Orchestrator/PaaS (+27)
  4. Energy-First Provider (+25)
  5. AI Factory (+19)
  6. Aggregator (+8)
  7. Compute Financier (+8)
- HHI values produce sensible concentration readings
- Self-Assessment fit scores produce the expected archetype recommendation for each of the 7 reference entrant profiles

---

## 8. Limitations & Future Work

### 8.1 Known limitations (v1)

- No VBA or macros — all logic in cell formulas. Complex conditional text in Self-Assessment uses nested IF chains (limited to ~7 conditions in standard Excel).
- Trend interactions are limited to pairwise effects. No three-way or cascading interactions modeled.
- Self-Assessment fit score weights (50% stack, 30% resources, 20% capabilities) are fixed. Future versions could make these adjustable.
- No live data feeds. All evidence is point-in-time, updated via the semi-annual review protocol.

### 8.2 v1.1 candidates

- Sensitivity analysis tab: "Which 3 assumptions, if wrong by ±1, would most change this archetype's rank?"
- Archetype transition paths: visual map of natural evolution pathways
- Market sizing overlay: TAM/SAM estimates per archetype for economic context
- Export to web app (Approach C upgrade path): same config files, React/Streamlit renderer

### 8.3 Upgrade path to Approach C (web app)

The config-driven architecture is designed for this. The same `configs/` directory would feed a web renderer instead of openpyxl. The formula logic would move to JavaScript/Python compute functions. The Self-Assessment becomes an interactive wizard. Real-time data feeds could update trend probabilities automatically.
