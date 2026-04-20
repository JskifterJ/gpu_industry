---
title: Compute Doesn't Need a Unit. It Needs a Grading System.
author: Ariel Deschapell
date: 2026-04-13
source_type: report
sphere: hardware
tags: [compute-measurement, heterogeneous-workloads, gpu-grading, standardisation, benchmarking]
relevance: medium
deck_slide: S6
---

https://notasithlord.substack.com/p/compute-doesnt-need-a-unit-it-needs
The Trump Administration’s AI Action Plan, released in July 2025, calls for the creation of financial markets for compute, modeled on commodity spot and forward markets, and puts NIST, OSTP, and NSF in the lead. NIST, the National Institute of Standards and Technology, sits within the Department of Commerce. If you have ever pumped gas, weighed produce, or traded a commodity in the United States, you have relied on NIST’s work without knowing it. NIST Handbook 44, first published in 1949 and updated annually for seventy-six consecutive years, specifies the tolerances and testing procedures for every commercial measuring device in the economy. Every commodity derivatives market in the country rests on measurement infrastructure NIST built: flow meters for natural gas, smart meters for electricity, volume and density standards for petroleum. The infrastructure is invisible precisely because it works.

NIST should play the same foundational role for compute markets. The question is what, exactly, they should standardize. The institutional instinct is right. Getting the answer wrong could set the market back years.

The stakes extend far beyond market efficiency. The AI Action Plan is meant to win a multi-trillion dollar race against China in what is the largest infrastructure buildup in human history. With Chinese manufacturing and energy production both surpassing the U.S. and Europe combined, it’s a formidable opponent. One that also has a weakness.

China’s state-directed model can command resources where needed, but it cannot tap global capital markets or build the risk-transfer mechanisms that accelerate private investment. The United States’ greatest structural advantage is not its energy output or even its chip supply but its financial markets. Compute priced and settled on U.S. exchanges, denominated in dollars, with delivery verified against Commerce Department standards, would extend dollar hegemony into the AI economy the same way dollar-denominated oil trading entrenched it in the energy economy. Those standards would also give U.S. and U.S.-aligned compute companies access to liquidity Chinese competitors do not have.

Getting the standardization infrastructure right is not an academic exercise. It is the foundation on which geopolitical strategic advantage gets built.

A Common Argument, and Why It Fails
A common argument, articulated well by David Friedman, holds that compute derivatives and advanced financial tooling can’t work until NIST creates a dimensionless, cross-hardware compute unit, the way MMBtu standardizes natural gas and MWh standardizes electricity. Define a weighted composite of floating-point throughput, memory bandwidth, interconnect bandwidth, and energy efficiency, and you get a universal “compute unit” that makes GPU hours fungible across hardware. The appeal is real: a cross-hardware unit would let you write forward contracts that survive hardware transitions, insulating buyers from rotation risk. This is the strongest version of the argument.

It’s also wrong, for three reasons.

First, compute is not homogeneous the way electricity is. A megawatt-hour from a coal plant and a megawatt-hour from a solar farm are physically identical at the bus bar. An H100 GPU-hour and a B200 GPU-hour are structurally different products with different memory architectures, different interconnect capabilities, different driver stacks, and different performance profiles. They cannot practically substitute for each other in most workloads. There is no meaningful conversion ratio that ubiquitously applies to all potential consumers.

Second, within a chip type, the problems multiply. A B300 GPU-hour from one provider is not the same as a B300 GPU-hour from another. The network interconnect topology determines whether you can run distributed training. The cooling capacity determines whether the GPU sustains its rated clock speed or throttles under load. The power redundancy determines whether the server stays on. A single composite score collapses exactly the information that real buyers need to assess workload and business compatibility. FLOPS, the most intuitive candidate for a universal unit, varies by numerical precision (FP8 is not FP64), tells you nothing about the infrastructure the chip runs in, and can’t account for memory bandwidth bottlenecks in inference workloads at all. It’s a useful metric for some comparisons, but not a commodity unit.

Third, and most fundamentally: a composite unit requires industry agreement on the parameters and their weights, and that agreement is impossible because what matters differs not just by company but by workload within a company. Some workloads are GPU-constrained, some memory-constrained, some storage-dependent, some bottlenecked by interconnect. A weighted composite that serves a distributed training run misprices an inference deployment. Every hardware vendor would lobby for weights favoring their architecture. Every revision would reprice every outstanding contract. The index administrator would become the most powerful actor in the market, a position fundamentally at odds with NIST’s role as a neutral standards body. Ultimately a commodity unit that cannot be meaningfully settled against is useless for finance, no matter how elegant it looks on paper. 6fusion’s Workload Allocation Cube and other attempts have gotten this exactly wrong.

Friedman himself has noted elsewhere that financial markets routinely handle heterogeneous underliers through deliverable baskets and adjustment factors, and that Treasury futures prove heterogeneity doesn’t block financialization. He’s right. The question is whether you solve heterogeneity at the measurement layer, by collapsing it into a single number, or at the contract layer, by grading the differences and letting the market price them.

The Better Precedent: Petroleum, Not Electricity
Crude oil solved this problem decades ago. Over 200 recognized grades trade globally. They vary by sulfur content, API gravity (density), and delivery location. Each dimension independently affects what a refinery can do with the oil, and therefore the price. NIST does not collapse them into a single number. NIST standardizes how each dimension is measured.




This is the right model for compute. It may in fact be the only practical one.

And unlike the universal compute unit, this model does not require inventing something new. The market has already converged on a de facto commodity unit: the GPU server hour, or simply GPU hour. Every neocloud, every broker, every pricing index already denominates in GPU hours. Most deals are referenced at dollars per GPU per hour. Nobody is trading WACs or composite compute units. The practical task for NIST is not to attempt to create a chimerical unit from scratch but to formalize the one already in use, define the grading dimensions around it, and build the measurement and attestation infrastructure that makes those grades trustworthy. This is exactly the pattern NIST has followed for virtually every commodity: the market establishes the practice, NIST standardizes the measurement.

But a GPU hour from one provider is not a GPU hour from another. Today not even a shared language exists for describing the difference. When buyers receive competing offers for “B200” server hours, they are comparing prices with no standardized way to assess the differing reliability and capability underneath. The grading system is what closes that gap. Compute infrastructure varies along at least three independent dimensions that matter for workload compatibility:

Components. The hardware itself: GPU model, memory capacity, thermal behavior, the OEM chassis used, memory and storage specs. This is roughly analogous to API gravity in crude oil, a measure of intrinsic quality. There are high quality B200 servers, and there are low quality ones. This has real practical impacts, from the performance and reliability customers can expect, to the longevity of the hardware.

Network. The interconnect topology and bandwidth: NVSwitch fabric for large scale distributed training, NVLink bridges for small-cluster work, PCIe for single-GPU inference. This is the hard capability boundary, analogous to sulfur content. Sweet crude and sour crude go to different refineries. A-network compute and C-network compute serve entirely different workloads. A buyer who needs full NVSwitch fabric for 256-GPU distributed training cannot use PCIe networking at any discount. No spread resolves a capability mismatch.

Datacenter. The facility: power redundancy, cooling capacity, physical security, uptime track record. This is analogous to delivery location in oil markets, a measure of reliability and accessibility.

A buyer looking at a grade like “ABA” knows immediately: top-tier hardware, NVLink interconnect (small-cluster capable, not premium frontier training), top-tier facility. That’s actionable. A single score of “82” or “3.7 WCU-hours” tells you almost nothing about whether the capacity fits your workload.

Each chip type constitutes a separate market, the way WTI and Brent are separate crude oil markets. H100 contracts and B300 contracts have independent order books and independent forward curves. As hardware ages out, its market thins naturally. As new hardware ships, a new market opens. You don’t need a “barrel of oil equivalent” to make oil markets work. You don’t need a universal compute unit to make compute markets work. The hardware rotation problem is unique but not disqualifying, and it’s ultimately a contract design problem, not a measurement problem.

Reasonable participants can disagree about where to draw the line between an “A” and a “B” component rating. That is a fundamentally different and more narrow kind of argument than “what goes into the unit.” Threshold debates converge because they are empirical: you can test whether a server sustains a given clock speed under an example work load. Definitional debates over composite weights do not converge, because every hardware vendor lobbies for weights favoring their architecture, every revision reprices outstanding contracts, and the index administrator becomes the most powerful actor in the market. The petroleum market embraced grading, and it worked.

What NIST Should Build
If NIST takes the AI Action Plan’s mandate seriously and looks at its own institutional precedent, the work product would look like what it has produced for every other commodity: not a synthetic unit, but a classification system, measurement standards, and verification procedures.

The foundation is a multi-dimensional grading taxonomy: defined dimensions (components, network interconnect, datacenter facility), grade levels within each dimension, and the thresholds that separate them. The methodology should be public and the thresholds should be debatable. What matters is that the industry converges on a shared language for describing compute quality, not that any particular set of thresholds is correct on the first pass.

That taxonomy needs measurement procedures behind it: standardized benchmark suites specifying how GPU performance is tested (which benchmarks, under what conditions, for how long), how network interconnect is characterized (bandwidth, latency, topology verification), and how datacenter facility quality is assessed. These are the compute equivalents of gas flow meter calibration standards. And because a grade is only as trustworthy as the process that verified it, the framework needs attestation standards: who can attest, what they must test, how results are recorded, how long attestation remains valid, and what triggers re-attestation.

Without independent verification, compute markets face a classic lemons problem: operators with subpar hardware overstate their grade, genuinely high-quality operators cannot credibly distinguish themselves, and the market degrades to the lowest common denominator. For compute, unlike physical commodities, the verification process can be largely programmatic. Software can provision servers, run benchmark suites, measure network topology, and produce cryptographically signed attestation records without human intervention. NIST would then certify the organizations performing attestation, the same way it trains state weights and measures officials who inspect gas pumps and grain scales.

NIST has a well-established pattern here. The Cybersecurity Framework codified practices that originated in industry. SP 800-145 standardized cloud computing definitions after the market had already converged on them. Handbook 44 incorporates industry experience into annually updated standards. The right approach for compute is the same: industry develops the grading taxonomy, benchmark suites, and attestation procedures through production use, and NIST formalizes them into authoritative standards once validated. Industry efforts to build multi-dimensional grading and programmatic attestation are already underway and generating production data, including at my own company, Hydra Host.

Note what NIST would not do under this model: set depreciation schedules (that’s IRS), mandate financial reporting (SEC), design contract terms (exchanges and CFTC), or prescribe collateral haircuts (bank regulators). NIST’s role is measurement. That confinement is precisely what gives it credibility across all the downstream domains.

The Stakes
The petroleum measurement standards work because they respect the heterogeneity of the underlying commodity. They don’t pretend all crude oil is the same. They give the market a shared language for describing how it’s different, and let market participants price those differences, build the infrastructure, and supply the liquidity.

Compute requires the same. The Department of Commerce built the measurement infrastructure that made the United States the center of global commodity markets. It can do the same with compute markets and the AI Economy.