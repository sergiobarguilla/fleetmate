# Initiative Birth Certificate

**Initiative:** Offline Navigation Mode  
**Start quarter:** 2026 Q3  
**Planning horizon:** Q3 2026 to Q1 2027 (3 quarters)  
**Owner:** Sergio Barguilla  
**Status:** Draft  
**Last updated:** 2026-06-19

---

## General Description (≤ 200 words)

**One-liner:** Enable full offline navigation and trip execution for FleetMate drivers operating in areas with no or poor mobile connectivity.

**Target customers / users:** Company drivers (employed) and owner-operator drivers on long-haul or rural routes in Central and Eastern Europe; fleet owners and dispatchers who depend on reliable trip execution in low-connectivity corridors.

**Customer problem / opportunity:**  
FleetMate currently requires live connectivity for all navigation. When signal drops — in tunnels, rural roads, or industrial zones — navigation stops. Drivers miss turns, arrive late, and lose trust in the app. "Navigation drops signal" has been the #1 quality complaint in driver churn surveys for three consecutive quarters.

**Proposed outcome:**  
Drivers download their route before departure and execute the full trip offline — turn-by-turn navigation, truck restrictions, speed limits, POD capture — with automatic sync when connectivity returns.

**Business relevance:**  
A major German fleet account (50+ trucks, ~€18k ARR) has made offline a pre-condition for renewal. Competitor tools (Sygic Truck, CoPilot Truck) already offer offline modes. Without this capability, FleetMate cannot retain or acquire fleet accounts that operate in low-connectivity regions.

**Constraints / assumptions:**  
- App download size must not increase by more than 150 MB from current baseline  
- Phase 1 must be deliverable by end of 2026-Q3 to satisfy German fleet renewal condition  
- Offline functionality will be scoped to Growth tier (pricing model decision required before PRD)

---

## Hypothesis

If we deliver **full offline route download and turn-by-turn navigation**  
For **company drivers and owner-operators on rural and long-haul routes**  
We expect **drivers to complete trips without aborting due to connectivity loss**  
Which will move **Offline Session Success Rate** to **≥ 90%** and **App Store Rating** from **4.1 to ≥ 4.5**  
By **6 months post-launch**  
Because **"navigation drops signal" is the #1 stated quality complaint across three consecutive churn surveys, and offline capability is a confirmed pre-condition for a major fleet renewal.**

_Open questions / assumptions to validate:_  
- Will drivers proactively download routes before departure, or does this require a prompted/automated trigger?  
- What offline routing engine approach is feasible on low-end Android devices (HERE SDK vs. custom tile server vs. OSM offline bundles)?  
- Will positioning offline as Growth-tier-only hold against Sygic Truck, which offers it on all tiers?  
- What is the realistic Starter → Growth upgrade rate if offline is a Growth-only feature?

---

## Main Metrics / Key Results

**Primary metric (north star for this initiative):**  
Offline Session Success Rate — % of navigation sessions started in offline mode that completed without forcing the driver to abort (as defined in `product-context/03-metrics-and-definitions.md`)

**Baseline:** Not currently tracked (no offline capability exists)  
**Target:** ≥ 90% of offline-initiated sessions complete — measured 6 months post-Phase 1 launch

**Secondary metric:**  
App Store Rating — weighted average (iOS App Store + Google Play), last 90 days  
**Baseline:** 4.1  
**Target:** ≥ 4.5 — measured 6 months post-Phase 1 launch

**Guardrail metrics (must not degrade):**  
- Crash-Free Rate (CFR) ≥ 99.2% — hard rollback trigger  
- App download size increase ≤ +150 MB from pre-feature baseline

**Post-runtime learning statement (to be completed after launch):**

After a runtime of **26 weeks**, we saw **Offline Session Success Rate** [[increase/decrease]] [[significantly/not significantly]].  
This [[validates / fails to validate]] our hypothesis that offline route download will allow drivers to complete trips without aborting due to connectivity loss.  
We also observed **App Store Rating** go [[up/down]].

---

## 💰 Impact

> Revenue impact is primarily a retention and upsell play in Year 1; net-new acquisition from Year 2.

**Revenue impact (by year):**
- **Year 1:** €0 direct revenue; retention of German fleet account (~€18k ARR at risk)
- **Year 2:** €80k–€150k (Growth tier upsell — 60–120 Starter fleets upgrading if offline is Growth-only)
- **Year 3:** €200k–€400k (offline as a net-new acquisition differentiator in CEE and DACH markets)

**Key assumptions:**  
- ≥ 40% of current Starter fleets upgrade to Growth tier if offline is a Growth-only feature  
- German fleet renewal (~€18k ARR) is secured upon Phase 1 delivery  
- App Store rating lift from 4.1 → 4.5 drives 5–8% improvement in organic driver installs  
- Competitor pressure (Sygic Truck offers offline on all tiers) may limit Growth-tier gating — pricing decision required

**Cost & investment (high level):**  
- Build cost (CapEx / MDs): 68 MDs across Phase 1 (Mobile 35, Backend 15, Platform 10, Design 8)  
- Phase 2 additional: 45 MDs (Mobile 20, Backend 20, Data 5)  
- Phase 3 scope: [[TBD — to be estimated in Phase 2 planning]]  
- Run cost (Opex): Offline tile/bundle storage and serving infrastructure [[TBD — architecture spike required]]  
- Opportunity cost: Displaces other mobile reliability work planned for 2026-Q3

---

## General Scope

**Customer-visible outcomes by end of initiative:**  
- Drivers can download a full route to their device before departure
- Navigation works fully offline: turn-by-turn, truck restrictions, speed limits
- Trip data (POD, fuel log entries) syncs automatically when connectivity is restored

**Key deliverables:**  
- Offline map tile download and local routing engine (Mobile)  
- Route export API for offline bundle generation (Backend)  
- Local storage management and cache invalidation (Platform)  
- Offline mode UX: download UI, signal indicator, offline badge (Design)  
- Offline data capture + sync engine (Mobile, Phase 2)  
- Conflict resolution and sync reconciliation service (Backend, Phase 2)  
- Smart pre-download suggestion based on departure time and historical connectivity (Phase 3)

**Definition of Done (high level):**  
- A driver can start and complete a full trip with zero connectivity  
- All offline trip data is reconciled in the dispatcher web view within 60 seconds of connectivity being restored

---

## 🗓️ Phased Scope & Milestones

### Phase 1 — Offline Core (2026-Q3)
**Intended outcome(s):**
- Offline navigation is functional for 100% of pre-downloaded routes
- German fleet renewal condition satisfied

**Key deliverables / milestones:**
- Route download UI (driver app)
- Offline turn-by-turn navigation engine
- Offline truck restriction layer (height, weight, tonnage bans)
- Signal status indicator in driver UI

---

### Phase 2 — Offline Sync & Data (2026-Q4)
**Intended outcome(s):**
- Drivers can capture POD and fuel logs offline
- Dispatcher sees real-time "driver is offline" status
- All offline data reconciles automatically on reconnect

**Key deliverables / milestones:**
- Offline POD capture (photo + signature)
- Offline fuel log entry
- Automatic sync engine with conflict resolution
- Dispatcher "offline status" indicator on map view
- Offline session analytics events instrumented

---

### Phase 3 — Smart Pre-download (2027-Q1)
**Intended outcome(s):**
- Drivers are proactively prompted to download routes based on historical connectivity data per corridor
- Reduces driver effort to activate offline mode

**Key deliverables / milestones:**
- Connectivity heatmap per route corridor (data pipeline)
- Smart pre-download prompt engine (ML or rule-based [[TBD]])
- Driver notification: "Poor connectivity expected — download route now?"

---

## 🔗 Dependencies

**Team codes:** MOBILE, BACKEND, PLATFORM, DESIGN, DATA

### Phase 1
- MOBILE — 35 MDs — Offline map tile download + local routing engine integration — Owner: [[TBD]]
- BACKEND — 15 MDs — Route export API for offline bundle generation — Owner: [[TBD]]
- PLATFORM — 10 MDs — Local storage management + cache invalidation — Owner: [[TBD]]
- DESIGN — 8 MDs — Offline mode UX (download UI, signal indicator, offline badge) — Owner: [[TBD]]

### Phase 2
- MOBILE — 20 MDs — Offline data capture (POD, fuel log) + sync engine — Owner: [[TBD]]
- BACKEND — 20 MDs — Conflict resolution + sync reconciliation service — Owner: [[TBD]]
- DATA — 5 MDs — Offline session tracking + analytics event instrumentation — Owner: [[TBD]]

### Phase 3
- MOBILE — [[TBD]] MDs — Smart pre-download prompt + notification engine — Owner: [[TBD]]
- DATA — [[TBD]] MDs — Connectivity heatmap pipeline per route corridor — Owner: [[TBD]]
- BACKEND — [[TBD]] MDs — Pre-download scheduling API — Owner: [[TBD]]

---

## Not in Scope

- Real-time traffic updates while offline (requires connectivity by definition)
- Offline dispatcher web interface — dispatcher functionality remains online-only
- Offline support for passenger car or van fleets
- Integration with third-party offline map providers as a user-selectable option (single engine decision in Phase 1)
- Tachograph data capture offline (separate compliance initiative)

---

## Why Now?

**Trigger / context:**  
Three converging factors: (1) "Navigation drops signal" is the #1 driver quality complaint for three consecutive quarters. (2) A major German fleet account (50+ trucks) has explicitly made offline a pre-condition for their subscription renewal. (3) Competitor tools (Sygic Truck, CoPilot Truck) already offer offline navigation — FleetMate is now behind on a table-stakes capability in CEE and DACH markets.

**Cost of delay:**  
German fleet renewal (~€18k ARR) is at immediate risk if Phase 1 misses 2026-Q3. Continued negative App Store reviews on navigation reliability will compound MAD and Trial-to-Paid Conversion Rate headwinds.

**Strategic alignment:**  
Directly supports the **Reliability First** strategic theme and the **"Make FleetMate the most reliable SMB fleet tool in EU"** objective. Also supports **Offline Resilience** — a named strategic theme — and the OKR: App Store Rating ≥ 4.5.

---

## 📎 Documentation Links

### Product Risks

| Risk Area | Evidence / Link | Notes / Mitigation |
|-----------|----------------|--------------------|
| **Value** | Driver churn survey Q1 2026 — "navigation drops signal" #1 complaint (3 consecutive quarters); German fleet renewal pre-condition confirmed by account team | Some evidence. Strongest risk signal in the dataset. |
| **Usability** | No link yet | Hypothesis only. Planned: prototype test with 5 drivers on a rural route (offline simulation). Key question: will drivers proactively download routes, and what trigger UI works best? |
| **Feasibility** | No link yet | Hypothesis only. Planned: architecture spike to evaluate offline routing engine options (embedded HERE SDK vs. custom tile server vs. OSM offline bundles). Storage constraints on low-end Android TBD. |
| **Viability** | German fleet renewal (~€18k ARR) confirmed as offline-dependent | Some evidence. Risk: Sygic Truck offers offline on all tiers — Growth-only gating may be contested. Pricing model decision required before PRD. |

---

## Discovery Artifacts

| Artifact | Evidence / Link |
|----------|-----------------|
| Lean Product Canvas | [[TBD — to be created in discovery]] |
| Opportunity–Solution Tree | [[TBD — to be created in discovery]] |
| High-level solution architecture | [[TBD — architecture spike, Phase 1]] |
| Lo-fi design | [[TBD — Design, Phase 1]] |
| Customer Journey (as-is / to-be) | [[TBD — to be created in discovery]] |
| Customer Journey Map | [[TBD — to be created in discovery]] |
| Personas | See `product-context/04-personas-and-segments.md` |

---

## 👥 Stakeholders

**Marketing lead:** [[TBD — assign before PRD]]  
**Operations lead:** [[TBD]]  
**Commercial lead:** [[TBD — German fleet account owner needed urgently]]  
**Finance:** [[TBD]]
