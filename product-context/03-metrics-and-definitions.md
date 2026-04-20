# Metrics & Definitions — FleetMate

This document defines the **canonical metrics** used for FleetMate initiatives.
All initiatives must reference metrics **exactly as defined here**.

---

## Core adoption metrics

### Monthly Active Drivers (MAD)
- Definition: Unique drivers who opened the FleetMate driver app at least once in a calendar month
- Platform: iOS + Android
- Use: Reach, top-of-funnel, driver base size

### Monthly Active Fleets (MAF)
- Definition: Unique fleet accounts with at least one active dispatcher or owner session in a calendar month
- Use: Core business health metric; subscription base proxy

### Driver-to-Fleet Link Rate
- Definition: % of active drivers connected to a paying fleet subscription
- Use: Measures network effect and monetization leverage

---

## Quality & reliability metrics

### Crash-Free Rate (CFR)
- Definition: % of app sessions (driver app) without a crash
- Guardrail metric: must be maintained ≥ 99.2% for all releases
- Platform: iOS + Android

### Navigation Accuracy Rate
- Definition: % of completed navigation sessions without a reported route error or rerouting complaint
- Use: Quality signal for the routing engine

### Offline Session Success Rate
- Definition: % of navigation sessions started in offline mode that completed without forcing the driver to abort
- Use: Reliability of offline functionality; key metric for offline initiatives

---

## Engagement metrics

### Trips Dispatched per Active Fleet (weekly)
- Definition: Average number of trips assigned by dispatchers per active fleet account per week
- Use: Product stickiness; measures operational adoption

### Driver App Sessions per Active Driver (weekly)
- Definition: Average number of sessions opened by an active driver per week
- Use: Depth of daily driver engagement

### Route Completion Rate
- Definition: % of dispatched trips where the driver reached the final destination as recorded in FleetMate
- Use: Measures end-to-end execution success

---

## Monetization metrics

### Trial-to-Paid Conversion Rate
- Definition: % of fleet accounts that convert from free trial to a paid subscription within 30 days
- Use: Growth lever; top commercial metric

### Monthly Recurring Revenue (MRR)
- Definition: Sum of all active paid fleet subscriptions normalized to monthly value
- Use: Business health; reported monthly

### Average Revenue Per Fleet (ARPF)
- Definition: MRR / number of paying fleet accounts
- Use: Monetization efficiency; tracks plan mix and upsell performance

### Monthly Churn Rate
- Definition: % of paying fleet accounts that cancel or lapse in a given month
- Use: Retention health

---

## Customer satisfaction

### App Store Rating
- Definition: Weighted average rating (iOS App Store + Google Play) in the last 90 days
- Use: Lagging quality signal; guardrail for driver-facing initiatives

### Fleet NPS
- Definition: Net Promoter Score collected quarterly from paying fleet owners
- Use: Loyalty and strategic satisfaction of B2B customers

---

## Measurement principles

- Prefer **leading indicators** in initiative success metrics
- Always define **baseline + target + measurement date** in initiatives
- CFR is a **hard guardrail** — any release dropping below 99.2% triggers rollback
- Use exact metric names from this document in all artifacts
