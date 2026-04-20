# Demo Prompt — Live Webinar

## How to use this file

During the live demo, you will:
1. Open Cursor chat (Cmd+L)
2. Add context: `@product-context/00-index.md` and `@research/master-summary.md`
3. Type: `/create-initiative`
4. When Cursor asks for initiative setup, paste **BLOCK A** below
5. When Cursor asks the full questionnaire, paste **BLOCK B** below

---

## BLOCK A — Initiative Setup (paste when Cursor asks)

```
Initiative name: offline-navigation-mode
Target quarter: 2026-Q3
Owner: Sergio Barguilla
Start quarter: 2026-Q3
```

---

## BLOCK B — Full Questionnaire Answers (paste when Cursor asks)

```
### A) ExCo Summary

1) We are enabling full offline navigation and trip execution for FleetMate drivers
   operating in areas with no or poor mobile connectivity.

2) Target users: Company drivers (employed) and owner-operator drivers operating
   long-haul or rural routes in Central/Eastern Europe.

3) Current behavior: Drivers rely on live connectivity for navigation —
   when signal drops in tunnels, rural roads, or industrial zones, navigation
   stops, causing missed turns, anxiety, and late arrivals.
   Desired behavior: Drivers download their route before departure and execute
   the full trip offline — navigation, turn-by-turn, POI warnings —
   with automatic sync when connectivity is restored.

4) Why now: Competitor apps (Sygic Truck, Google Maps offline) already offer
   offline modes. Driver churn surveys show "navigation drops signal" as the
   #1 quality complaint for the past 3 quarters. A major German fleet customer
   (50+ trucks) has made offline a pre-condition for their renewal.

### B) Hypothesis & Success

5) Primary KPIs:
   - Offline Session Success Rate (% of sessions started offline that complete
     without aborting — defined in metrics doc)
   - App Store Rating (weighted avg, iOS + Android)

6) Baseline:
   - Offline Session Success Rate: not currently tracked (no offline capability)
   - App Store Rating: 4.1 (last 90 days)

7) Targets (6 months post-launch):
   - Offline Session Success Rate: ≥ 90% of offline-initiated sessions complete
   - App Store Rating: ≥ 4.5

8) Guardrails (must not degrade):
   - Crash-Free Rate (CFR) must stay ≥ 99.2%
   - App download size must not exceed +150MB from pre-feature baseline

### C) Scope

9) Customer-visible outcomes:
   - Drivers can download a full route to their device before departure
   - Navigation works fully offline: turn-by-turn, truck restrictions, speed limits
   - Trip data (POD, fuel log entries) sync automatically when signal returns

10) Definition of Done:
    - Driver can start and complete a full trip with zero connectivity
    - All offline trip data is reconciled in dispatcher web view within 60 seconds
      of connectivity being restored

### D) Phases

Phase 1 — Offline Core (2026-Q3):
  Route download, offline turn-by-turn navigation, offline restriction layer.
  Target: offline navigation functional for 100% of pre-downloaded routes.

Phase 2 — Offline Sync & Data (2026-Q4):
  POD capture offline, automatic sync on reconnect, dispatcher visibility
  of "driver is offline" status in real time.

Phase 3 — Smart Pre-download (2027-Q1):
  Auto-suggest route download based on departure time and historical
  connectivity data per route corridor.

### E) Impact

14) Revenue impact:
    - Year 1: €0 direct (retention play — prevents churn of paying fleet accounts)
    - Year 2: €80k–€150k (premium feature driving Growth tier upsell for 60–120 fleets)
    - Year 3: €200k–€400k (offline becomes a differentiator for net-new fleet acquisition)

15) Key assumptions:
    - At least 40% of current Starter fleets will upgrade to Growth tier if offline
      is a Growth-only feature
    - German fleet renewal (50+ trucks) is secured with Phase 1 delivery = ~€18k ARR retained
    - App Store rating lift drives 5–8% improvement in organic installs

### F) People

16) Stakeholders:
    - Marketing lead: [[TBD — assign before PRD]]
    - Operations lead: [[TBD]]
    - Commercial lead: [[TBD — German fleet account owner needed]]
    - Finance lead: [[TBD]]

### G) Dependencies

Phase 1:
  - MOBILE — 35 MDs — Offline map tile download + local routing engine integration — Owner: [[TBD]]
  - BACKEND — 15 MDs — Route export API for offline bundle generation — Owner: [[TBD]]
  - PLATFORM — 10 MDs — Local storage management + cache invalidation — Owner: [[TBD]]
  - DESIGN — 8 MDs — Offline mode UX (download UI, signal indicator, offline badge) — Owner: [[TBD]]

Phase 2:
  - MOBILE — 20 MDs — Offline data capture + sync engine — Owner: [[TBD]]
  - BACKEND — 20 MDs — Conflict resolution + sync reconciliation — Owner: [[TBD]]
  - DATA — 5 MDs — Offline session tracking + analytics events — Owner: [[TBD]]

### H) Product Risks

Value:
  Status: Some evidence
  Evidence: Driver churn survey Q1 2026 — "navigation drops signal" is #1 quality complaint
  (3 consecutive quarters). German fleet pre-condition for renewal confirmed by account team.

Usability:
  Status: Hypothesis only
  Planned validation: Prototype test with 5 drivers on a rural route (offline simulation)
  Questions to answer: Will drivers proactively download routes before departure?
  What download trigger UI works best?

Feasibility:
  Status: Hypothesis only
  Planned validation: Architecture spike — evaluate offline routing engine options
  (embedded HERE SDK vs custom tile server vs OpenStreetMap offline bundles).
  Storage constraints on low-end Android devices not yet assessed.

Viability:
  Status: Some evidence
  Evidence: German fleet renewal ($18k ARR) confirmed as offline-dependent.
  Competitor analysis: Sygic Truck offline available on all tiers — our offline
  as Growth-only may be contested. Pricing model decision required.

### I) Existing Links

Research: /research/master-summary.md (Driver section — Offline gaps)
Competitor reference: Sygic Truck, CoPilot Truck (offline capabilities)
All other artifacts: [[TBD — to be created in discovery]]
```

---

## Notes for presenter

- The `[[TBD]]` fields are intentional — they show the audience that the tool flags
  uncertainty explicitly rather than inventing data.
- After the birth certificate is generated, highlight the **Hypothesis section**
  and the **Risk table** — these are the two things PMs typically skip in real life.
- Mention: "Everything Cursor just generated is grounded in the product-context
  and research files we loaded at the start. It didn't invent anything."
