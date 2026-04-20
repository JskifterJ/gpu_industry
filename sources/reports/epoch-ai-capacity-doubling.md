---
title: Global AI Computing Capacity is Doubling Every 7 Months
author: Epoch AI
date: 2025-01-15
source_type: report
url: https://www.epochai.org/
sphere: hardware
tags: [capacity-growth, scaling, training-compute, compute-doubling, exponential-growth]
relevance: high
deck_slide: S4, S6
---

# Global AI computing capacity is doubling every 7 months | Epoch AI

Data Insight

Jan. 9, 2026

# Global AI computing capacity is doubling every 7 months

Cite

By Josh You, Venkat Somala, Yafah Edelman, and Luke Emberson

Total available computing capacity from AI chips across all major designers has grown by approximately 3.3x per year since 2022, enabling larger-scale model development and consumer adoption. NVIDIA AI chips currently account for over 60% of total compute, with Google and Amazon making up much of the remainder.

![](/assets/images/data-insights/ai-chip-production.png)

These estimates are inferred based on revenue data, other financial disclosures, and analyst reports. Data coverage varies by manufacturer: Nvidia and Google data begin in 2022 while others start in 2024.

Epoch's work is free to use, distribute, and reproduce provided the source and authors are credited under the Creative Commons BY license.

## Learn more about this graph

We fit a trend to the quarterly quantities of AI compute sold, as measured in H100-equivalents. We find that computing capacity has been growing by 3.3x per year, equivalent to a doubling time of 7 months.

### Data

Our data comes from the [AI Chip Sales datahub](https://epoch.ai/data/ai-chip-sales). This data is primarily obtained via financial documents and chip information collected in the ML Hardware datahub. See the [AI Chip Sales documentation](https://epoch.ai/data/ai-chip-sales-documentation) for more details.

### Analysis

We fit a simple log-linear regression model to estimate an exponential trend in the number of H100 equivalents sold quarterly. 90% confidence intervals are obtained analytically, using standard errors on our estimate of the slope coefficient.

We estimate that global computing capacity has been growing by 3.3x per year since 2022 (90% CI: 2.7x to 4.1x). This is equivalent to a doubling time of 7 months (90% CI: 6 to 8 months).

### Limitations

While we have data for Nvidia as early as 2022Q1, and for Google beginning from 2022Q4, other chip manufacturer data only starts in 2024Q1. We do not attempt to correct for the missing data in our cumulative totals, since these other manufacturers account for less than 10% of total chip production as of 2024Q1, and we believe this quantity was smaller in earlier years. The effect on estimated growth rate is likely minimal.

Our data tracks chip sales, not deployments. Actual quantities of available compute may differ on the margin due to depreciation and lags between sales and deployment. We do not account for depreciation, but its effects are negligible since compute quantities are growing far faster than they depreciate.

## Explore this data

![AI Chip Sales](/assets/images/datahub/thumbnails/small/chip-sales-icon-circle.svg)

[](/data/ai-chip-sales)AI Chip Sales

AI chip sales data.

## Related insights

[

![The stock of computing power from NVIDIA chips is doubling every 10 months](/assets/images/data-insights/insight-gpu-depreciation-absolute.png)

Data Insight

Feb. 13, 2025

### The stock of computing power from NVIDIA chips is doubling every 10 months

](/data-insights/nvidia-chip-production)[

![The computational performance of leading AI supercomputers has doubled every nine months](/assets/images/data-insights/ai-supercomputers-performance-trend.png)

Data Insight

Updated Jun. 5, 2025

### The computational performance of leading AI supercomputers has doubled every nine months

](/data-insights/ai-supercomputers-performance-trend)

## Related topics

[Compute](/latest?topic=Compute)[Hardware](/latest?topic=Hardware)[Data Centers](/latest?topic=Data+Centers)

Feedback