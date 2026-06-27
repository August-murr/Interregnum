# The Cost Curve of Total Surveillance
---

## The Bottleneck That's Disappearing

Mass 24/7 citizen surveillance has been a goal of authoritarian governments throughout history. It has always been self-defeating — not because of legal constraints, but because of a simple arithmetic problem: you need nearly as many watchers as watched. The Stasi employed one informant per 63 citizens. The ratio was still too thin to be comprehensive.

What's changing is that the human bottleneck is being replaced by a compute bottleneck. And unlike humans, compute follows a price curve.

Moore's Law was already a slow-motion promise to the surveillance state. GPU scaling has broken Moore's Law, dropping the cost of inference faster. The question is no longer *whether* mass automated surveillance becomes feasible — it's *when*, *for whom*, and *at what cost*.

---

## Project Goal

Produce a rigorous, quantitative estimate of:

1. **The current cost** of running 24/7 automated surveillance on a population of a given size
2. **The cost curve** — how fast that number is dropping and why
3. **Feasibility timelines** — when does it become practical for governments with different resource profiles?
4. **A country-specific breakdown** — accounting for wealth, technical capacity, existing infrastructure, and political will

The bar is not a warning essay. The bar is something precise enough to be cited in a policy brief or a parliament hearing.

---

## What the System Actually Looks Like

To estimate costs seriously, the architecture must be fully specified. This means going into detail on:

### Sensing Layer
- Street cameras 
- Audio capture infrastructure
- Mobile device data interception 
- Financial transaction monitoring
- Internet traffic interception 
- Smart device data

### Transport Layer
This is underappreciated. Surveillance at scale requires moving enormous volumes of raw data. A dense camera network in a mid-sized city can generate petabytes per day before any processing. This requires:
- Dedicated fiber backbone or reserved ISP bandwidth
- Edge preprocessing nodes to reduce transmission load
- Latency tolerances (real-time vs. batch surveillance have very different infrastructure requirements)

Some nations already have this as dual-use infrastructure. Others would need to build it from scratch, which dramatically changes the cost picture.
---

## The Cost Model

The goal is to produce a number: **cost per citizen per year** under a fully operational automated surveillance system. Then model how that number changes over time.

Key variables:
- Inference cost per GPU-hour (historical trend + projection)
- Data volume per citizen per day
- Compression and edge-compute offloading efficiency
- Storage cost trajectory
- Human-in-the-loop requirements (the system still flags; humans still act)

This should produce a graph comparable to METR's capability forecasts — a curve with confidence intervals, not a point estimate.

---

## Country Profiles

Feasibility is not uniform. A serious analysis needs to classify nations along at least:

- **GDP and defense/security budget** — raw financial capacity
- **Technical workforce** — can they build it, or must they buy it?
- **Existing infrastructure** — camera networks, fiber, state ISP control
- **Outsourcing appetite** — some states will pay a vendor (likely Chinese or US firms) rather than engineer in-house
- **Political will and legal friction** — democracies face different constraints than consolidated authoritarian states

Example profiles to model:
- **China** — already partially deployed; the interesting question is marginal cost of full coverage
- **A mid-income authoritarian state** — high will, limited capacity, likely to outsource
- **A wealthy liberal democracy** — high capacity, legal and political friction as the binding constraint, in the name of national security
- **A low-income state with foreign surveillance infrastructure** — the neo-colonial surveillance dynamic

---

## The Evasion Layer

A surveillance system is only as good as its ability to handle adversarial citizens. The cost of a *functional* surveillance system is higher than the cost of the infrastructure alone, because:

- Citizens learn to cover faces, spoof gait, use Faraday bags
- Encrypted communications shift the detection burden
- Decoy behaviors and coordinated evasion (as seen in Hong Kong protests)
- VPN usage, mesh networks, physical dead drops

Each of these shifts the cost curve upward. The project should model:
- Baseline system cost
- Cost uplift from adversarial population behavior at different evasion penetration rates
- Government counter-responses and their costs (legal coercion, hardware mandates, etc.)

---

## The Timeline Question

The central deliverable: **for each country profile, in what year does the annual cost of full-population surveillance fall below X% of the security budget?**

This requires:
- A cost projection model (grounded in GPU price trends, model efficiency scaling, storage cost curves)
- A budget model per country
- A threshold definition for "feasible" (politically meaningful, not just technically possible)

The output should look like a matrix: countries on one axis, years on the other, with a feasibility probability or cost-to-budget ratio in each cell.

---

## Why This Matters

This is not a dystopia essay. The value of this work is that it forces the question into quantitative territory, where vague warnings become actionable forecasts. Policymakers who dismiss surveillance concerns as hypothetical future problems should be confronted with a specific year and a specific price tag.

The goal is to be the METR report of democratic erosion risk — technically detailed, visually compelling, referenceable.

---

## Open Questions / Research Directions

- What is the current best public estimate of China's per-citizen surveillance cost?
- How do model efficiency improvements (smaller, faster vision models) affect the timeline?
- Is there a meaningful difference in cost between *recording everything* and *analyzing everything in real time*?
- At what evasion rate does a surveillance system become operationally useless?
- Who are the likely infrastructure vendors and what do their pricing structures look like?
