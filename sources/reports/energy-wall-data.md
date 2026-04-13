---
title: Energy Wall — Comparable Numbers Research
author: Compiled from IEA, Goldman Sachs, EIA, NVIDIA specs
date: 2026-04-13
source_type: report
sphere: hardware
tags: [energy-wall, datacenter-power, household-comparison, nuclear, ppa, eia, iea, goldman-sachs]
relevance: high
deck_slide: S6 (Energy Wall)
---

# Energy Wall — Research Findings

*Compiled April 13, 2026 for pres3 S6 redesign.*

---

## Single GPU and Rack Power Draw

| Unit | Power |
|------|-------|
| H100 SXM5 (single GPU) | 700W |
| B200 SXM (single GPU) | 700W |
| GB200 Superchip (Grace + 2× B200) | ~1,200W per B200 in this config |
| GB200 NVL72 rack (full system) | **120–132 kW** (115 kW liquid + 17 kW air = 132 kW TDP) |
| GB300 NVL72 rack | ~120–132 kW |
| Vera Rubin NVL144 rack (2027 projected) | **~600 kW** |
| Traditional CPU rack (pre-AI baseline) | 8–12 kW |
| H100 DGX air-cooled rack | ~40 kW |

Use 120 kW for average-case slide comparisons; 132 kW for worst-case. Official TDP is 132 kW.

---

## "How Many Homes?" Calculation

**US average household:** 10,332 kWh/year → **1.18 kW continuous draw** (EIA FAQ)

| System | Power | Equivalent homes |
|--------|-------|-----------------|
| 1× B200 GPU | 700W | ~0.6 homes |
| GB200 NVL72 rack (120 kW) | 120 kW | **~102 homes** |
| GB200 NVL72 rack (132 kW) | 132 kW | **~112 homes** |
| Vera Rubin NVL144 (600 kW) | 600 kW | **~508 homes** |
| 100-rack GB200 cluster (12 MW) | 12 MW | **~10,170 homes** |
| Frontier training cluster (40 MW) | 40 MW | **~33,900 homes** |
| Frontier training cluster (100 MW) | 100 MW | **~84,700 homes** |

**Slide callout:** "One GB200 NVL72 rack consumes as much electricity as ~100 average American homes — continuously."

---

## Global Datacenter Electricity: Historical vs Projected

| Year | TWh | Notes |
|------|-----|-------|
| 2015 | ~220 TWh | Pre-cloud scale, CPU-dominated |
| 2020 | ~200–223 TWh | Efficiency gains offset cloud growth |
| 2022 | ~240 TWh est. | Post-COVID cloud surge |
| **2024** | **~415 TWh** | 1.5% of global electricity; 12%/yr CAGR last 5 years |
| 2030 (IEA Base Case) | **~945 TWh** | ~3% of global electricity |
| 2035 (IEA Base Case) | **~1,200 TWh** | |
| 2035 (IEA Lift-Off) | **~1,700 TWh** | ~4.4% of global electricity |

**Pre-AI profile shift:** In 2020, datacenter energy was dominated by storage, networking, and CPU compute with declining intensity (efficiency improvements offset growth). GPU AI workloads reversed the efficiency curve — sector now grows at 4× the rate of total global electricity demand.

**Accelerated servers' share:** GPU/AI servers account for almost half the net increase in global DC electricity 2024→2030, growing at **30% per year** vs. 9% for conventional servers.

---

## Goldman Sachs Projections

| Metric | Value |
|--------|-------|
| 2023 baseline global DC power | ~55 GW |
| 2027 forecast | ~92 GW (+67%) |
| 2030 forecast | +165–175% vs 2023 |
| CAGR 2025–2028 | 17% |
| Top 5 hyperscaler capex (2025–2026 combined) | $736 billion |

**McKinsey:** AI-related DC infrastructure needs **$5.2 trillion by 2030**; expected capacity of **156 GW** by 2030.

---

## Nuclear / Renewable Energy Deals (Scale of Power Purchase Agreements)

| Company | Asset | Capacity | Term |
|---------|-------|----------|------|
| Microsoft | Three Mile Island Unit 1 restart (Constellation) | 835 MW | 20 years |
| Amazon | Talen Energy Susquehanna campus, PA | 1,900 MW | Through 2042 |
| Amazon | SMR, Washington state | 320 MW → 960 MW option | — |
| Google | Kairos Power SMR fleet | 500 MW | 2030+ |
| Meta | Clinton Clean Energy Center, IL | 1,100 MW | 20 years |
| Meta | RFP for new nuclear | Up to 4,000 MW | 2030s |

**Total:** Big Tech contracted **10+ GW** of US nuclear capacity in one year.  
**2025 stat:** Amazon, Meta, Google, Microsoft = **49% of all global clean energy PPA volume** in 2025.

**Slide callout:** "Microsoft, Google, Amazon, and Meta contracted more nuclear power in 2025 than most countries generate. Power location strategy is now a primary competitive moat — above GPU procurement strategy."

---

## Key comparison for slide (pre-AI vs AI era framing)

| Era | DC Power Profile | Energy intensity |
|-----|-----------------|-----------------|
| 2015 (storage era) | CPU compute, storage arrays, networking | 200–250 kW/rack peak |
| 2020 (cloud era) | Virtualised CPU, modest GPU inference | 8–25 kW/rack typical |
| 2026 (AI era) | GPU AI training + inference, liquid cooling | **120–600 kW/rack** for AI racks |

A single GB200 NVL72 rack consumes more power than **10–15× a typical pre-AI server rack.** The datacenter's physical infrastructure (cooling, power distribution, UPS) must be redesigned from scratch to host AI-era density.

---

## Sources

- EIA household FAQ: https://www.eia.gov/tools/faqs/faq.php?id=97&t=3
- IEA Energy and AI — Energy Demand: https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai
- IEA AI surging datacenter demand: https://www.iea.org/news/ai-is-set-to-drive-surging-electricity-demand-from-data-centres
- Goldman Sachs 165% by 2030: https://www.goldmansachs.com/insights/articles/ai-to-drive-165-increase-in-data-center-power-demand-by-2030
- Tweaktown 132kW TDP: https://www.tweaktown.com/news/100839/nvidias-new-gb200-nvl72-ai-server-highest-power-consuming-in-history-with-132kw-tdp/index.html
- Introl Vera Rubin 600kW: https://introl.com/blog/nvidia-vera-rubin-gpu-600kw-racks-2027
- ESG Today Big Tech nuclear: https://www.esgtoday.com/amazon-meta-google-microsoft-account-for-half-of-global-clean-energy-purchase-deals-in-2025-report/
