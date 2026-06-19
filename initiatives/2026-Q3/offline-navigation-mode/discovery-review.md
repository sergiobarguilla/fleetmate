# Discovery Review — Offline Navigation Mode

## 1. Key Assumptions Identified

### Value
- "Navigation drops signal" complaints in churn surveys are primarily caused by *total connectivity loss* — and therefore solved by pre-downloaded offline mode (vs. reconnection failures, re-routing errors, or UI freezes during signal recovery, which would not be solved by this initiative)
- Delivering offline navigation will improve App Store rating from 4.1 to ≥ 4.5 — there is no analysis of what % of negative reviews specifically cite offline/connectivity as the primary failure
- Drivers who currently complain about signal loss will adopt the offline feature once available — complaint frequency does not equal feature adoption intent

### Usability
- Drivers will proactively download routes before departure — this is a new behavior with no prior habit, no in-app prompt today, and no research validation
- Drivers can reliably predict when they will need offline mode (i.e., they know their route will pass through low-connectivity zones)
- The download action can be completed safely before or at departure without requiring in-cab interaction while moving
- Low-end Android devices (the dominant hardware in CEE long-haul fleets) can support the download UI and offline engine without degraded performance

### Feasibility
- A full offline routing engine with truck-specific restrictions (height, weight, tonnage bans) can be delivered in 35 Mobile MDs within a single quarter — no spike has been completed
- The 150MB app size increase constraint is sufficient to contain meaningful geographic coverage for CEE long-haul routes with truck restriction data layers
- Offline map tile bundles + embedded routing engine are viable on the storage and RAM profiles of low-end Android devices common in target markets
- Conflict resolution for offline POD/sync (Phase 2) is a tractable problem solvable in 20 Backend MDs — no architecture work has begun

### Viability
- Growth-tier-only gating is commercially defensible when Sygic Truck (the primary comparable) offers offline on all tiers
- ≥ 40% of Starter fleets will upgrade to Growth tier if offline is a Growth-only feature — this conversion rate has no basis in current data
- Phase 1 technical delivery is sufficient to satisfy the German fleet account's renewal condition — what that account team specifically requires has not been formally scoped
- Year 2 revenue of €80k–€150k assumes 60–120 fleet upgrades; the current Starter fleet count and its eligibility for upgrade are not stated

---

## 2. Hypothesis Stress Test

**What works in the logic**
- The causal chain from connectivity failure → trip abort → driver frustration → negative reviews is the most evidenced part of the hypothesis: three consecutive quarters of survey data with a consistent #1 complaint is a genuine signal
- Using Offline Session Success Rate as primary KPI is correct — it is the only metric that directly measures the stated outcome
- The German fleet renewal creates a real, time-bound commercial forcing function that de-risks the "why now" entirely
- The initiative is aligned with two named strategic themes (Reliability First, Offline Resilience) and directly targets an active OKR (App Store Rating ≥ 4.5)

**What is weak or unproven**
- The Offline Session Success Rate baseline is zero — no offline capability exists, so any non-zero result exceeds baseline. The 90% target is stated without any benchmark: there is no reference for what "good" looks like for first-generation offline navigation on the target device profile
- App Store rating improvement is asserted, not modelled: the hypothesis jumps from "offline feature delivered" → "+0.4 rating" with no analysis of what share of current negative reviews are specifically attributable to connectivity failures
- The hypothesis conflates *feature delivery* with *feature adoption* — if drivers do not proactively download routes (the unvalidated behavior), the Offline Session Success Rate metric will show near-zero offline sessions rather than ≥ 90% success, rendering the KPI unmeasurable
- The 6-month measurement window is ambiguous: Phase 1 launches end of Q3 2026, 6 months lands in Q1 2027 — the same quarter Phase 3 begins. It is unclear whether the 90% target is intended for Phase 1 capability alone or the full 3-phase initiative

**Hidden dependencies**
- The entire hypothesis depends on drivers *adopting* the feature. Without a validated activation model (manual download vs. auto-trigger vs. smart prompt), adoption could be too low to produce statistically meaningful Offline Session Success Rate data
- App Store rating improvement is additionally dependent on negative reviewers returning to re-rate — a well-known inertia problem that makes rating recovery slow even after the underlying issue is resolved
- The German fleet renewal is a commercial dependency distinct from the product KPIs — its resolution depends on what the account team has specifically committed to the customer, which is not documented
- Phase 2 sync quality is required for the feature to be trustworthy in operations (dispatchers need visibility of offline drivers), yet Phase 1 success metrics are defined without Phase 2 scope — a driver completing a trip offline but with delayed reconciliation may still generate a negative experience

---

## 3. Top Risks (Ranked)

1. **Usability risk: Drivers will not proactively download routes**
   - Why it matters: Pre-download is the only activation model described. If drivers do not initiate downloads before departure, the feature exists but is unused. The entire Offline Session Success Rate metric collapses — either from lack of attempts (good success rate on very few sessions) or from drivers attempting offline navigation on un-downloaded routes (failure rate spikes). Research confirms drivers already operate under time pressure and cognitive load; adding a manual pre-trip step requires a strong UX trigger or an automatic model.
   - What would invalidate it: A prototype test showing < 50% of drivers initiate a route download when given the option, or showing that drivers consistently forget / deprioritise the step under realistic departure conditions.
   - Consequence if ignored: Feature ships, adoption is low, Offline Session Success Rate metric is not meaningful, App Store rating does not move, German fleet renewal is secured but broader KPI targets are missed. Initiative declared a technical success and product failure.

2. **Feasibility risk: Offline routing engine cannot meet device and size constraints**
   - Why it matters: 35 Mobile MDs is a significant commitment with zero architecture validation. Truck-specific routing data (height/weight restrictions, hazmat layers, road type exclusions) is substantially more complex and larger than standard road routing. CEE long-haul routes span multiple countries with different restriction datasets. Low-end Android devices (common in target segment — budget Samsungs, older Xiaomi devices) have constrained RAM and storage. If the engine cannot be made to fit within 150MB and function reliably on low-end hardware, Phase 1 scope must be cut or re-architected — with direct risk to the Q3 delivery deadline and the German fleet renewal.
   - What would invalidate it: Architecture spike showing that a truck-capable offline routing engine exceeds 150MB for representative CEE route coverage, or that crash rates on low-end Android devices exceed the CFR guardrail under offline conditions.
   - Consequence if ignored: Engineering begins on an architecture that must be abandoned mid-quarter. Q3 deadline is missed. German fleet renewal is lost. CFR guardrail is breached on launch.

3. **Viability risk: Growth-tier gating is commercially untenable**
   - Why it matters: The entire Year 2–3 revenue case (€80k–€400k) depends on offline being a Growth-tier-only feature driving upsell. Sygic Truck — the named competitor — offers offline on all tiers. If fleet owners and commercial teams view offline as a baseline expectation rather than a premium differentiator, gating it will generate commercial friction and may be reversed post-launch, eliminating the upsell revenue model. The 40% Starter → Growth upgrade assumption has no supporting data.
   - What would invalidate it: Commercial team analysis showing that < 20% of Starter fleets express willingness to upgrade for offline, or that the German fleet (and accounts like it) are on Growth tier already — meaning the upsell opportunity is concentrated in a smaller addressable pool than projected.
   - Consequence if ignored: Initiative ships with Growth-only gating, generates commercial complaints from Starter fleets, gating is reversed under pressure, Year 2 revenue model disappears, initiative is repriced as a retention feature with no upsell contribution.

---

## 4. Decision Points

> Decisions that must be made before PRD. These are choices between mutually exclusive options, not assumptions to validate.

### DP-1: What tier(s) will offline navigation be available on?
- **Options:** (a) Growth tier only — offline as premium upsell driver (b) All paid tiers (Starter + Growth) — offline as retention feature, no upsell (c) All tiers including free driver app — offline as acquisition driver for fleet owner conversion
- **What depends on this:** Entire Year 2–3 revenue model; commercial positioning vs. Sygic Truck; whether Starter fleet churn due to missing offline capability is the primary risk or secondary; metrics hierarchy (MAF growth vs. ARPF growth)
- **Current default:** Growth-only (stated in birth certificate constraints)
- **How to inform:** Commercial team analysis of current Starter fleet offline demand signals; competitive pricing review; willingness-to-pay signal from 5–10 Starter fleet owner conversations
- **Decide by:** Before PRD

### DP-2: What is the offline routing engine technology?
- **Options:** (a) Embedded HERE SDK — established truck routing, licensing cost, larger binary footprint (b) OSM offline bundles — open source, lower cost, less reliable truck restriction accuracy in CEE (c) Custom tile server with client-side rendering — most control, highest build cost and timeline risk
- **What depends on this:** 150MB constraint feasibility; Opex (licensing vs. infrastructure); truck restriction data accuracy; Phase 1 timeline (HERE SDK has faster integration path than custom); CFR risk profile per option
- **Current default:** None — TBD (architecture spike pending)
- **How to inform:** Architecture spike — mandatory first deliverable, must complete before any Phase 1 MD commitment is made
- **Decide by:** Before Phase 1 (immediately — this decision gates all other Phase 1 estimates)

### DP-3: What is the offline activation model?
- **Options:** (a) Manual: driver initiates download before departure (b) Semi-automatic: download triggered when dispatcher assigns route (c) Fully automatic: download triggered by departure time + connectivity forecast (Phase 3 capability)
- **What depends on this:** Phase 1 vs. Phase 3 scope boundary; UX architecture for the download flow; dispatcher-side API design (option b requires dispatcher-to-device trigger); adoption rate and therefore KPI measurability; driver behavior change required
- **Current default:** Implied manual (birth certificate describes driver downloading before departure)
- **How to inform:** 5-driver prototype test under realistic departure conditions; dispatcher workflow analysis to assess feasibility of route-assignment trigger
- **Decide by:** Before Phase 1 (UX design cannot start without this)

### DP-4: What specifically constitutes Phase 1 delivery for the German fleet renewal?
- **Options:** (a) Technical capability live in production (offline nav functional for any pre-downloaded route) (b) Pilot rollout to German fleet drivers with monitored success rate (c) Specific feature checklist agreed in writing with the account team and commercial lead
- **What depends on this:** Whether Phase 1 scope can be minimal (nav only) or must include additional features to satisfy the renewal condition; Q3 deadline pressure calibration; whether the 35 MD Mobile estimate is right-sized
- **Current default:** "Phase 1 delivery = renewal secured" (stated without definition of what delivery means to this customer)
- **How to inform:** Immediate commercial alignment — Commercial lead must obtain the account team's written acceptance criteria before Phase 1 scope is locked
- **Decide by:** Before Phase 1 — most urgent decision given the Q3 deadline

### DP-5: What is the geographic scope of offline map coverage?
- **Options:** (a) Route-specific: driver downloads only the assigned route corridor (b) Corridor/region-level: driver downloads a regional area (e.g., all of Poland) (c) Country-level offline packs
- **What depends on this:** Storage requirements and 150MB constraint viability; driver experience (route-specific requires knowing the exact route in advance — feasible for assigned loads, not for owner-operators with flexible routing); backend complexity for bundle generation; whether the feature works for owner-operators who may deviate from planned routes
- **Current default:** Route-specific (implied by "download a full route before departure")
- **How to inform:** Driver interview: do drivers always know their exact route at download time, or do they make routing decisions en route? Architecture spike on bundle sizes per option.
- **Decide by:** Before Phase 1

---

## 5. Discovery Gaps by Risk Area

| Risk Area | Current Confidence | What Is Missing |
|-----------|-------------------|-----------------|
| Value | Medium | No breakdown of negative App Store reviews by root cause — unknown what % cite connectivity/offline specifically. Causal chain from "complaint" to "churn" is survey-asserted, not analytically validated. Adoption intent (will drivers use the feature?) is entirely unresearched. |
| Usability | Low | Zero user research on offline feature activation behavior. No prototype tested. No evidence that drivers will (or can) proactively download before departure. Download trigger UX is undesigned and untested. Low-end device performance under offline conditions is unknown. |
| Feasibility | Low | No architecture spike completed. Engine technology undecided. Storage constraint (150MB) has not been validated against truck-routing data volumes for representative CEE routes. Low-end Android compatibility untested. Phase 2 conflict resolution complexity not scoped. |
| Viability | Medium | German fleet renewal signal is real and documented. However: tier gating decision unresolved, 40% upgrade rate assumption has no data foundation, current Starter fleet count and upgrade addressable pool not stated, German fleet acceptance criteria not formally documented. |

---

## 6. Recommended Learning Objectives

- We need to learn whether drivers will proactively download routes before departure, or whether an automatic trigger (route assignment by dispatcher, departure time proximity, or connectivity forecast) is required to achieve meaningful adoption
- We need to validate that a truck-capable offline routing engine with CEE coverage can be delivered within the 150MB app size constraint and perform reliably on low-end Android devices representative of the target driver fleet
- We need to understand what specific percentage of current negative App Store reviews cite connectivity or navigation signal loss as the primary failure — to validate that offline mode will materially move the rating
- We need to learn whether Growth-tier-only gating is commercially viable given Sygic Truck's all-tier offline offering — specifically whether Starter fleet owners will upgrade or will instead churn or complain
- We need to validate what the German fleet account team specifically requires from Phase 1 to release their renewal commitment — in writing, before scope is locked
- We need to understand whether route-specific downloads are sufficient for the owner-operator segment, or whether region/corridor-level coverage is required given their routing flexibility

---

## 7. Readiness Signal

Based on current information, this initiative is:

- [x] Ready to move to Discovery Learning Plan

**Rationale:** The value case is sufficiently evidenced (3 quarters of survey data, confirmed commercial forcing function) to justify structured discovery investment. However, two decisions — routing engine technology (DP-2) and German fleet acceptance criteria (DP-4) — are blocking items that must be treated as the first two outputs of discovery, not deferred to Phase 1. The architecture spike and commercial alignment must begin immediately in parallel with user research, not sequentially.

---

## 8. Suggested Next Step

- [x] Create Discovery Learning Plan
