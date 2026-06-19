# Epic Plan — Offline Navigation Mode

**Initiative:** Offline Navigation Mode  
**Owner:** Sergio Barguilla  
**Last updated:** 2026-07-17

**Related artifacts:**
- `../birth-certificate.md`
- `../discovery-summary.md`
- `../prd/prd.md`
- Delivery Decomposition (produced by delivery-decomposition-agent, 2026-07-17)

---

## 1. Epic Overview

| Epic ID | Epic Name | Outcome / Goal | PRD Coverage | Primary Owner | Depends On |
|---------|-----------|----------------|-------------|--------------|------------|
| EP-1 | Offline Routing Engine Foundation | Driver can navigate a pre-downloaded route end-to-end with zero connectivity, with truck restrictions enforced | FR-3, FR-4 | [[TBD — Mobile engineering lead]] | None |
| EP-2 | Offline Route Download Pipeline | Route bundles are downloaded automatically on dispatcher assignment and invalidated on cancellation — no driver action required | FR-1, FR-2, FR-6 | [[TBD — Mobile lead]] | EP-1 (bundle format contract) |
| EP-3 | Driver Offline UX & Status Layer | Driver always knows their offline mode state through a clear, non-intrusive indicator — no ambiguity about route availability | FR-5 | [[TBD — Product Designer]] | EP-1 (real states); EP-2 (end-to-end flow) |
| EP-4 | Offline Observability & Instrumentation | Offline Session Success Rate is measured and visible from day one of the pilot | FR-8 | [[TBD — Data lead]] | None (parallel) |
| EP-5 | Pilot Enablement & GA Readiness | German fleet pilot runs successfully on 10 drivers, renewal sign-off is obtained, and offline navigation reaches GA for all Growth-tier company driver accounts | FR-7 | [[TBD — PM + CS lead]] | EP-1, EP-2, EP-3, EP-4 |

---

## 2. Epic Details

---

### EP-1 — Offline Routing Engine Foundation

**Outcome / Goal:**  
A driver on a pre-downloaded route can complete full turn-by-turn navigation — including speed limits, truck restriction enforcement, and deviation re-routing — with zero network connectivity from session start to destination, without triggering a CFR regression.

**Problem slice addressed:**  
Navigation currently stops when signal drops. This epic creates the on-device capability that makes offline navigation technically possible. Everything else in the initiative is inert without this foundation.

**Scope (in):**
- OSM offline bundle format: structure, packaging, and versioning for route-specific bundles
- Backend route export API producing downloadable offline bundles from the route data model
- FleetMate truck restriction overlay packaged into the bundle (height, weight, tonnage bans — standard CEE data)
- Local routing engine integration in the driver app: turn-by-turn voice + visual, speed limit display, ETA recalculation, deviation re-routing within bundle coverage
- Geographic coverage: Poland, Germany, Czech Republic minimum; Slovakia, Hungary, Romania if bundle sizes remain within +150MB headroom
- CFR validation on representative device pool (≥ 2GB RAM mandatory; sub-2GB RAM monitored and documented)
- App download size measurement: confirm ≤ +118MB (OSM path confirmed in architecture spike)

**Scope (out):**
- Download trigger, download manager, background download UI (EP-2)
- Connectivity status indicators (EP-3)
- HERE SDK (deferred to Phase 2)
- Hazmat routing accuracy parity with HERE SDK (accepted gap — disclosed, not fixed)
- Corridor or region-level bundle coverage (Phase 2)

**PRD requirements covered:**
- FR-3 (Offline turn-by-turn navigation)
- FR-4 (Offline truck restriction layer)

**Non-functional considerations:**
- CFR must remain ≥ 99.2% post-EP-1 integration — hard rollback trigger if breached on ≥ 2GB RAM devices
- App download size increase must not exceed +150MB from pre-feature baseline; OSM path expected at +118MB
- Navigation session start latency ≤ 3 seconds on ≥ 2GB RAM devices
- Deviation re-routing within bundle coverage must complete within 5 seconds

**Key dependencies:**
- MOBILE — local routing engine integration and navigation UI — Owner: [[TBD]]
- BACKEND — route export API and truck restriction data overlay in bundle — Owner: [[TBD]]
- PLATFORM — local storage read/write during navigation session — Owner: [[TBD]]

**Risks / assumptions carried into delivery:**
- OSM bundle sizes for multi-country routes (e.g., Poland → Germany in one trip) may exceed the +150MB constraint  
  _Mitigation: Validate multi-country route bundle sizes in Sprint 1; fallback is single-country bundles per leg if constraint is breached_
- Sub-2GB RAM devices may show worse-than-expected performance in production vs. architecture spike  
  _Mitigation: Extended soak test before pilot; mandatory CFR monitoring segment post-launch; EP-3 includes sub-2GB performance warning to users_
- Hazmat routing errors may affect hazmat-carrying drivers in the German fleet pilot  
  _Mitigation: Disclosed gap in pilot briefing material; CS must advise hazmat fleets not to rely on offline in Phase 1_

**How we measure success:**
- **Offline Session Success Rate** ≥ 90% of offline-initiated sessions complete without system-forced abort — measured in German fleet pilot window (EP-5)

**Epic exit criteria:**
- Driver can start and complete a full navigation session on a representative CEE route (500km, multi-country) with zero network connectivity
- CFR on ≥ 2GB RAM device pool does not degrade vs. pre-EP-1 baseline
- App download size delta confirmed ≤ +150MB
- Bundle format contract published and agreed with Mobile and Backend — EP-2 can begin

---

### EP-2 — Offline Route Download Pipeline

**Outcome / Goal:**  
When a dispatcher assigns a route, the offline bundle is automatically downloaded to the driver's device in the background — before the driver even thinks about it. If the route is later cancelled or reassigned, the stale bundle is automatically removed.

**Problem slice addressed:**  
Discovery invalidated manual download as the primary activation model (2/5 task completion in prototype testing). This epic implements auto-trigger as the primary path, removing the behaviour-change barrier that would have caused adoption to fail at scale.

**Scope (in):**
- Dispatcher-to-device push trigger API: route assignment event on the dispatcher side initiates a download signal to the target driver's device (Backend new capability)
- Background download manager in driver app: non-blocking, survives app backgrounding (e.g., incoming call mid-download), retry on failure
- Non-intrusive download progress indicator on the route card
- "Offline ready" confirmation on the route card — driver sees this before departure
- Driver-facing download failure error state: clear message, retry option
- Cache invalidation: push invalidation signal on route cancellation/reassignment → remove stale bundle from device storage
- Non-blocking mid-trip notification when an active offline route is invalidated (surfaces at next safe moment)
- Manual "clear offline data" option in driver settings (storage management fallback)
- Automated bundle deletion 24 hours after route completion

**Scope (out):**
- Navigation itself (EP-1)
- Connectivity status indicator on the navigation screen (EP-3)
- Manual download as primary activation — may be included as a secondary fallback if within Mobile MD budget, otherwise deferred to Phase 2
- Corridor/region-level downloads (Phase 2)

**PRD requirements covered:**
- FR-1 (Auto-triggered offline route bundle download)
- FR-2 (Background download management)
- FR-6 (Cache invalidation for cancelled/reassigned routes)

**Non-functional considerations:**
- Bundle download must complete in ≤ 90 seconds for a representative 500km CEE route on a standard 4G connection
- Download must not block navigation, messaging, or other app functions
- Background download must function correctly when the app is backgrounded on both iOS and Android

**Key dependencies:**
- MOBILE — download manager, background download, route card UI states, cache management — Owner: [[TBD]]
- BACKEND — dispatcher-to-device push trigger API (new), route bundle generation pipeline, cache invalidation signal — Owner: [[TBD]]
- PLATFORM — local storage management, 24-hour cleanup job — Owner: [[TBD]]
- EP-1 must deliver the bundle format contract before EP-2 build begins

_Open question: Does existing push infrastructure support driver-level targeting (one push per individual driver device)? Must be confirmed at story breakdown — if not supported, Backend estimate of +5–8 MDs for the trigger API may increase further._

**Risks / assumptions carried into delivery:**
- Dispatcher-to-device push trigger is a new Backend capability — MD estimate of +5–8 MDs assumes existing push infrastructure can be extended, not rebuilt  
  _Mitigation: Backend team confirms push infrastructure capability at story breakdown kickoff; if not extensible, scope manual download fallback as risk mitigation_
- Download performance on rural 3G connections (common in CEE long-haul) may significantly exceed 90-second target  
  _Mitigation: Include bundle compression and progressive download strategies in Engineering design; consider a "download on Wi-Fi only" option as a driver preference in Phase 2_

**How we measure success:**
- **Offline bundle download completion rate** ≥ 85% of auto-triggered download attempts complete successfully within 3 minutes — measured in the first 4 weeks post-GA

**Epic exit criteria:**
- Dispatcher assigns a route → driver device receives the bundle automatically in the background with no driver action
- Route is cancelled → stale bundle is removed from driver device within 60 seconds of the cancellation signal
- Driver can see "Offline ready" on the route card before departure
- App download size delta is unaffected by EP-2 (download pipeline does not add to binary size)

---

### EP-3 — Driver Offline UX & Status Layer

**Outcome / Goal:**  
A driver glancing at the app for half a second knows exactly whether their route is available offline — no ambiguity, no need to dig into settings, readable in bright daylight while wearing gloves.

**Problem slice addressed:**  
Drivers need passive confidence that offline mode is active and their route is ready. Without visible status, drivers won't trust the feature — consistent with the research finding that drivers "check app reliability before starting a long-haul trip" and "will not trust an unstable app." Trust is the product.

**Scope (in):**
- Persistent connectivity status indicator on the navigation screen: three states — Online / Offline (route downloaded) / Offline (no route)
- "Offline ready" route card badge (distinct from EP-2's download progress indicator — this is the final confirmed state)
- Non-blocking notification when an active offline route is invalidated mid-trip (surfaces at next safe moment, does not interrupt a manoeuvre — animation/UX, not the push logic which is in EP-2)
- Sub-2GB RAM device performance warning: displayed on first offline navigation attempt on qualifying devices
- All indicators must meet driver app UX conventions: large font, high contrast, legible in bright daylight, no driver interaction required to dismiss, accessible to gloved users

**Scope (out):**
- Download progress indicator (route card — lives in EP-2)
- Dispatcher-side offline indicators (Phase 2)
- Navigation UI changes (EP-1)

**PRD requirements covered:**
- FR-5 (Connectivity and offline status indicators)

**Non-functional considerations:**
- Indicator must not require a tap to activate or dismiss during navigation
- Must pass contrast accessibility review for in-cab daylight readability
- Must not overlap safety-critical navigation elements (turn arrows, speed limit display)

**Key dependencies:**
- DESIGN — indicator design, route card offline badge, sub-2GB warning — Owner: [[TBD — Product Designer]]
- MOBILE — implementation — Owner: [[TBD]]
- EP-1 required for real connectivity states to develop and test against (can design in parallel; implementation requires EP-1)

**Risks / assumptions carried into delivery:**
- Three-state model (Online / Offline+Downloaded / Offline+NoRoute) is assumed sufficient for Phase 1  
  _Mitigation: If driver research during pilot surfaces confusion, a fourth "downloading" state can be added — the EP-2 route card already covers download progress, so this is low risk_

**How we measure success:**
- **Qualitative pilot signal:** 0 pilot drivers report confusion about whether their route is available offline (assessed via pilot debrief — CS-facilitated)

**Epic exit criteria:**
- Connectivity status indicator is visible and accurate across all three states in a real offline navigation session
- Sub-2GB RAM warning displays correctly on qualifying test devices
- Design has passed internal accessibility review for in-cab readability

---

### EP-4 — Offline Observability & Instrumentation

**Outcome / Goal:**  
The Offline Session Success Rate is measured accurately and available in an internal dashboard from the first day of the German fleet pilot — giving the team real data to confirm or challenge the 90% target hypothesis.

**Problem slice addressed:**  
The primary KPI (Offline Session Success Rate) has no baseline and no measurement infrastructure. Without this epic, the initiative cannot demonstrate whether it succeeded. This epic exists to make the initiative's success objectively verifiable.

**Scope (in):**
- Event schema definition: offline session start (device has no connectivity at navigation start), session completion (destination reached), session abort (system-forced exit before destination — not a voluntary driver choice)
- Event emission from driver app at each state transition
- Backend event ingestion and storage
- Internal dashboard: Offline Session Success Rate — overall, segmented by fleet, driver, and device model
- Per-participant view for German fleet pilot monitoring (feeds EP-5)
- Dashboard available within 2 weeks of pilot start

**Scope (out):**
- Fleet owner-facing analytics or reporting (Phase 2)
- Offline bundle download success rate instrumentation (distinct metric — can be added if within 5 MD budget)
- Battery consumption dashboard (monitoring only in Phase 1 — no structured instrumentation required)

**PRD requirements covered:**
- FR-8 (Offline Session Success Rate instrumentation)

**Non-functional considerations:**
- Event data must distinguish system-forced abort from voluntary driver exit — definition must be agreed between Mobile and Data before any instrumentation sprint begins
- Dashboard must be available to the CS pilot monitor (EP-5) before pilot day one

**Key dependencies:**
- DATA — event pipeline, Offline Session Success Rate dashboard, per-participant view — Owner: [[TBD — Data lead]]
- MOBILE — event emission at session state transitions — Owner: [[TBD]]
- BACKEND — event ingestion API — Owner: [[TBD]]

_Open question: Is the abort vs. voluntary exit distinction reliably detectable from the existing session lifecycle events in the driver app, or does Mobile need to instrument a new explicit "system abort" event? Must be agreed at story breakdown before any instrumentation work begins._

**Risks / assumptions carried into delivery:**
- "Session abort" definition is ambiguous if not explicitly instrumented — metric will be noisy  
  _Mitigation: Write explicit event definitions in story acceptance criteria before sprint begins; Mobile and Data must review together_
- DATA team's existing pipeline (~5 MD estimate) may not support a new real-time segmented dashboard within timeline  
  _Mitigation: A static daily-refresh dashboard is acceptable for pilot monitoring; real-time is not required in Phase 1_

**How we measure success:**
- **Offline Session Success Rate dashboard live and accurate** — validated by replaying a controlled offline test session and confirming the event appears correctly in the dashboard before pilot starts

**Epic exit criteria:**
- A test offline session (zero connectivity, route pre-downloaded) produces correct events: session start, completion/abort
- Offline Session Success Rate is visible in the internal dashboard, segmented by fleet and driver
- Per-participant view is available for the German fleet pilot cohort

---

### EP-5 — Pilot Enablement & GA Readiness

**Outcome / Goal:**  
Offline navigation reaches the German fleet pilot (10 drivers), produces documented session outcome data, satisfies the written renewal acceptance criteria, and subsequently reaches GA for all eligible Growth-tier company driver accounts — completing the Phase 1 commercial obligation.

**Problem slice addressed:**  
The German fleet renewal (~€18k ARR) requires a monitored pilot before GA, per written acceptance criteria. This epic is the delivery gate between "feature complete" and "commercial obligation fulfilled." It also ensures that when offline navigation reaches GA, only eligible accounts (Growth tier, company drivers) can access it.

**Scope (in):**
- Fleet-level feature flag enabling offline navigation for specified driver accounts (pilot cohort configuration without a new app release)
- Growth-tier eligibility gate enforcement in the driver app: offline navigation not surfaced to Starter-tier or owner-operator accounts
- Pilot cohort management: ability to add / remove drivers from the active pilot set
- Pilot monitoring view: per-participant Offline Session Success Rate for the 2-week pilot window (fed by EP-4)
- Pilot runbook for Customer Success: onboarding checklist, monitoring cadence, sign-off checklist (operational document — not a product feature)
- Post-pilot sign-off workflow: criteria check against acceptance criteria, approval gate before GA configuration is flipped
- GA release: feature flag removed or set to Growth-tier all, confirmed eligibility gate active

**Scope (out):**
- User-facing feature toggle (drivers cannot self-enrol in the pilot)
- A/B testing or experiment infrastructure
- Starter-tier access to offline navigation (pricing constraint — enforced in this epic)
- Owner-operator access in Phase 1 (feature not surfaced to owner-operator accounts)

**PRD requirements covered:**
- FR-7 (Pilot rollout capability for German fleet)

**Non-functional considerations:**
- Growth-tier gate must be enforced server-side and client-side — incorrect eligibility logic would expose offline mode to Starter-tier users, violating the pricing constraint
- Feature flag must be operable by CS without an engineering deployment

**Key dependencies:**
- BACKEND — feature flag / fleet-level config, Growth-tier eligibility gate — Owner: [[TBD]]
- MOBILE — eligibility gate enforcement in driver app UI — Owner: [[TBD]]
- DATA — per-participant pilot monitoring view (EP-4 must be complete) — Owner: [[TBD]]
- CUSTOMER SUCCESS — pilot driver onboarding, monitoring cadence, renewal sign-off — Owner: [[TBD — CS lead; must be assigned before EP-5 begins]]
- German fleet account team — pilot participation and written renewal sign-off — external dependency
- EP-1, EP-2, EP-3, EP-4 must all be complete before pilot start

_Open question: Does existing feature flag infrastructure support driver-level granularity (flag applies to specific driver accounts within a fleet, not the entire fleet)? If not, a simpler server-side config or a dedicated pilot build may be needed. Must be confirmed before story breakdown._

**Risks / assumptions carried into delivery:**
- German fleet pilot may be delayed by external account team or driver scheduling constraints  
  _Mitigation: CS owner must secure written pilot schedule ≥ 3 weeks before planned pilot start; include 1-week contingency buffer in Phase 1 timeline_
- CS owner is currently unassigned — pilot has no operational owner  
  _Mitigation: Sergio Barguilla assumes direct pilot coordination ownership if CS lead is not confirmed by Sprint 1; this is a hard constraint_
- Growth-tier eligibility gate misconfiguration could expose the feature to ineligible users  
  _Mitigation: Eligibility gate must be tested in staging with Starter-tier test accounts before pilot begins; include as a mandatory exit criterion_

**How we measure success:**
- **German fleet pilot completion:** ≥ 8 of 10 pilot drivers complete at least one full offline navigation session during the pilot window; written renewal sign-off obtained from account team

**Epic exit criteria:**
- Feature flag enables offline navigation for the 10 German fleet pilot drivers and no other accounts
- Growth-tier eligibility gate is verified: Starter-tier test accounts cannot access offline navigation
- Pilot runs for minimum 2 weeks with Offline Session Success Rate monitored daily
- Written renewal acceptance criteria are met (per `german-fleet-criteria.md`) and sign-off is documented
- GA configuration is live: offline navigation available to all Growth-tier company driver accounts

---

## 3. Sequencing & Milestones (High Level)

**Proposed execution order:**

1. **EP-1 — Offline Routing Engine Foundation** — Must start first; it is the technical critical path. All other Mobile work is blocked until the engine is integrated and the bundle format contract is published. Bundle format contract must be ready by end of Sprint 1 so EP-2 Backend work can begin. Full EP-1 completion target: end of Sprint 3.

2. **EP-2 — Offline Route Download Pipeline** — Backend pipeline work (route export API, push trigger) starts as soon as EP-1 bundle format contract is agreed (Sprint 1 output). Mobile download manager starts once EP-1 engine integration is stable (Sprint 2). Full EP-2 completion target: end of Sprint 5.

3. **EP-3 — Driver Offline UX & Status Layer** — Design work begins in Sprint 1 in parallel with EP-1. Implementation starts in Sprint 2 once EP-1 delivers real connectivity states. Full EP-3 completion target: end of Sprint 5, in parallel with EP-2.

4. **EP-4 — Offline Observability & Instrumentation** — Starts Sprint 1 in full parallel with EP-1. No code dependencies on other epics. Event schema must be agreed between Mobile and Data in Sprint 1 before any instrumentation work begins. Full EP-4 completion target: end of Sprint 4 (must be complete before pilot).

5. **EP-5 — Pilot Enablement & GA Readiness** — Begins only when EP-1, EP-2, EP-3, EP-4 are all verified complete. German fleet pilot runs for minimum 2 weeks. GA follows pilot sign-off. Target: pilot complete and GA live by end of Q3 2026.

**Parallelization opportunities:**
- **EP-3 design track** can run fully in parallel with **EP-1 build track** from Sprint 1 — Design does not require the engine to complete UX specifications
- **EP-4** (Data + Backend event ingestion) runs fully in parallel with EP-1, EP-2, and EP-3 throughout the build phase
- **EP-2 Backend pipeline** (route export API, push trigger) can begin in parallel with **EP-1 Mobile engine** once the bundle format contract is agreed at end of Sprint 1

**Key milestone points:**

| Milestone | Epics | Target |
|-----------|-------|--------|
| Bundle format contract published | EP-1 | End of Sprint 1 |
| Offline session event schema agreed | EP-4 | End of Sprint 1 |
| Engine integrated + CFR validated | EP-1 | End of Sprint 3 |
| Download pipeline complete end-to-end | EP-2 | End of Sprint 5 |
| Driver offline UX complete | EP-3 | End of Sprint 5 |
| Instrumentation dashboard live | EP-4 | End of Sprint 4 |
| German fleet pilot starts | EP-5 | Sprint 6 |
| German fleet pilot complete + sign-off | EP-5 | Sprint 7 |
| GA release live | EP-5 | End of Q3 2026 |

---

## 4. Cross-Team Dependency Matrix

| Team / System | Dependent Epics | Dependency Summary | Owner |
|--------------|----------------|--------------------|-------|
| MOBILE | EP-1, EP-2, EP-3, EP-5 | Offline engine, download manager, navigation UI, status indicators, eligibility gate — largest single team commitment (~40–43 MDs) | [[TBD — Mobile engineering lead]] |
| BACKEND | EP-1, EP-2, EP-4, EP-5 | Bundle export API, push trigger API (new), truck restriction overlay, cache invalidation, event ingestion, feature flag / eligibility gate (~20–23 MDs) | [[TBD — Backend lead]] |
| PLATFORM | EP-1, EP-2 | Local storage read/write, storage management, 24-hour cleanup job (10 MDs) | [[TBD — Platform lead]] |
| DESIGN | EP-3 | Offline UX: connectivity indicator, route card states, sub-2GB RAM warning (8 MDs) — can begin Sprint 1 | [[TBD — Product Designer]] |
| DATA | EP-4 | Event pipeline, Offline Session Success Rate dashboard, per-participant pilot view (~5 MDs) | [[TBD — Data lead]] |
| CUSTOMER SUCCESS | EP-5 | German fleet pilot coordination, driver onboarding, monitoring, renewal sign-off (effort TBD) | [[TBD — CS lead; must be assigned before Sprint 1]] |
| German fleet account team | EP-5 | External: pilot participation and written renewal sign-off — outside FleetMate control | [[TBD — German fleet commercial contact]] |

---

## 5. Open Questions

- **EP-2:** Does existing push infrastructure support driver-level targeting (per-device push for a specific driver account within a fleet)? Must be confirmed at story breakdown — if not, Backend MD estimate for the push trigger API will increase beyond the +5–8 MD estimate.

- **EP-4:** Is the "session abort" state (system-forced navigation exit before destination) reliably distinguishable from a voluntary driver exit using existing session lifecycle events? If not, Mobile must instrument a new explicit event — must be agreed between Mobile and Data at Sprint 1 kickoff before any instrumentation sprint begins.

- **EP-5:** Does existing feature flag infrastructure support driver-level granularity (flag scoped to specific driver accounts, not entire fleets)? If not, a simpler pilot mechanism may be required before story breakdown.

- **All epics:** All six team leads and the CS owner are currently unassigned ([[TBD]]). Sergio Barguilla must confirm owner names before Sprint 1 begins — without named owners, cross-team dependency management cannot function.

- **EP-1:** Additional CEE market coverage (Slovakia, Hungary, Romania) — confirm bundle sizes within +150MB headroom in Sprint 1 before committing to their inclusion in Phase 1 GA scope.
