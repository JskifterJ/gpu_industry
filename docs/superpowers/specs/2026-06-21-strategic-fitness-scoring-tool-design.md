# Strategic Fitness Scoring Tool — Technical Specification

**Date:** 2026-06-21
**Purpose:** Python-generated Excel workbook that scores GPU cloud business model archetypes against industry trends, with a dynamic self-assessment feature for market entrants.
**Thesis context:** EPFL Master's thesis contribution — a generalizable strategic playbook grounded in Resource-Based View (Barney, 1991; Wernerfelt, 1984), Dynamic Capabilities (Teece et al., 1997; Eisenhardt & Martin, 2000), Resource Orchestration (Sirmon et al., 2007), Ecosystem Theory (Jacobides et al., 2018), Transaction Cost Economics (Williamson, 1985), and MCDA methodology (Saaty, 1980; Hwang & Yoon, 1981). Not ICN-specific.
**Architecture:** Approach B — Python generator script reads JSON configs, produces `.xlsx` with live formulas, conditional formatting, and data validation.
**Methodological rigor:** VRIO operationalization per archetype (§3.5.1), inter-rater reliability protocol with Krippendorff's α ≥ 0.80 threshold (§3.11.1), weight sensitivity analysis via tornado + Monte Carlo (§3.11.2), MCDA-grounded EQ discount formula (§3.11.4).

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
| 3 | Scoring Matrix | 7×19 matrix with Exp/Ali/EQ per cell, formula-computed scores | Editable (update scores directly) |
| 4 | Trend Definitions | 19 trends with metadata, evidence, probability weights, scoring questions | Reference + editable probabilities |
| 5 | Archetype Profiles | 7 archetypes with stack requirements, resources, exemplars | Reference |
| 6 | Stack Taxonomy | ~30 L0-L7 components with per-archetype importance weights and acquisition difficulty | Reference |
| 7 | Scenario Weights | Per-trend probabilities and impact weights for 3 scenarios + time-discount multipliers | Editable |
| 8 | Trend Interactions | Top 19 trend pairs with reinforcement/cancellation effects (+1/−1) | Reference + editable |
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
- Subtitle: "Resource-Based Playbook for Market Entrants — 7 Archetypes × 19 Industry Trends"
- Grounding: "Barney 1991 (VRIO), Teece et al. 1997 (Dynamic Capabilities), Saaty 1980 (AHP/MCDA), ClusterMAX 2.0, Hydra Host bifurcation thesis"
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
Stack_Fit = Σ(posture_match_score_i) / Σ(max_possible_match_i)
```

Where `posture_match_score` compares user's actual posture against archetype's recommended posture:
- Exact match (e.g., recommended=Own, actual=Own): 3 points
- One level off (e.g., recommended=Own, actual=Control): 2 points
- Two levels off (e.g., recommended=Own, actual=Use): 1 point
- Mismatch (e.g., recommended=Own, actual=Outsource or N/A): 0 points
- Recommended=N/A: excluded from calculation (not penalized)

Over-investment penalty: If recommended=Outsource but actual=Own, flag as capital-inefficient but still score 2 (partial credit — owning is not wrong, just suboptimal).

```
Resource_Fit = SUMPRODUCT(user_resource_values, archetype_resource_weights)
             / SUMPRODUCT(max_resource_values, archetype_resource_weights)

Overall_Fit = (Stack_Fit × 0.5) + (Resource_Fit × 0.3) + (Capability_Fit × 0.2)
```

Displayed as:
| Archetype | Stack Fit % | Resource Fit % | Capability Fit % | Overall Fit % | Rank | Flags |

Where Flags column shows:
- "Over-invested in N components" (owning things the archetype should outsource)
- "Under-invested in N components" (outsourcing things the archetype should own)

Conditional formatting: top 2 archetypes highlighted green.

**E2. Gap Analysis (rows 78-95)**

For the top-ranked archetype, list all stack components where:
- Archetype recommended posture = Own or Partner AND
- User actual posture = Outsource or N/A

Each gap row shows:
| Component | Recommended posture | Actual posture | Gap type (Under/Over) | Acquisition difficulty (1-3) | Bridgeable? |

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

**Purpose:** The core analytical engine — 7 archetypes × 19 trends.

**Layout:**

- Row 1: Archetype names (merged across 4 sub-columns each)
- Row 2: Sub-headers — Exp | Ali | EQ | Score — repeated 7 times
- Column A: Trend ID
- Column B: Trend name
- Rows 3+: Data rows, grouped by category with section headers (Supply-Side: S-1 to S-6, Demand-Side: D-1 to D-6, Market Structure: M-1 to M-6 + R-1)
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

**Scoring protocol:** Scorers assign Alignment values using the VRIO-to-Alignment decision rule (§3.11.3). Each Alignment score must be traceable to at least one VRIO row in §3.5.1, documented in the Decision Journal. Evidence Quality assignment follows the inter-rater reliability protocol (§3.11.1) — all EQ values are provisional until α ≥ 0.80 is achieved.

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

**Purpose:** Master reference for all 19 trends (S-1 through S-6, D-1 through D-6, M-1 through M-6, R-1). Feeds the Scoring Matrix and Scenario Weights.

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
| M | Hyperscaler Implication | How AWS/Azure/GCP decisions amplify or dampen this trend for non-hyperscaler entrants |
| N | Citation Keys | Comma-separated keys referencing Source Registry (e.g., "CW2025, SA-CM2") |
| O | Last Updated | Date of most recent evidence refresh |
| P | Status | Active / Under Review / Retired |

**Hyperscaler Implication examples:**

| Trend | Hyperscaler Implication |
|---|---|
| S-1 Energy wall | Google/MSFT/Amazon securing GW-scale nuclear + solar PPAs. Raises the energy bar for entrants but validates the energy-first model. |
| S-6 Neocloud bifurcation | Hyperscalers rent neocloud capacity as sustained supply diversification (not temporary bridge) while building own ASIC capacity in parallel. Creates durable demand floor for well-capitalized neoclouds. |
| M-3 ASIC diversification | Google TPU, AWS Trainium, MSFT Maia accelerate multi-silicon adoption. Reduces NVIDIA pricing power, benefiting orchestrators who support multi-silicon. |
| D-3 Enterprise ML proliferation | Hyperscaler managed ML platforms (SageMaker, Vertex) are primary competition for Full-Stack Neoclouds in enterprise segment. |
| M-1 Orchestration commoditization | Hyperscaler K8s services (EKS, GKE, AKS) commoditize orchestration from above; open-source Soperator commoditizes from below. |

**Scope delimitation:** This framework evaluates market entry strategies for non-hyperscaler GPU cloud participants. Hyperscaler behavior is modeled as an environmental input — the single largest exogenous variable affecting trend probabilities and archetype viability — not as a competing archetype. Hyperscalers span L0-L7 with fundamentally different economics (platform lock-in, cross-subsidization, trillion-dollar balance sheets) that make direct archetype comparison methodologically unsound. This delimitation follows Jacobides et al.'s (2018) ecosystem analysis: hyperscalers occupy the "keystone" position in the GPU compute ecosystem, while non-hyperscaler entrants compete for complementor or niche-keystone positions at specific stack layers. Williamson's (1985) transaction cost framework explains the vertical integration logic: high asset specificity (custom silicon, long-term power contracts, proprietary networking) pushes hyperscalers toward full integration, while non-hyperscaler entrants make selective make-or-buy decisions at each layer.

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

#### 3.5.1 VRIO Analysis per Archetype

**Purpose:** Operationalize Barney's (1991) RBV framework by mapping each archetype's key resources to the VRIO criteria (Value, Rarity, Imitability, Organization). An academic reviewer requires this to validate the theoretical link between RBV and the scoring model. Each archetype's competitive position depends on which resources pass all four VRIO tests — resources that pass V+R+I but fail O represent unrealized potential; resources that pass only V are competitive necessities, not advantages (Barney, 1991, pp. 105–112).

**VRIO scoring:** Y (Yes) / N (No) per criterion. Competitive implication follows Barney's logic:
- V only → Competitive parity
- V+R → Temporary advantage
- V+R+I → Sustained advantage (potential)
- V+R+I+O → Sustained competitive advantage (realized)

**VRIO mapping (key resources per archetype):**

| Archetype | Key Resource | V | R | I | O | Implication | Evidence |
|-----------|-------------|---|---|---|---|-------------|----------|
| **A1 AI Factory** | Proprietary DLC systems (Valvey/Racky) | Y | Y | Y | Y | Sustained advantage | CoreWeave 96% goodput, custom RLCC |
| A1 | NVIDIA first-allocation relationship | Y | Y | N | Y | Temporary advantage | First to GA GB200/GB300; replicable by others with capital |
| A1 | Bare-metal K8s (no hypervisor) | Y | N | N | Y | Parity | Open-source stack, any entrant can replicate |
| **A2 Full-Stack Neocloud** | Vertical integration (L1-L5) | Y | Y | Y | Y | Sustained advantage | Nebius: custom storage 442 GB/s write, Papyrax 1.5-2.5x faster |
| A2 | Proprietary inference engine (Token Factory/Papyrax) | Y | Y | Y | Y | Sustained advantage | NOT open-sourced; 50-60% KV-cache hit rate |
| A2 | Energy asset (renewable PPA) | Y | Y | Y | N | Unrealized potential | Nebius Iceland 100% renewable but not yet monetized as differentiator |
| **A3 Orchestrator/PaaS** | Multi-cloud abstraction layer | Y | N | N | Y | Parity | SkyPilot 100+ contributors; open-source commoditizes this |
| A3 | Developer ecosystem/marketplace | Y | Y | N | Y | Temporary advantage | Network effects provide window; imitable with scale |
| **A4 Aggregator/Clearing House** | Supply-side aggregation (multi-provider) | Y | Y | N | Y | Temporary advantage | Liquidity network effects; imitable once volume achieved |
| A4 | Pricing/matching algorithms | Y | N | N | Y | Parity | Standard optimization; no proprietary moat |
| **A5 Compute Financier** | GPU-backed financial instruments (ABS/futures) | Y | Y | Y | Y | Sustained advantage | IREN 3.31% A-rated; ICE/Ornn futures; 1300→110 bps compression |
| A5 | Residual value models | Y | Y | Y | N | Unrealized potential | No standardized depreciation (Amazon 5yr vs Meta 6yr) |
| **A6 Energy-First Provider** | Power procurement (PPA/self-generation) | Y | Y | Y | Y | Sustained advantage | 7-10yr grid lead times; behind-the-meter generation |
| A6 | DC site control (land + permits) | Y | Y | Y | Y | Sustained advantage | ERCOT 410GW queue; xAI DOJ override is exceptional |
| **A7 Infrastructure Enabler** | Specialized tooling (monitoring/observability) | Y | N | N | Y | Parity | Narrow and replicable |
| A7 | Cross-provider integration partnerships | Y | Y | N | Y | Temporary advantage | Relationship-based; not structurally defensible |

**Theoretical link to scoring model:** Resources that achieve V+R+I+O produce high Alignment scores (+2) against trends they address, because competitors cannot replicate the advantage. Resources at V+R (temporary advantage) produce moderate Alignment (+1) with a time-discount since the advantage erodes. Resources at V-only (parity) produce zero Alignment — they track the trend but don't create differentiation. This mapping grounds every Alignment score in Barney's VRIO logic rather than subjective judgment alone.

**Dynamic capabilities overlay (Teece et al., 1997; Eisenhardt & Martin, 2000):** Static VRIO assessment is necessary but insufficient in GPU cloud, where GPU generations turn over every 18 months and inference architectures change quarterly. Each archetype's Organization (O) score must be re-evaluated against its ability to reconfigure resources across silicon transitions (H100→Blackwell→Rubin), inference paradigm shifts (monolithic→disaggregated), and workload evolution (training→inference→agentic). Sirmon et al.'s (2007) resource orchestration framework — structure, bundle, leverage — provides the operational test for O: providers who restructure their bundles quickly score O=Y; those locked into prior-generation configurations score O=N.

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
| H-N | Per-archetype **recommended posture** — 7 columns, one per archetype |

**Recommended posture values per component per archetype:**
- **Own** (3): Core to the archetype's value proposition. Outsourcing this undermines competitive position.
- **Partner** (2): Important but acquirable through strategic partnerships. Owning is optional; reliable access is essential.
- **Outsource** (1): Not a differentiator for this archetype. Owning is capital-inefficient.
- **N/A** (0): Irrelevant to this archetype's business model.

The Self-Assessment compares the user's *actual* posture against the *recommended* posture for their target archetype. Mismatches flag in both directions:
- **Under-investment**: Outsourcing something the archetype should own (strategic vulnerability)
- **Over-investment**: Owning something the archetype should outsource (capital inefficiency)

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
  L1.6  Storage hardware (block / file / object)       Difficulty: 1

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
  L5.5  Storage architecture (block, file, object,    Difficulty: 2
         checkpoint, KV offload tiers)
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

**Pre-populated probability defaults (all 19 trends):**

| Trend | Horizon | P(Con) | P(Dis) | P(Reg) | Rationale |
|-------|---------|--------|--------|--------|-----------|
| S-1 Energy Wall | Structural | 0.90 | 0.95 | 0.60 | ERCOT 410GW queue, 7-10yr grid lead times. Near-certain in all scenarios; regression only if nuclear/SMR accelerates beyond current timelines. |
| S-2 Silicon Supply Cycles | Near-term | 0.85 | 0.80 | 0.50 | B200→Rubin transition underway. Cyclical by nature; disruption if ASIC alternatives mature faster. |
| S-3 GPU Lifecycle & Secondary | Medium | 0.80 | 0.70 | 0.40 | H100 secondary market established ($18-25K). Regression if rapid depreciation kills residual value. |
| S-4 CPU-GPU Rebalancing | Near-term | 0.75 | 0.85 | 0.35 | AWS agentic stack on Graviton; JP Morgan projects 7:1 CPU:GPU. Disruption likely as agentic workloads scale. |
| S-5 KV Cache & Context Infra | Near-term | 0.80 | 0.85 | 0.30 | Crusoe MemoryAlloy 9.9x TTFT; NVIDIA ICMSP standardizes KV offload. Low regression probability; KV demand only grows with context windows. |
| S-6 Neocloud Bifurcation | Medium | 0.85 | 0.90 | 0.40 | IREN 3.31% A-rated debt; CoreWeave $99.4B RPO. Disruption if M&A consolidation accelerates. |
| D-1 Inference Dominance | Structural | 0.90 | 0.95 | 0.20 | 33%→67% inference share in 3 years. Structural shift; regression near-impossible barring AI winter. |
| D-2 Agentic & Robotics | Medium | 0.70 | 0.85 | 0.30 | Unitree 10K humanoids, Claude Code/Cursor adoption. High disruption probability; still early for consensus certainty. |
| D-3 Enterprise ML Proliferation | Near-term | 0.85 | 0.80 | 0.40 | 72% enterprises with AI in production. Regression if ROI disappointment triggers enterprise pullback. |
| D-4 Open-Weight Economics | Structural | 0.85 | 0.90 | 0.25 | GLM-5.2 (MIT) at $2.40/task vs Fable $31. Structural trajectory; Fable export ban further accelerates. |
| D-5 Edge Distribution | Medium | 0.65 | 0.80 | 0.45 | 8B models at 30+ tok/s on phones. Lower consensus confidence; 80% edge inference projection may be aggressive. |
| D-6 Hardware Jevons Paradox | Structural | 0.80 | 0.85 | 0.35 | GPT-4→GPT-4o 100x cheaper, 1000x usage. Core thesis for sustained GPU demand. |
| M-1 Orchestration Commoditization | Near-term | 0.85 | 0.80 | 0.30 | Soperator 395 stars; CoreWeave supports 3 orchestration paths. Low regression; commoditization rarely reverses. |
| M-2 Inference Stratification | Near-term | 0.80 | 0.85 | 0.35 | Frontier tier ($5-25/Mtok) vs commodity ($0.05-0.90/Mtok). 60-80% price collapse in 12 months. |
| M-3 ASIC Diversification | Medium | 0.70 | 0.80 | 0.40 | TPU v6 4.7x v5e; Trainium2 ~$1/hr vs H100 $3/hr. Medium consensus; NVIDIA moat still strong. |
| M-4 Multi-Cloud Abstraction | Medium | 0.65 | 0.75 | 0.45 | SkyPilot 100+ contributors; disaggregated inference production-ready. Higher regression risk from hyperscaler lock-in. |
| M-5 Supply-Chain Security | Structural | 0.80 | 0.85 | 0.30 | CVE-2025-23266 container escape; NVBleed side-channel. Regulatory requirements only tighten. |
| M-6 Financialization of Compute | Medium | 0.75 | 0.85 | 0.40 | ICE/Ornn GPU futures; ABS spreads 1300→110 bps. Regression if GPU residual value uncertainty increases. |
| R-1 Regulatory & Geo Fragmentation | Structural | 0.85 | 0.90 | 0.30 | Fable export ban, xAI DOJ override, EU AI Act. Geopolitical fragmentation accelerates across all scenarios. |

**Section C: Scenario Definitions (rows 35+)**

Prose definitions of each scenario for context:
- **Consensus:** Industry trends continue on current trajectory. Most probable baseline.
- **Disruption:** Accelerated change -- ASIC diversification faster, neocloud consolidation deeper, energy wall higher, agentic workloads mature faster.
- **Regression:** Slowdown -- enterprise AI adoption stalls, GPU demand growth decelerates, financing tightens, open-weight momentum slows.

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
| R-1 Regulatory fragmentation | D-4 Open-weight economics | +1 | Export bans on frontier closed models (Fable ban) push adoption toward open-weight alternatives |
| R-1 Regulatory fragmentation | M-5 Supply-chain security | +1 | Jurisdictional compliance requirements raise the security/compliance bar for all providers |
| R-1 Regulatory fragmentation | D-3 Enterprise ML proliferation | -1 | Regulatory uncertainty slows enterprise AI adoption in heavily regulated verticals |
| R-1 Regulatory fragmentation | S-6 Neocloud bifurcation | +1 | Multi-jurisdictional compliance costs accelerate financial bifurcation among neoclouds |

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
4. Evidence Quality rubric (1-3) with MCDA discount rationale (see §3.11.4)
5. Concentration risk formula (HHI)
6. Time-discount methodology
7. Trend interaction methodology
8. Reproducibility standard
9. Three worked examples:
   - Energy-First × S-1 Energy Wall (high Exp, high Ali, high EQ)
   - Aggregator × M-4 Multi-Cloud Abstraction (high Exp, negative Ali)
   - Infrastructure Enabler × S-6 Neocloud Bifurcation (medium EQ discount)
10. VRIO-to-Alignment mapping protocol (see §3.5.1)
11. Inter-rater reliability protocol (see §3.11.1)
12. Weight sensitivity analysis (see §3.11.2)
13. Scoring differentiation logic (see §3.11.5)

#### 3.11.5 Scoring Differentiation Logic

**Purpose:** Guide scorers on how each trend produces different Exposure and Alignment patterns across archetypes, ensuring the 133-cell matrix produces meaningful rank-order separation rather than uniform scores.

**Principle:** A trend that scores the same across all 7 archetypes has zero discriminating power and should be reconsidered. Each trend should produce at least 3 distinct Exposure levels and a mix of positive/negative Alignments.

**Per-trend scoring guidance:**

| Trend | High Exposure (2-3) | Low Exposure (0-1) | Positive Alignment | Negative Alignment | Key Discriminator |
|-------|--------------------|--------------------|--------------------|--------------------|-------------------|
| S-1 Energy Wall | A1 AI Factory, A2 Full-Stack, A6 Energy-First | A3 Orchestrator, A4 Aggregator, A5 Financier | A6 (energy IS the moat), A1 (who can procure wins) | A4 (aggregates others' energy risk) | Do you own physical infrastructure? |
| S-2 Silicon Supply | A1, A2 (deploy GPUs directly) | A3, A4, A5 (don't touch silicon) | A1 (first-allocation advantage), A2 (multi-gen fleet) | A4 (supply volatility disrupts marketplace liquidity) | Do you procure GPUs at scale? |
| S-3 GPU Lifecycle | A4 Aggregator, A5 Financier | A3, A6 | A5 (residual value models are the product), A4 (secondary supply = marketplace inventory) | A1 (depreciation risk on balance sheet) | Do you profit from GPU aging? |
| S-4 CPU-GPU Rebalance | A2, A3 (run mixed workloads) | A5, A6, A7 | A3 (orchestrate heterogeneous compute), A2 (full-stack includes CPU) | A1 (pure GPU focus, less CPU capability) | Can you serve CPU-heavy agentic workloads? |
| S-5 KV Cache | A1, A2 (operate inference infra) | A5, A6 | A2 (proprietary KV systems like MemoryAlloy/Token Factory), A1 (cluster-scale KV fabric) | A4 (commoditized infra can't differentiate on KV) | Do you have proprietary inference memory tech? |
| S-6 Neocloud Bifurcation | A1, A2, A5, A6 (capital-intensive) | A3, A7 | A5 (financial instruments determine survival), A6 (energy assets as collateral) | A4 (undercapitalized marketplace risk), A7 (enablers get acquired) | Do you have a capitalization moat? |
| D-1 Inference Dominance | A1, A2, A3 (serve inference) | A5, A6, A7 | A2 (full-stack optimized for inference), A3 (route inference demand) | A1 (training-optimized clusters less efficient for inference) | Is your infrastructure optimized for inference? |
| D-2 Agentic & Robotics | A2, A3 (platform for agentic) | A5, A6 | A3 (orchestrate long-running agentic sessions), A2 (CPU+GPU+storage integrated) | A4 (spot/marketplace incompatible with stateful agentic), A6 (energy-only, no platform) | Can you serve stateful, long-running workloads? |
| D-3 Enterprise ML | A2, A3 (serve enterprises) | A5, A6 | A2 (compliance + full stack), A3 (developer tools + APIs) | A4 (Docker-only isolation = enterprise ceiling) | Do you have SOC 2 / enterprise compliance? |
| D-4 Open-Weight | A2, A3, A4 (host open models) | A5, A6 | A4 (open models = more marketplace demand), A3 (model-neutral platform benefits) | A2 (proprietary inference advantages erode if models commoditize) | Does cheaper inference help or hurt your model? |
| D-5 Edge Distribution | A3 (multi-cloud including edge) | A1, A5, A6 | A3 (orchestrate cloud-edge continuum), A7 (tooling for edge deployment) | A1 (pure cloud, no edge), A2 (full-stack cloud, loses demand to edge) | Do you span cloud and edge? |
| D-6 Jevons Paradox | All (macro demand driver) | None at 0 | A1 (more demand = more GPUs needed), A2 (volume × margin), A6 (demand validates energy investment) | None strongly negative (rising tide) | Uniformly positive; discriminates on magnitude only |
| M-1 Orchestration Commoditization | A3 Orchestrator (existential) | A5, A6 | A2 (commoditized orchestration reduces competitor moat), A4 (open orchestration helps aggregation) | A3 (orchestration IS the product; commoditization destroys it) | Is orchestration your core product? |
| M-2 Inference Stratification | A2, A3, A4 (price-sensitive) | A5, A6, A7 | A4 (stratification creates arbitrage), A2 (can compete in premium tier) | A3 (caught between tiers), A1 (no pricing flexibility in bulk contracts) | Can you serve multiple price tiers? |
| M-3 ASIC Diversification | A2, A3 (multi-silicon valuable) | A5, A6 | A3 (multi-silicon abstraction = product), A4 (more silicon options = more marketplace supply) | A1 (NVIDIA-only bet threatened by ASIC alternatives) | Are you locked into NVIDIA? |
| M-4 Multi-Cloud Abstraction | A3 (core product), A4 | A1, A5, A6 | A3 (multi-cloud IS the product), A4 (aggregation across clouds) | A1 (locked to own infrastructure), A2 (vertical integration resists abstraction) | Do you benefit from workload portability? |
| M-5 Supply-Chain Security | A2, A3 (serve enterprises) | A4, A7 | A2 (compliance as moat), A1 (bare-metal isolation = security advantage) | A4 (Docker-only = security ceiling), A7 (lacks resources for certification) | Do you have enterprise security certifications? |
| M-6 Financialization | A5 (existential), A1, A6 | A3, A7 | A5 (financial instruments ARE the product), A6 (energy assets as financeable collateral) | A3 (software-only, nothing to financialize), A7 (too small to access capital markets) | Do you have hard assets to financialize? |
| R-1 Regulatory Fragmentation | A2 (multi-jurisdiction), A5 | A7 | A6 (national security framing benefits energy/infra owners), A2 (jurisdiction-specific compliance = moat) | A4 (cross-border aggregation complicated), A3 (multi-cloud across jurisdictions adds compliance burden) | Can you navigate jurisdiction-specific regulation? |

#### 3.11.1 Inter-Rater Reliability Protocol

**Purpose:** Establish that the scoring dimensions (Exposure, Alignment, Evidence Quality) produce consistent results across independent raters, addressing the methodological requirement for reliability in qualitative-to-ordinal coding (Krippendorff, 2004).

**Protocol:**

1. **Rater selection:** Minimum 2 raters, recommended 3. At least one rater with industry domain expertise (GPU cloud, data center infrastructure); at least one with strategic management / RBV academic background. The thesis author is rater 1; the thesis advisor serves as calibration authority.

2. **Training phase:** All raters jointly score a training set of 10 cells (selected to cover the full range: high/low Exposure, positive/negative Alignment, EQ 1-3). During training, raters discuss disagreements and align on rubric interpretation. Training scores are excluded from reliability calculations.

3. **Independent coding phase:** Each rater independently scores a reliability sample of minimum 21 cells (~17% of the 126-cell matrix). The sample is stratified: 7 cells per trend category (Supply/Demand/Market), ensuring at least one cell per archetype. Each rater scores all three dimensions (Exposure, Alignment, EQ) per cell.

4. **Reliability coefficients:**
   - **Krippendorff's alpha (α):** Primary reliability measure. Handles ordinal data, missing values, and 2+ raters (Krippendorff, 2004, pp. 221–243). Threshold: α ≥ 0.80 for each dimension independently. If α ≥ 0.667 but < 0.80, the dimension is flagged as "tentative" and findings are qualified. If α < 0.667, the rubric for that dimension is revised and the coding phase is repeated.
   - **Cohen's kappa (κ):** Reported for each rater pair as supplementary measure. Kappa ≥ 0.61 indicates "substantial agreement" (Landis & Koch, 1977).
   - **Percent agreement:** Reported but not used as the primary measure (inflated by chance agreement on concentrated scales).

5. **Disagreement resolution:** Cells where raters diverge by ≥2 points on any dimension are flagged for consensus discussion. The final score is the post-discussion consensus value, documented in the Decision Journal with the original rater scores noted.

6. **Reporting in thesis:** The thesis methodology chapter reports: (a) number of raters, (b) sample size and selection method, (c) α and κ values per dimension, (d) number and nature of disagreements resolved, (e) rubric revisions if any.

**Citation chain:** Krippendorff, K. (2004). *Content analysis: An introduction to its methodology* (2nd ed.). Sage. — Cohen's kappa: Cohen, J. (1960). A coefficient of agreement for nominal scales. *Educational and Psychological Measurement*, 20(1), 37–46. — Landis, J. R., & Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1), 159–174.

#### 3.11.2 Weight Sensitivity Analysis

**Purpose:** Demonstrate that the framework's rank-order conclusions are robust to reasonable perturbation of assumed weights, addressing the academic concern that the Self-Assessment weights (50% stack, 30% resources, 20% capabilities) and scenario probability weights lack empirical justification.

**Approach: Tornado Analysis + Monte Carlo Simulation**

**A. Tornado analysis (deterministic):**

For each weight parameter, vary ±20% from default while holding all others constant. Parameters tested:
- Self-Assessment: Stack fit weight (0.40–0.60), Resource fit weight (0.24–0.36), Capability fit weight (0.16–0.24)
- Scenario Weights: P(Consensus), P(Disruption), P(Regression) per trend
- Time-discount multipliers: Near (0.80–1.00), Medium (0.64–0.96), Structural (0.48–0.72)
- Interaction multiplier (0.40–0.60)

For each perturbation, record the change in archetype rank order and the magnitude of composite TRS shift. The tornado diagram shows which parameters produce the largest rank-order changes — these are the "swing assumptions" that analysts should scrutinize most carefully.

**B. Monte Carlo simulation (probabilistic):**

Run 10,000 iterations sampling all weight parameters from uniform distributions within ±20% of defaults. For each iteration, compute all 7 archetype TRS values and record the rank order. Output:
- Probability that each archetype holds its base-case rank (rank stability %)
- 90% confidence interval on each archetype's composite TRS
- Identification of "rank-fragile" archetypes (rank stability < 70%)

**Implementation:** The Monte Carlo runs in the Python generator as a validation step (`--sensitivity` flag). Results are written to a Sensitivity Analysis section in the Methodology tab as a summary table. The full 10,000-iteration output is saved as a CSV alongside the workbook for thesis appendix.

**AHP precedent (Saaty, 1980):** The Analytic Hierarchy Process literature establishes that composite scores derived from subjective pairwise comparisons require sensitivity analysis to establish that conclusions survive reasonable weight perturbation. The ±20% range reflects the standard AHP sensitivity envelope (Saaty, 1980, Ch. 7). Analysts who lack confidence in specific weights should widen the range and re-run.

#### 3.11.3 VRIO-to-Scoring Protocol

**Purpose:** Ensure every Alignment score in the Scoring Matrix is traceable to a VRIO assessment (§3.5.1), not subjective judgment alone.

**Decision rule:**
- Resource passes V+R+I+O → Alignment = +2 against trends the resource directly addresses
- Resource passes V+R → Alignment = +1 (advantage exists but erodes)
- Resource passes V only → Alignment = 0 (competitive necessity, no differentiation)
- Resource fails V → Alignment = -1 or -2 (misalignment with trend direction)

Scorers document which VRIO row(s) support each Alignment score in the Decision Journal (col K: Evidence source). This creates a formal audit trail from RBV theory → VRIO assessment → cell score.

#### 3.11.4 EQ Discount Formula: MCDA Precedent

**Purpose:** Provide academic justification for the Evidence Quality discount formula (EQ ÷ 3), addressing the reviewer's concern that the ÷3 normalization appears ad hoc.

**Justification:**

The cell score formula `Exposure × Alignment × (EQ ÷ 3)` is a **multiplicative composite** in the MCDA tradition. The EQ term functions as a **confidence discount** that reduces the influence of poorly-evidenced scores on the composite TRS.

**Why multiplicative, not additive:** In multi-attribute value theory (MAVT), multiplicative aggregation is preferred when criteria are **not mutually preferentially independent** (Eisenführ, Weber & Langer, 2010). In this framework, Exposure and Alignment are not preferentially independent of Evidence Quality — a high-exposure, well-aligned score based on inference alone (EQ=1) should carry less weight than the same score backed by primary data (EQ=3). The multiplicative structure enforces this: EQ=1 produces a ⅓ discount; EQ=3 produces no discount.

**AHP parallel (Saaty, 1980):** AHP composite scores multiply criteria weights by priority scores, producing a single index. The EQ÷3 term functions identically to a normalized weight in AHP — it scales the base score by the analyst's confidence in the underlying evidence. The ÷3 normalizer maps EQ to the unit interval [0.33, 1.0], consistent with AHP's requirement that weights sum or normalize within bounded ranges.

**TOPSIS parallel (Hwang & Yoon, 1981):** TOPSIS ranks alternatives by weighted distance from ideal and anti-ideal solutions. The EQ discount is analogous to the TOPSIS weight vector — it adjusts the contribution of each criterion-score to the composite distance. Low-EQ cells contribute less to the composite, equivalent to a TOPSIS weight reduction for uncertain criteria.

**Alternative discounts considered and rejected:**
- EQ÷4 (range 0.25–0.75): Over-penalizes moderate evidence; a score with EQ=2 (sourced but secondary) receives only 50% weight
- Binary (EQ≥2 = full weight, EQ=1 = zero): Loses granularity; treats "secondary source" identically to "primary verified data"
- Logarithmic (ln(EQ+1)/ln(4)): Compresses the scale; difference between EQ=2 and EQ=3 becomes negligible
- No discount (ignore EQ): Fails to address evidence uncertainty, defeats the purpose of collecting EQ data

The ÷3 normalizer produces the most discriminating and theoretically defensible mapping: EQ=1 → 0.33 (strong discount for inference-only), EQ=2 → 0.67 (moderate confidence for secondary sources), EQ=3 → 1.0 (full confidence for primary data).

**Citation chain:** Saaty, T. L. (1980). *The analytic hierarchy process*. McGraw-Hill. — Hwang, C.-L., & Yoon, K. (1981). *Multiple attribute decision making*. Springer-Verlag. — Eisenführ, F., Weber, M., & Langer, T. (2010). *Rational decision making*. Springer-Verlag.

### 3.12 Source Registry

**Purpose:** Formal citation index enabling traceability from any score back to its evidence chain. Each trend's "Citation Keys" column (Trend Definitions, col N) references entries here.

**Columns:**

| Col | Header |
|-----|--------|
| A | Citation Key (e.g., CW2025, SA-CM2, GS-PWR) |
| B | Source Type (Report / Filing / Article / Podcast / Dataset / Interview) |
| C | Author / Organization |
| D | Title |
| E | Date Published |
| F | URL or location |
| G | Trends Supported (comma-separated trend IDs this source is evidence for) |
| H | Key Data Points (specific numbers or claims extracted) |
| I | Last Verified (date someone confirmed the source is still accessible and accurate) |

**Pre-populated sources (initial registry):**

| Key | Source | Trends |
|-----|--------|--------|
| CW-10K | CoreWeave 10-K FY2025 | S-6, M-6 |
| SA-CM2 | SemiAnalysis ClusterMAX 2.0 | All (benchmarking baseline) |
| GS-PWR | Goldman Sachs "Powering the AI Era" | S-1, D-6 |
| CW-2025 | Cushman & Wakefield DC Market 2025 | S-1 |
| HH-BIF | Ariel Deschapell "AI Factories vs Neoclouds" | S-6, M-1, M-2 |
| HH-GRD | Ariel Deschapell "Compute Grading" | S-3 |
| NEB-FIN | Nebius $4.3B raise press release | S-6 |
| GLM-51 | Z.ai GLM-5.1 release | D-2, S-4 |
| GEM-4 | Google DeepMind Gemma 4 release | D-4, D-5 |
| NIST-AI | NIST AI Action Plan | M-5, S-3 |
| LITELLM | LiteLLM PyPI attack post-mortem | M-5 |
| SACRA | Sacra neocloud revenue reports | S-6, M-2 |
| EPO-AI | Epoch AI scaling & capacity reports | D-6, D-1 |
| SIL-DAT | Silicon Data GPU secondary market | S-2, S-3 |
| STR-NAD | Stratechery Nadella interview (2026) | Hyperscaler context |
| BAR-91 | Barney, J. B. (1991). Firm resources and sustained competitive advantage. *Journal of Management*, 17(1), 99–120. DOI: 10.1177/014920639101700108 | Framework: VRIO, all archetypes |
| WER-84 | Wernerfelt, B. (1984). A resource-based view of the firm. *Strategic Management Journal*, 5(2), 171–180. DOI: 10.1002/smj.4250050207 | Framework: RBV foundation |
| TPS-97 | Teece, D. J., Pisano, G., & Shuen, A. (1997). Dynamic capabilities and strategic management. *SMJ*, 18(7), 509–533. DOI: 10.1002/SICI1097-0266(199708)18:7<509::AID-SMJ882>3.0.CO;2-Z | Framework: Dynamic capabilities |
| EM-00 | Eisenhardt, K. M., & Martin, J. A. (2000). Dynamic capabilities: What are they? *SMJ*, 21(10–11), 1105–1121. DOI: 10.1002/1097-0266(200010/11)21:10/11<1105::AID-SMJ133>3.0.CO;2-E | Framework: Operational dynamic capabilities |
| SHI-07 | Sirmon, D. G., Hitt, M. A., & Ireland, R. D. (2007). Managing firm resources in dynamic environments. *AMR*, 32(1), 273–292. DOI: 10.5465/AMR.2007.23466005 | Framework: Resource orchestration |
| JCG-18 | Jacobides, M. G., Cennamo, C., & Gawer, A. (2018). Towards a theory of ecosystems. *SMJ*, 39(8), 2255–2276. DOI: 10.1002/smj.2904 | Framework: Ecosystem/stack position |
| WIL-85 | Williamson, O. E. (1985). *The economic institutions of capitalism*. Free Press. ISBN: 978-0-02-934820-6 | Framework: Make-or-buy, asset specificity |
| SAA-80 | Saaty, T. L. (1980). *The analytic hierarchy process*. McGraw-Hill. ISBN: 978-0-07-054371-3 | Framework: AHP/MCDA scoring precedent |
| HY-81 | Hwang, C.-L., & Yoon, K. (1981). *Multiple attribute decision making*. Springer-Verlag. | Framework: TOPSIS, multiplicative scoring |
| KRI-04 | Krippendorff, K. (2004). *Content analysis* (2nd ed.). Sage. | Methodology: Inter-rater reliability |

**Evidence chain requirement:** Every cell with Evidence Quality = 2 or 3 must have at least one citation key in the corresponding trend's Citation Keys field. EQ = 1 (inference only) does not require a citation but should note the reasoning basis. Academic framework citations (BAR-91, TPS-97, SAA-80, etc.) support the *methodology*, not individual cell scores — they are referenced from the Methodology tab, VRIO table (§3.5.1), and sensitivity analysis (§3.11.2).

### 3.13 Expansion Guide

**Purpose:** How to maintain and evolve the tool.

**Content sections:**

1. **Recommended cadence** — 6-month and 12-month review cycles
2. **Adding a new trend** — step-by-step with config file edits
3. **Retiring a trend** — preserve history, zero out exposure
4. **Adding a new archetype** — config edits + stack weight assignment
5. **Merging overlapping trends** — correlation threshold and merge protocol
6. **Updating scores** — EQ-first principle
7. **Semi-annual review protocol** — the 4-phase checklist (Evidence refresh → Trend assessment → Scoring update → Validation)
8. **Source registry maintenance** — how to add new sources, verify existing ones, retire stale ones
9. **Version history** — changelog

---

## 4. Formula Specification

### 4.1 Named ranges

The generator creates Excel named ranges for clean cross-tab references:

| Named Range | Scope | Points to |
|-------------|-------|-----------|
| `Trends_All` | Workbook | Trend Definitions!A2:A20 |
| `Trends_TimeDiscount` | Workbook | Trend Definitions!G2:G20 |
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
│   ├── playbook.json
│   └── sources.json             # Citation registry
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
│       ├── source_registry.py
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
    "hyperscaler_implication": "Google/MSFT/Amazon securing GW-scale nuclear + solar PPAs. Raises the energy bar for entrants but validates the energy-first model.",
    "citation_keys": ["CW-2025", "GS-PWR"],
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
    "archetype_posture": {
      "A1": "Own",      "A2": "Partner",  "A3": "N/A",
      "A4": "N/A",      "A5": "N/A",      "A6": "Own",
      "A7": "N/A"
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
#   --sensitivity                   (run §3.11.2 tornado + Monte Carlo; writes sensitivity_results.csv)
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
- 19 trends fully defined with evidence and scoring questions
- 7 archetypes with profiles and resource weights
- 133 scored cells (7×19) with Exposure, Alignment, Evidence Quality
- ~30 stack components with acquisition difficulty and archetype weights
- 19 trend interaction pairs
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
- Self-Assessment fit score weights (50% stack, 30% resources, 20% capabilities) are fixed defaults derived from industry weighting logic (stack is the largest capital commitment, capabilities are the most transferable). The ±20% tornado analysis (§3.11.2) tests robustness to this assumption; the Monte Carlo simulation quantifies rank stability across the full weight space. Future versions could allow user-adjustable weights with AHP pairwise-comparison input (Saaty, 1980).
- No live data feeds. All evidence is point-in-time, updated via the semi-annual review protocol.
- Inter-rater reliability has been specified (§3.11.1) but not yet executed. The thesis must report actual α and κ values before the scoring matrix is finalized. Until inter-rater testing is complete, all scores carry a "provisional" qualifier.
- Infrastructure Enabler (A7) has the weakest exemplar base. Candidate exemplars: Weights & Biases (MLOps/experiment tracking), Determined AI (training orchestration), Modal (serverless compute platform), VAST Data (AI-optimized storage). These companies operate at specific stack layers rather than spanning the full archetype definition. A7 may naturally merge into A3 (Orchestrator/PaaS) as the market matures, or split into specialized sub-archetypes (MLOps Enabler, Storage Enabler, Observability Enabler). The current framework retains A7 as a catch-all for narrow-layer players who don't fit A1-A6, acknowledging the weak exemplar base.

### 8.2 R-1 Regulatory & Geopolitical Fragmentation (Included)

**Status:** Included as 19th trend in v1. Discriminating power test passed: R-1 and M-5 produce distinct scoring patterns across archetypes (see §3.11.5). Post-scoring validation gate: if the Pearson correlation between R-1 and M-5 archetype scores exceeds 0.85, merge them in v1.1.

**Evidence base (5 data points, EQ 2-3):**

1. **Fable/Mythos export ban (June 2026):** US government banned export of frontier AI models to adversary nations. Forces sovereign and non-allied entities toward open-weight models and domestic compute infrastructure. EQ: 3 (primary regulatory event, corroborated by CNBC/Axios/Politico).

2. **xAI DOJ national security override (2026):** DOJ intervened to defend xAI's 57 unpermitted gas turbines, arguing national security. Demonstrates that national security framing bypasses standard regulatory barriers. EQ: 3 (TechCrunch/DataCenterDynamics, DOJ filing).

3. **EU AI Act implementation (2024-2026):** High-risk AI system classification creates jurisdiction-specific compliance requirements. European data sovereignty constraints limit non-EU cloud options. EQ: 2 (regulatory text is primary, implementation effects are projected).

4. **China compute autonomy (ongoing):** Huawei Ascend 910B/C deployed as domestic NVIDIA alternative. ~30% of global AI compute demand behind a geopolitical wall. EQ: 2 (Huawei deployment confirmed, market share figures are analyst estimates).

5. **NIST AI RMF + FedRAMP:** US government AI workloads require FedRAMP authorization. Protected market segment with premium pricing for certified providers. EQ: 2 (regulatory framework is primary, market sizing is inferred).

**Trend definition:**

| Field | Value |
|-------|-------|
| ID | R-1 |
| Category | Market Structure |
| Name | Regulatory & Geopolitical Fragmentation |
| Description | Jurisdictional AI regulation (EU AI Act, US export controls, China compute autonomy) fragments the global GPU cloud market, creating protected regional segments and penalizing providers without multi-jurisdictional compliance. |
| Time Horizon | Structural (5-10yr) |
| P(Consensus) | 0.85 |
| P(Disruption) | 0.90 |
| P(Regression) | 0.30 |
| Scoring Q: Exposure | Does this archetype operate across multiple jurisdictions? Is regulatory compliance a top-3 operational concern? |
| Scoring Q: Alignment | Does jurisdictional fragmentation create protected markets this archetype can exploit? Or does it fragment the archetype's addressable market? |
| Hyperscaler Implication | AWS/Azure/GCP have FedRAMP, SOC 2, and multi-jurisdiction compliance by default. Neoclouds must invest heavily to match. Fragmentation benefits hyperscalers in regulated segments but creates protected niches for jurisdiction-specialist providers. |
| Citation Keys | AN-18, AN-19, BB-7, BB-16 |

**Interaction pairs** (added to §3.8):
- R-1 → D-4 (+1): Export bans push frontier closed-model users toward open-weight alternatives
- R-1 → M-5 (+1): Jurisdictional compliance requirements compound security/compliance demands
- R-1 → D-3 (-1): Regulatory uncertainty chills enterprise AI adoption in heavily regulated verticals
- R-1 → S-6 (+1): Multi-jurisdiction compliance costs accelerate neocloud financial bifurcation

### 8.3 v1.1 candidates

- Weight sensitivity tab with interactive tornado diagram (§3.11.2 Monte Carlo built into generator; v1.1 adds visual output in Excel)
- Archetype transition paths: visual map of natural evolution pathways
- Market sizing overlay: TAM/SAM estimates per archetype for economic context
- Export to web app (Approach C upgrade path): same config files, React/Streamlit renderer
- User-adjustable Self-Assessment weights via AHP pairwise comparison input
- R-1/M-5 correlation check: if Pearson r > 0.85 after full scoring, merge R-1 into M-5 as a sub-dimension

### 8.4 Upgrade path to Approach C (web app)

The config-driven architecture is designed for this. The same `configs/` directory would feed a web renderer instead of openpyxl. The formula logic would move to JavaScript/Python compute functions. The Self-Assessment becomes an interactive wizard. Real-time data feeds could update trend probabilities automatically.
