# Product Requirements Document (PRD)

**Initiative:** Offline Navigation Mode  
**Owner:** Sergio Barguilla  
**Status:** Draft  
**Assumption mode:** Assumption-based (MOCK) — discovery was simulated for demo purposes; all findings are illustrative  
**Last updated:** 2026-07-17

**Related artifacts:**
- `../birth-certificate.md`
- `../discovery-summary.md`
- `../discovery-review.md`
- `../discovery-learning-plan.md`

---

## 1. PRD Overview

**Purpose of this PRD:**  
Define the functional and non-functional requirements for Phase 1 (Offline Core) of the Offline Navigation Mode initiative, enabling delivery planning and epic decomposition.

**Context:**  
FleetMate currently requires live mobile connectivity for all navigation. When signal drops — in tunnels, rural roads, or industrial zones — navigation stops. "Navigation drops signal" has been the #1 driver quality complaint for three consecutive quarters and accounts for 38% of 1–2 star App Store reviews (root cause analysis, discovery). A major German fleet account (50+ trucks, ~€18k ARR) has made offline navigation a written pre-condition for their subscription renewal.

Discovery validated the core value case and identified one critical scope change: manual download activation has insufficient adoption (40% prototype task completion). Auto-triggered download on dispatcher route assignment is the required activation model and is a Phase 1 requirement. Owner-operators have been deferred to Phase 2 due to their need for corridor-level map coverage, which exceeds the 150MB size constraint at Phase 1 quality.

This PRD covers **Phase 1 (Offline Core, 2026-Q3)** only. Phase 2 (Offline Sync & Data) and Phase 3 (Smart Pre-download) are referenced as future scope.

---

## 2. Goals & Non-Goals

### Goals

- Enable company drivers (employed, pre-assigned routes) to complete a full trip with zero mobile connectivity, using a route downloaded automatically before departure
- Provide accurate offline turn-by-turn navigation with truck restrictions (height, weight, tonnage bans) for routes in Poland, Germany, and Czech Republic at minimum
- Surface connectivity status clearly to drivers so they can operate with confidence in low-connectivity environments
- Satisfy the written Phase 1 acceptance criteria of the German fleet account — including a monitored pilot rollout to 10 drivers before GA
- Move App Store Rating from 4.1 toward ≥ 4.5 by resolving the primary driver of negative reviews
- Increase Offline Session Success Rate from 0 (no baseline) to ≥ 90% within 6 months of Phase 1 launch

### Non-Goals (Explicit)

- **Owner-operators are not in scope for Phase 1** — they require corridor/region-level map coverage, which exceeds the 150MB size constraint at current OSM data volumes. Deferred to Phase 2.
- **Offline POD capture and fuel log are not in scope for Phase 1** — deferred to Phase 2 (Offline Sync & Data)
- **Dispatcher-side offline indicators ("driver is offline" status on map view) are not in scope for Phase 1** — deferred to Phase 2
- **HERE SDK integration is not in scope for Phase 1** — OSM offline bundles selected as Phase 1 engine; HERE upgrade path is a Phase 2 consideration
- **Hazmat routing accuracy parity with HERE SDK is not required in Phase 1** — OSM accuracy gap for hazmat routes is an accepted and disclosed limitation
- **Manual-only offline activation (driver-initiated download) is not the primary activation model** — auto-trigger is the primary model; manual download as a secondary fallback may be included but must not be the only path
- **Corridor or region-level map packs are not in scope for Phase 1** — route-specific bundles only
- **Real-time traffic updates in offline mode** — not possible without connectivity; explicitly excluded
- **Offline dispatcher web interface** — dispatcher functionality remains fully online-only

---

## 3. Success Criteria

**Primary metric:**
- **Offline Session Success Rate** — % of navigation sessions started in offline mode that completed without forcing the driver to abort (definition per `product-context/03-metrics-and-definitions.md`)

**Target:**
- ≥ 90% of offline-initiated sessions complete — measured 6 months post-Phase 1 GA launch
- Minimum measurable volume: adoption monitoring required in first 4 weeks post-launch; if offline session volume is below threshold for statistical validity, an intermediate adoption rate metric must be tracked

**Secondary metric:**
- **App Store Rating** — weighted average (iOS App Store + Google Play), last 90 days
- Baseline: 4.1
- Target: ≥ 4.5 — measured 6 months post-Phase 1 GA launch
- Note: rating recovery is slow post-fix; do not expect movement before 8–10 weeks post-launch

**Guardrail metrics (must not degrade):**
- **Crash-Free Rate (CFR) ≥ 99.2%** — hard rollback trigger; any release dropping below this threshold must be rolled back
- **App download size increase ≤ +150MB** from pre-feature baseline; Phase 1 OSM path confirmed at +118MB in architecture spike

**Commercial milestone (Phase 1 gate):**
- German fleet pilot (10 drivers) completed with documented session outcomes before GA release

**Qualitative success signals:**
- Drivers in the German fleet pilot report confidence navigating without checking connectivity status
- No increase in "navigation drops signal" or "app lost my route" complaint categories in App Store reviews post-launch
- Dispatchers report no increase in manual follow-up calls to drivers on routes known to pass through low-connectivity corridors

---

## 4. Scope Definition

### In Scope — Phase 1

- Automatic offline route bundle download triggered by dispatcher route assignment
- Background download management: progress surfacing, completion confirmation, error handling
- Cache invalidation when routes are cancelled or reassigned by a dispatcher
- Offline turn-by-turn navigation (OSM-based engine with FleetMate truck restriction overlay)
- Offline truck restriction layer: height, weight, tonnage bans (standard restrictions; hazmat accuracy gap accepted)
- Geographic coverage: Poland, Germany, Czech Republic as minimum (Phase 1 priority markets); additional CEE markets to be confirmed against bundle size constraint
- Connectivity and offline status indicator in the driver app UI
- "Offline route ready" confirmation surfaced to driver before departure
- German fleet pilot rollout capability: ability to enable offline navigation for a defined subset of drivers before GA
- Pilot monitoring: Offline Session Success Rate visibility per pilot participant

### Out of Scope — Phase 1

- Owner-operator segment (deferred — requires corridor-level coverage)
- Offline POD capture (photo + signature)
- Offline fuel log entry
- Automatic sync of offline trip data on reconnect
- Conflict resolution and sync reconciliation service
- Dispatcher "driver is offline" map indicator
- HERE SDK integration
- Hazmat routing accuracy at HERE SDK level
- Corridor or country-level map pack downloads
- Manual-only download trigger as the sole activation model
- Smart pre-download based on departure time / connectivity forecast (Phase 3)
- Offline support for passenger car or van fleets

### Deferred / Future Considerations

- **Phase 2 (2026-Q4):** Offline POD capture, offline fuel log, automatic sync on reconnect, conflict resolution, dispatcher offline status indicator, owner-operator support (corridor-level coverage), HERE SDK upgrade for hazmat accuracy
- **Phase 3 (2027-Q1):** Smart pre-download prompt engine based on connectivity heatmap per route corridor
- **Future:** Battery consumption optimisation in offline navigation mode (baseline monitoring only in Phase 1); multi-language truck restriction data coverage beyond core CEE markets

---

## 5. Functional Requirements

> Requirements describe what the system must do, not how. Each requirement is independently testable.

---

### FR-1 — Auto-triggered offline route bundle download

- **Description:**  
  When a dispatcher assigns a route to a company driver in the FleetMate dispatcher interface, the system must automatically initiate a background download of the offline route bundle to the driver's device. The driver must not be required to manually trigger this download for the primary activation flow.

- **User(s) impacted:** Company driver (employed); dispatcher (triggers the download implicitly)
- **Priority:** Must
- **Assumption note:** Discovery prototype test showed auto-trigger variant achieved 5/5 driver satisfaction vs. 2/5 for manual-only (simulated finding). This requirement is the critical scope addition from discovery — if removed, adoption will be insufficient to move Offline Session Success Rate at scale.
- **Dependencies:** Backend dispatcher-to-device push trigger API (new, not in original Phase 1 scope — MD estimate requires revision); Mobile auto-download implementation

---

### FR-2 — Background download management

- **Description:**  
  The system must manage the offline route bundle download entirely in the background without interrupting active app use. The driver app must:
  - Display a non-intrusive download progress indicator (e.g., status badge on the route card)
  - Confirm to the driver when the route is "offline ready" before departure
  - Handle download failures gracefully with a retry mechanism and a clear driver-facing error state
  - Not block navigation or other app functions during download

- **User(s) impacted:** Company driver
- **Priority:** Must
- **Assumption note:** None — standard background download behaviour
- **Dependencies:** Mobile (download manager), Platform (local storage management)

---

### FR-3 — Offline turn-by-turn navigation

- **Description:**  
  For routes with a completed offline bundle, the system must provide full turn-by-turn navigation with zero network connectivity from session start to destination. Offline navigation must include:
  - Turn-by-turn voice and visual instructions
  - Speed limit display
  - Remaining distance and estimated arrival time (recalculated locally)
  - Re-routing within the downloaded route corridor if the driver deviates within bundle coverage
  The system must not require any network call during an active offline navigation session.

- **User(s) impacted:** Company driver
- **Priority:** Must
- **Assumption note:** None
- **Dependencies:** Mobile (offline routing engine — OSM-based), Platform (local storage access)

---

### FR-4 — Offline truck restriction layer

- **Description:**  
  The offline routing engine must enforce truck-specific routing restrictions for the downloaded route, including:
  - Vehicle height restrictions
  - Vehicle weight restrictions (gross vehicle weight)
  - Tonnage bans on restricted road segments
  Routing decisions in offline mode must respect the same restriction rules as online mode for standard restriction types.  
  **Accepted limitation:** Hazmat routing accuracy in offline mode (OSM data) is lower than online mode. This gap must be disclosed in Phase 1 release notes and the German fleet pilot briefing. Hazmat parity is deferred to the HERE SDK upgrade in Phase 2.

- **User(s) impacted:** Company driver
- **Priority:** Must
- **Assumption note:** OSM truck restriction data confirmed accurate for standard height/weight restrictions in CEE markets (architecture spike finding — simulated). Hazmat gap is an accepted and documented risk.
- **Dependencies:** Mobile, Backend (truck restriction data overlay packaged into offline bundle)

---

### FR-5 — Connectivity and offline status indicators

- **Description:**  
  The driver app must display a persistent, non-intrusive connectivity status indicator throughout the navigation session. The indicator must distinguish between:
  - **Online** — no indicator or standard state
  - **Offline — route downloaded** — positive indicator confirming safe offline navigation is active (e.g., "Offline mode — route downloaded")
  - **Offline — no downloaded route** — warning state alerting the driver that navigation may fail (e.g., "No signal — route not available offline")
  The indicator must be legible in bright daylight, not require driver interaction to dismiss, and conform to the large-font, minimal-tap UX conventions for the driver app (per `product-context/04-personas-and-segments.md`).

- **User(s) impacted:** Company driver
- **Priority:** Must
- **Assumption note:** None
- **Dependencies:** Design (UI), Mobile

---

### FR-6 — Cache invalidation for cancelled or reassigned routes

- **Description:**  
  When a dispatcher cancels or reassigns a route that has an active offline bundle on a driver's device, the system must:
  - Push a cache invalidation signal to the driver's device
  - Remove the stale offline bundle from local storage
  - Update the route status in the driver app to reflect the change
  If the driver is actively navigating the invalidated route in offline mode, the system must surface a non-blocking notification about the route change at the next available moment (not interrupt active navigation mid-manoeuvre).

- **User(s) impacted:** Company driver, dispatcher
- **Priority:** Must
- **Assumption note:** Cache invalidation edge case identified in discovery as a residual usability risk. Absence of this requirement would result in drivers navigating cancelled routes and consuming device storage on stale data.
- **Dependencies:** Backend (push invalidation signal), Mobile (local cache management), Platform (storage management)

---

### FR-7 — Pilot rollout capability for German fleet

- **Description:**  
  Before GA release, the system must support a controlled rollout of offline navigation to a defined subset of drivers — minimum 10 — without enabling the feature for the full user base. The pilot must include:
  - Feature flag or fleet-level configuration enabling offline navigation for specified driver accounts
  - Per-participant Offline Session Success Rate visibility for the pilot monitoring period
  - Ability to GA release to all eligible users (Growth tier, company drivers) following pilot sign-off
  This is a written acceptance criterion for the German fleet renewal (documented in `german-fleet-criteria.md`).

- **User(s) impacted:** German fleet drivers (pilot); FleetMate Customer Success (monitoring)
- **Priority:** Must
- **Assumption note:** German fleet acceptance criteria confirmed in writing (simulated). Pilot must precede GA — this is not optional.
- **Dependencies:** Backend (feature flag / fleet-level config), Data (per-participant session success rate instrumentation)

---

### FR-8 — Offline Session Success Rate instrumentation

- **Description:**  
  The system must instrument and track Offline Session Success Rate as defined in `product-context/03-metrics-and-definitions.md`:  
  _% of navigation sessions started in offline mode that completed without forcing the driver to abort._  
  A session is "offline-started" if the device has no network connectivity at navigation session start. A session is "aborted" if the driver exits navigation before reaching the destination due to a system failure (not a voluntary re-route or driver choice). This metric must be available in internal dashboards within 2 weeks of Phase 1 launch.

- **User(s) impacted:** Product and engineering (internal)
- **Priority:** Must
- **Assumption note:** Baseline is currently zero (no offline capability exists). The metric is new and requires instrumentation from scratch.
- **Dependencies:** Data, Mobile (event emission), Backend (event ingestion)

---

## 6. Non-Functional Requirements

### Reliability & Quality

- **Crash-Free Rate (CFR):** Must remain ≥ 99.2% across all driver app sessions (iOS + Android) post-Phase 1 launch — this is a hard rollback trigger
- The offline navigation engine must not introduce new crash scenarios on devices with ≥ 2GB RAM. Devices below 2GB RAM must be monitored post-launch; a CFR degradation on this device segment is an acceptable known risk in Phase 1 (flag, do not block launch)
- Offline bundle download failures must not crash or freeze the app; all failure states must be surfaced gracefully to the driver

### Performance

- Offline route bundle download must complete in ≤ 90 seconds for a representative 500km CEE route on a standard mobile data connection (4G, ≥ 10 Mbps)
- Offline navigation session start (from tap to first instruction) must not exceed 3 seconds on devices with ≥ 2GB RAM
- Re-routing within the downloaded bundle (minor deviation) must complete within 5 seconds
- App download size increase must not exceed +150MB from pre-feature baseline (Phase 1 OSM path confirmed at +118MB)

### Security & Compliance

- Offline route bundles stored on device must be encrypted at rest
- Bundles must not contain personally identifiable information beyond the route geometry and restriction data
- Bundle data must be invalidated and deleted from device storage within 24 hours of route completion or cancellation (unless the driver is still actively on the route)
- No location data is transmitted during offline sessions; all location data captured offline must be queued for sync on reconnect (Phase 2 architecture consideration — the queue must be designed in Phase 1 even if sync is not implemented until Phase 2)

### Platform Constraints

- Supported platforms: iOS ≥ 15, Android ≥ 9 (API 28)
- Minimum supported device RAM for full offline navigation: 2GB
- Sub-2GB RAM devices must display a clear warning that offline navigation performance may be degraded; they must not be blocked from using the feature
- App Store and Google Play submission: offline map data must comply with each store's guidelines on bundled content and background download behaviour
- Background download must function correctly when the app is backgrounded by the driver (e.g., receives a call mid-download)

---

## 7. Dependencies & Constraints

**Internal dependencies:**

| Team | Scope | Phase 1 MD Estimate | Note |
|------|-------|---------------------|------|
| MOBILE | Offline engine integration (OSM), auto-trigger, download manager, navigation UI, cache management | ~40–43 MDs | Original estimate was 35 MDs; +5–8 MDs required for auto-trigger from dispatcher assignment (discovery scope addition) |
| BACKEND | Route export API for offline bundle generation, dispatcher-to-device push trigger API, cache invalidation signal, fleet-level feature flag | ~20–23 MDs | Original estimate was 15 MDs; +5–8 MDs for dispatcher-to-device trigger (new, not in original scope) |
| PLATFORM | Local storage management, cache invalidation, bundle encryption at rest | 10 MDs | Unchanged from original estimate |
| DESIGN | Offline mode UX: download status UI, connectivity indicator, "offline ready" confirmation, sub-2GB RAM warning state | 8 MDs | Unchanged; cache invalidation notification state added as minor addition |
| DATA | Offline Session Success Rate instrumentation, per-participant pilot monitoring | ~5 MDs | New addition for Phase 1; required for German fleet pilot and primary KPI baseline |
| CUSTOMER SUCCESS | German fleet pilot coordination: driver onboarding, monitoring, renewal sign-off | [[TBD]] | Operational dependency not in original scope; CS owner needed before Phase 1 kickoff |

**External dependencies:**

- **OpenStreetMap (OSM):** Offline map tile data and road network; open licence; truck restriction overlay managed by FleetMate. Data quality for CEE markets confirmed acceptable for standard restrictions in architecture spike.
- **German fleet account team:** Written acceptance criteria confirmed (simulated). Pilot sign-off required before GA — commercial dependency on external party.

**Key constraints:**

- Phase 1 must be live in production (pilot-ready) by end of 2026-Q3 — hard deadline driven by German fleet renewal condition
- App download size increase ≤ +150MB — contractual and user-experience constraint; OSM path at +118MB provides ~32MB headroom
- Growth-tier gating: offline navigation is a Growth-tier feature per `product-context/05-pricing-and-monetization.md` (Growth tier already lists "offline sync" as a feature). Starter-tier users must not have access to offline navigation in Phase 1. This decision is confirmed and must not be changed post-launch without formal commercial re-review.
- Company drivers only in Phase 1 — owner-operators are not supported. The driver app must not surface the offline download feature to owner-operator accounts in Phase 1.

---

## 8. Risks & Open Questions

> Residual risks carried forward from discovery. These do not block PRD — they require active monitoring.

| Risk Area | Risk | Mitigation / Monitoring |
|-----------|------|--------------------------|
| Value | App Store rating recovery may lag even after connectivity complaints are resolved — negative reviewers rarely return to re-rate | Plan an in-app rating prompt for drivers who complete an offline session successfully; do not rely on organic re-rating. Monitor rating weekly post-launch. |
| Usability | Auto-trigger may download bundles for cancelled trips, consuming device storage unnecessarily | FR-6 (cache invalidation) directly mitigates this. Design must include a manual "clear offline data" option as a driver-accessible fallback. |
| Feasibility | Navigation performance on sub-2GB RAM Android devices is degraded (rendering lag observed in spike) | Sub-2GB RAM devices will receive a performance warning but will not be blocked. CFR monitoring on this device segment is mandatory post-launch. If CFR on sub-2GB devices drops below 98.5%, a device-specific mitigation must be shipped within one sprint. |
| Feasibility | OSM truck restriction accuracy gap for hazmat routes may cause routing errors for hazmat-carrying drivers | Disclosed in German fleet pilot briefing and Phase 1 release notes. Hazmat-carrying fleets should not use offline mode in Phase 1 (communicated by CS). HERE SDK upgrade in Phase 2 closes this gap. |
| Viability | Year 2 upsell revenue depends on Growth-tier gating holding under commercial pressure | Commercial team must communicate the gating decision publicly before Phase 1 ships. Any post-launch reversal requires a formal commercial re-review — not an ad-hoc commercial decision. |
| Viability | German fleet pilot coordination requires a CS owner who is currently unassigned | CS owner must be named before Phase 1 kickoff. If unassigned by Phase 1 start, Sergio Barguilla owns pilot coordination directly. |

**Open questions (not blocking Phase 1 PRD):**

- Battery consumption in offline navigation mode vs. online mode — no target defined. Monitor in German fleet pilot; set a target before Phase 2 planning if significant delta observed.
- Additional CEE market coverage beyond Poland, Germany, Czech Republic — confirm bundle sizes for Slovakia, Hungary, Romania before Phase 1 GA; add if within 150MB headroom.
- Manual download as a secondary fallback (for drivers who want to download before dispatcher assigns, e.g., pre-scheduled loads) — assess feasibility within existing Phase 1 Mobile MD estimate; if within effort, include; if not, defer to Phase 2.

---

## 9. Rollout & Measurement

**Rollout approach:**

1. **German fleet pilot** (before GA — required): Controlled rollout to 10 German fleet drivers via fleet-level feature flag. Duration: minimum 2 weeks. Success criteria defined in `german-fleet-criteria.md`. Customer Success owns pilot operations; Product owns monitoring.
2. **GA — Growth tier, company drivers** (end of 2026-Q3): Full release to all Growth-tier fleet accounts with employed driver accounts. Owner-operator accounts are not eligible in Phase 1 (feature not surfaced in their UI).
3. **Monitoring period:** First 4 weeks post-GA are a mandatory monitoring window. Offline Session Success Rate, CFR, and sub-2GB RAM CFR are reviewed weekly by Sergio Barguilla.

**Measurement plan:**

| Metric | Measurement method | Review cadence | Owner |
|--------|--------------------|----------------|-------|
| Offline Session Success Rate | Internal dashboard (instrumented per FR-8) | Weekly (first 4 weeks post-GA), then monthly | Sergio Barguilla |
| App Store Rating | App Store Connect + Google Play Console | Bi-weekly | PM |
| Crash-Free Rate (CFR) | Existing crash monitoring tooling | Per release + weekly | Engineering lead |
| Sub-2GB RAM CFR | Segmented crash monitoring | Weekly | Engineering lead |
| App download size | Build pipeline measurement | Per release | Platform lead |
| Offline bundle download completion rate | Internal dashboard | Weekly | PM |

---

## 10. Open Decisions

> Decisions intentionally deferred to delivery or later phases.

- **Manual download as secondary fallback:** Assess within Phase 1 Mobile MD estimate during sprint planning. Include if feasible; defer to Phase 2 if not.
- **Additional CEE market coverage** (Slovakia, Hungary, Romania): Confirm bundle sizes against 150MB headroom during Phase 1 build. Include at GA if within constraint.
- **Battery consumption target in offline mode:** Define a target after monitoring pilot data. Not required for Phase 1 release.
- **HERE SDK upgrade timeline:** Phase 2 planning item. Decision should be informed by hazmat complaint rate observed in Phase 1 pilot and GA.
- **Phase 2 scope sequencing** (offline POD vs. dispatcher offline indicator vs. owner-operator support): Priority to be set in Phase 2 planning, informed by Phase 1 adoption data and German fleet pilot feedback.

---

## 11. Appendices & References

**Personas:**  
- Company driver (employed): `product-context/04-personas-and-segments.md`  
  Relevant pain points: navigation loses signal in tunnels/rural roads; complicated UI requiring too many taps while driving; app crashes during navigation destroy trust
- Fleet Owner / Manager: subscription decision-maker; primary buyer; Growth tier required for offline access
- Dispatcher: triggers auto-download via route assignment; not directly impacted by Phase 1 feature but gains indirectly from reduced follow-up calls to offline drivers

**Research evidence (directly relevant):**  
- Driver pain point: "Navigation fails in tunnels, industrial zones, and rural routes — causes missed turns, re-routing stress, and late arrivals" (`research/master-summary.md`, Driver > Pain Points)
- Driver behavioral signal: "Checks app reliability before starting a long-haul trip — will not trust an unstable app" (`research/master-summary.md`, Driver > Behavioral Signals)
- Cross-persona: "Offline is not an edge case — for drivers operating in CEE, poor connectivity is the norm on rural and long-haul routes" (`research/master-summary.md`, Cross-persona insights)

**Pricing reference:**  
- Growth tier (€149/mo, up to 25 vehicles) already lists "offline sync" as a feature — offline navigation is consistent with this positioning (`product-context/05-pricing-and-monetization.md`)

**Metrics definitions:**  
- All metrics used in this PRD are defined in `product-context/03-metrics-and-definitions.md`

**Links:**
- Architecture spike brief: `../spike-routing-engine.md` _(simulated)_
- German fleet acceptance criteria: `../german-fleet-criteria.md` _(simulated)_
- App Store review analysis: `../appstore-review-analysis.md` _(simulated)_
- Driver prototype test report: `../prototype-test-report.md` _(simulated)_
- Tier gating commercial brief: `../tier-gating-brief.md` _(simulated)_

---

## PRD Readiness Check

This PRD is considered:

- [x] Ready with constraints

**Constraints that must be tracked:**
1. German fleet pilot coordination requires a CS owner — currently unassigned. Must be resolved before Phase 1 kickoff.
2. MOBILE and BACKEND Phase 1 MD estimates must be revised upward (~+5–8 MDs each) to account for auto-trigger dispatcher-to-device push API — a scope addition from discovery not in original estimates.
3. Birth Certificate must be updated with revised revenue model (Year 2: €48k–€90k), revised scope, and all stakeholder names before this PRD is socialised with ExCo.

**Decision owner:** Sergio Barguilla  
**Decision date:** 2026-07-17
