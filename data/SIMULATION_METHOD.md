# Synthetic simulation methodology

This is an independent educational **Amazon-style** fulfillment-center model, not Amazon operational data. The proposal specifies Option B: reproducible Python simulation.

- Random seed: 42 (fixed; the current demand profile is deterministic).
- 100 scenarios: 10 staffing levels (30–75 in steps of 5) × 10 expedited shares (0–45% in steps of 5).
- Eight hourly arrivals: 145, 170, 200, 225, 245, 255, 240, 215 orders. The same arrivals are used for every scenario.
- Workers: 7.5 paid productive hours per 8-hour shift (0.5-hour break), $22/hour, 4.2 regular orders/worker/productive hour. Breaks are staggered in the simplified daily capacity model.
- Expedited orders: min(daily orders × share, 280 daily expedited-lane slots). Expediting costs $2.80/order.
- Effective capacity: regular capacity × (1 − 0.12 × expedited share fraction) + 0.65 × expedited orders.
- On-time percentage: clip(100 × effective capacity / daily orders − max(0, (peak hourly arrivals − workers × 4.2) × 0.018), 0, 99.4). This is a *proxy*, not a discrete-event simulation of actual deadline timestamps.
- Daily cost: workers × 7.5 × $22 + expedited orders × $2.80.
- Workstation capacity, scheduling, hourly expedited limits, budget and service threshold are **not applied to the Phase 1 frontier**. They are intended for Phase 2 constraint filtering; the present model's 280 expedited-order limit is an operational saturation assumption.

These assumed rates and demand patterns are illustrative, not validated against Amazon warehouse measurements. Published Amazon sources motivate the business problem but do not establish the numerical parameters. The model is reproducible, internally checked, and should be calibrated using actual operational data before any practical use.
