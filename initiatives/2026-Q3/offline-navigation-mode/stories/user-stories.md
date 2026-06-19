# User Stories — Offline Navigation Mode

**Initiative:** Offline Navigation Mode  
**Owner:** Sergio Barguilla  
**Last updated:** 2026-07-17

**Related artifacts:**
- `../birth-certificate.md`
- `../discovery-summary.md`
- `../prd/prd.md`
- `../epics/epics.md`

---

## 1. Story Map Overview

| Story ID | Title | Epic | Priority | Design? |
|----------|-------|------|----------|---------|
| US-EP1-01 | Offline route bundle generation API | EP-1 | Must | No |
| US-EP1-02 | Offline turn-by-turn navigation | EP-1 | Must | No |
| US-EP1-03 | Offline truck restriction enforcement | EP-1 | Must | No |
| US-EP2-01 | Auto-triggered download on route assignment | EP-2 | Must | No |
| US-EP2-02 | Background download with progress indicator | EP-2 | Must | Yes — component update |
| US-EP2-03 | "Offline ready" confirmation on route card | EP-2 | Must | Yes — component update |
| US-EP2-04 | Download failure state and retry | EP-2 | Must | Yes — component update |
| US-EP2-05 | Cache invalidation on route cancellation or reassignment | EP-2 | Must | No |
| US-EP2-06 | Manual offline storage management | EP-2 | Should | Yes — existing screen update |
| US-EP3-01 | Three-state connectivity status indicator | EP-3 | Must | Yes — new component |
| US-EP3-02 | Sub-2GB RAM performance warning | EP-3 | Must | Yes — component update |
| US-EP3-03 | Mid-trip route invalidation notification | EP-3 | Must | Yes — component update |
| US-EP4-01 | Offline session event emission | EP-4 | Must | No |
| US-EP4-02 | Offline Session Success Rate dashboard | EP-4 | Must | No |
| US-EP4-03 | Per-participant pilot monitoring view | EP-4 | Must | No |
| US-EP5-01 | Fleet-level feature flag for pilot cohort | EP-5 | Must | No |
| US-EP5-02 | Growth-tier eligibility gate enforcement | EP-5 | Must | No |
| US-EP5-03 | Post-pilot GA release configuration | EP-5 | Must | No |

---

## 2. Stories by Epic

---

### EP-1 — Offline Routing Engine Foundation

**Epic goal:** Driver can navigate a pre-downloaded route end-to-end with zero connectivity, with truck restrictions enforced.  
**PRD coverage:** FR-3, FR-4

**Instrumentation / measurement notes (epic-level):**
- Track: Navigation session start in offline mode; session completion; CFR on ≥ 2GB and < 2GB RAM device segments
- Guardrails: CFR must remain ≥ 99.2% — hard rollback trigger; app download size delta ≤ +150MB
- Reporting: CFR via existing crash monitoring tool; app size via build pipeline; offline session events via EP-4 dashboard

---

#### US-EP1-01 — Offline route bundle generation API

- **User story:**  
  As the FleetMate system, I need to generate a downloadable offline route bundle for any assigned route, so that the driver app can store and navigate the route without network connectivity.

- **Scope notes:** Bundle must include: route geometry, turn-by-turn instruction data, speed limit data, FleetMate truck restriction overlay (height, weight, tonnage bans — standard CEE data). Bundle must not include hazmat routing data (accepted accuracy gap in Phase 1). Geographic coverage: Poland, Germany, Czech Republic minimum. Bundle format must be versioned and agreed with Mobile before US-EP2-01 begins.
- **PRD requirement(s):** FR-3, FR-4
- **Priority:** Must
- **Owner:** [[TBD — Backend lead]]
- **Dependencies:** None — this story defines the bundle format contract that all other EP-1 and EP-2 stories depend on; must be completed or have a published format spec before US-EP1-02 and US-EP2-01 begin

- **Design required:** No
- **Design type:** No UI (backend API)

**Acceptance criteria (Given/When/Then):**
- Given a route has been assigned to a driver in FleetMate  
  When the bundle generation API is called with the route ID  
  Then a downloadable bundle is produced containing: route geometry, turn-by-turn instruction data, speed limits, and truck restriction overlay for the route corridor

- Given a bundle is generated for a 500km CEE route crossing Poland and Germany  
  When the bundle size is measured  
  Then the total bundle size does not exceed +150MB from the app baseline (target: ≤ +118MB per architecture spike)

- Given a bundle is generated  
  When the bundle format version is checked  
  Then it conforms to the agreed Mobile/Backend bundle format spec published at Sprint 1

- Given truck restriction data is unavailable for a road segment in the route corridor  
  When the bundle is generated  
  Then the missing segment defaults to standard road routing without truck restriction enforcement, and this gap is logged but does not block bundle generation

_Open question: Must the bundle be pre-generated on the server and cached, or generated on-demand per API call? On-demand may introduce latency — confirm Backend architecture approach before story is pointed._

---

#### US-EP1-02 — Offline turn-by-turn navigation

- **User story:**  
  As a company driver, I want to navigate my assigned route with turn-by-turn instructions even when my phone has no mobile signal, so that I can complete my trip on time without relying on connectivity in rural or industrial areas.

- **Scope notes:** Covers the full navigation session: session start from the route screen, voice + visual turn-by-turn, speed limit display, ETA recalculation, deviation re-routing within downloaded bundle coverage. Navigation must work with zero network requests from session start to destination. This story does not cover the download trigger or UI indicator (EP-2 and EP-3 respectively). Persona research note: drivers check app reliability before long-haul trips — the engine must be stable on first use, as one failure destroys trust.
- **PRD requirement(s):** FR-3
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP1-01 (bundle format must be defined); PLATFORM for local storage read access

- **Design required:** No
- **Design type:** No UI (local navigation engine integration — navigation screen UI is pre-existing)

**Acceptance criteria (Given/When/Then):**
- Given a driver has an offline-ready route on their device  
  When the driver starts navigation with zero network connectivity  
  Then turn-by-turn voice and visual instructions are provided from the first turn to the destination without any network call

- Given the driver is navigating offline and deviates from the route within the downloaded bundle corridor  
  When the deviation is detected  
  Then the app re-routes within 5 seconds using locally stored data, without requiring network access

- Given the driver is navigating offline  
  When a speed limit is present on the current road segment in the bundle  
  Then the current speed limit is displayed correctly on the navigation screen

- Given the driver is navigating offline  
  When the driver is 10 minutes from the destination  
  Then the ETA is recalculated locally and shown on the navigation screen without a network call

- Given the offline navigation session runs for the full duration of a representative 500km test route  
  When the session completes  
  Then the app CFR on the test device (≥ 2GB RAM) is not degraded vs. the pre-EP-1 baseline

---

#### US-EP1-03 — Offline truck restriction enforcement

- **User story:**  
  As a company driver, I want my route to respect truck restrictions (height, weight, tonnage bans) while navigating offline, so that I do not accidentally take a restricted road when I have no signal.

- **Scope notes:** Enforces standard height, weight, and tonnage ban restrictions from the FleetMate truck restriction overlay in the offline bundle. Hazmat routing accuracy is an accepted gap in Phase 1 — this story does not address hazmat restrictions. If a route was planned online with restrictions correctly applied, the offline bundle must enforce the same restrictions. This story does not change how restrictions are applied online — offline must be consistent with online.
- **PRD requirement(s):** FR-4
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP1-01 (truck restriction overlay must be in the bundle); US-EP1-02 (engine must be integrated)

- **Design required:** No
- **Design type:** No UI (routing engine logic)

**Acceptance criteria (Given/When/Then):**
- Given a route is downloaded and includes road segments with height or weight restrictions in the bundle  
  When the driver navigates offline and the engine evaluates a routing decision at a restricted segment  
  Then the engine routes around the restriction consistent with the online routing behaviour for the same vehicle profile

- Given a route corridor includes a road with a tonnage ban  
  When the driver is navigating offline through that corridor  
  Then the tonnage-banned road is excluded from the route, and an alternative is used if available in the bundle

- Given the driver's vehicle profile includes a height that exceeds a restriction on the planned route  
  When the offline bundle is navigated  
  Then the restricted road is avoided and the driver is routed via a compliant alternative

_Open question: Hazmat restrictions are excluded from Phase 1. Should the app display a visible disclaimer to the driver at offline session start indicating that hazmat restrictions are not enforced offline? This is a safety question — confirm with Legal and CS before pilot launch._

---

### EP-2 — Offline Route Download Pipeline

**Epic goal:** Route bundles are downloaded automatically on dispatcher assignment and invalidated on cancellation — no driver action required.  
**PRD coverage:** FR-1, FR-2, FR-6

**Instrumentation / measurement notes (epic-level):**
- Track: Auto-trigger download initiation rate (% of route assignments that result in a download attempt); bundle download completion rate; download failure rate; cache invalidation success rate
- Guardrails: Download must not trigger a CFR regression; app must not freeze or crash during background download
- Reporting: Internal dashboard (can be basic in Phase 1 — daily refresh acceptable)

---

#### US-EP2-01 — Auto-triggered download on route assignment

- **User story:**  
  As a company driver, I want my route to be downloaded automatically when the dispatcher assigns it to me, so that I do not have to remember to download it before I lose signal.

- **Scope notes:** This is the primary activation model. When the dispatcher assigns a route in the web dispatcher interface, a push signal is sent to the driver's device and the offline bundle download begins automatically in the background. The driver takes no action to initiate this. Research context: drivers operate under time pressure and cognitive load before departure — any manual step risks being skipped (prototype test: 2/5 task completion for manual download variant).
- **PRD requirement(s):** FR-1
- **Priority:** Must
- **Owner:** [[TBD — Backend lead (push trigger API) + Mobile lead (download manager)]]
- **Dependencies:** US-EP1-01 (bundle must exist to download); Backend dispatcher-to-device push trigger API; dispatcher route assignment event must be available as a trigger

- **Design required:** No
- **Design type:** No UI (backend push trigger + Mobile background download initiation)

**Acceptance criteria (Given/When/Then):**
- Given a dispatcher assigns a route to a specific driver in the FleetMate dispatcher web interface  
  When the route assignment is confirmed  
  Then a push trigger is sent to that driver's device and a background bundle download begins automatically within 30 seconds of assignment, without any driver action

- Given a driver's device is in flight-mode or fully offline at the time of route assignment  
  When the device reconnects to any network  
  Then the bundle download begins automatically within 60 seconds of reconnection

- Given a driver already has a bundle for a previous version of this route cached  
  When a new route bundle is triggered by a route update from the dispatcher  
  Then the new bundle replaces the old one; the driver is not shown the previous bundle as "offline ready"

_Open question: Must be confirmed at story breakdown — does the existing dispatcher push infrastructure support per-device driver targeting? If not, the Backend push trigger API scope increases. This is a hard dependency for this story._

---

#### US-EP2-02 — Background download with progress indicator

- **User story:**  
  As a company driver, I want to see that my route is downloading in the background without interrupting what I am doing in the app, so that I know offline mode is being prepared without any effort on my part.

- **Scope notes:** Download must run in the background and survive app backgrounding (e.g., driver receives a phone call). A non-intrusive progress indicator is shown on the route card. The progress indicator must not interrupt navigation, messaging, or any other app function. Research context: drivers use multiple apps simultaneously and cannot afford disruptions — the download must be invisible except for a status badge.
- **PRD requirement(s):** FR-2
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP2-01 (download must be triggered before progress can be shown); PLATFORM (local storage write)

- **Design required:** Yes
- **Design type:** Component update (route card — add download progress state)
- **Design intent:**
  - Show the driver that offline preparation is happening passively — without demanding attention
  - Progress must be glanceable in under 1 second; no expanded UI required
- **Design constraints:**
  - Must not use full-screen loaders or modals
  - Must be legible in direct sunlight (high contrast)
  - Must not overlap or obscure navigation CTA buttons
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a route bundle download has been triggered  
  When the driver opens the route card  
  Then a download progress indicator is visible showing that the bundle is being downloaded (e.g., progress bar, animated icon, percentage)

- Given a bundle download is in progress and the driver backgrounds the app (e.g., switches to WhatsApp)  
  When the driver returns to the FleetMate app  
  Then the download has continued in the background and the progress indicator reflects the current download state

- Given a bundle download is in progress and the driver starts navigating a different (already-downloaded) route  
  When navigation is active  
  Then the background download continues without interrupting the active navigation session

- Given a download completes while the route card is visible  
  When the download finishes  
  Then the progress indicator transitions to the "Offline ready" state (US-EP2-03) without requiring any driver action

---

#### US-EP2-03 — "Offline ready" confirmation on route card

- **User story:**  
  As a company driver, I want to see a clear confirmation that my route is ready for offline navigation before I start driving, so that I can depart with confidence even if I expect to lose signal.

- **Scope notes:** A persistent "Offline ready" badge or label on the route card confirms that the bundle is fully downloaded and valid. This is distinct from the download progress indicator (US-EP2-02) — it is the final confirmed state. Must be visible before the driver taps "Start navigation."
- **PRD requirement(s):** FR-2
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP2-02 (download must complete before this state appears)

- **Design required:** Yes
- **Design type:** Component update (route card — add "Offline ready" confirmed state)
- **Design intent:**
  - Give drivers a single, unmistakable visual confirmation that their route is safe to use offline — no ambiguity before departure
  - Must feel like a positive confirmation, not just an absence of a warning
- **Design constraints:**
  - Must be legible in bright daylight; high contrast
  - Must be distinguishable from the download progress state (US-EP2-02)
  - Must not require a tap to reveal
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a route bundle has fully downloaded to the driver's device  
  When the driver views the route card  
  Then an "Offline ready" badge or label is persistently visible without requiring any interaction

- Given the route is shown as "Offline ready"  
  When the driver starts navigation and the device has no network connectivity  
  Then offline navigation begins successfully using the downloaded bundle (no connectivity prompt or error)

- Given a cached bundle becomes stale (route was updated by dispatcher)  
  When the new bundle has not yet finished downloading  
  Then the "Offline ready" badge is removed and replaced with the download progress state until the new bundle is complete

---

#### US-EP2-04 — Download failure state and retry

- **User story:**  
  As a company driver, I want to be clearly informed if my offline route download failed, so that I can take action before I lose signal rather than discovering the problem mid-trip.

- **Scope notes:** If the bundle download fails (network error, storage full, server error), the route card must show a clear error state with a retry option. The error message must be actionable — the driver should know what to do, not just that something failed. This must not block navigation for routes already downloaded or online navigation.
- **PRD requirement(s):** FR-2
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP2-01, US-EP2-02

- **Design required:** Yes
- **Design type:** Component update (route card — add download failure state)
- **Design intent:**
  - Inform the driver of the failure without causing alarm — the trip is still possible online; offline was a safety net
  - Make the retry action obvious and one-tap
- **Design constraints:**
  - Must not block the "Start navigation" button — online navigation is always available
  - Error message must be in plain language; no technical error codes visible to drivers
  - Large tap target for retry; accessible to gloved hands
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a route bundle download was triggered and fails due to a network error  
  When the download fails  
  Then the route card shows a clear failure state with a one-tap retry option within 5 seconds of the failure

- Given a bundle download fails due to insufficient device storage  
  When the driver sees the error  
  Then the error message includes guidance to free up storage (in plain language, no technical jargon)

- Given a driver taps "Retry" on a failed download  
  When they have network connectivity  
  Then the download restarts from the beginning and the progress indicator (US-EP2-02) is shown

- Given the download has failed  
  When the driver taps "Start navigation"  
  Then online navigation starts normally — the download failure does not block the trip

---

#### US-EP2-05 — Cache invalidation on route cancellation or reassignment

- **User story:**  
  As a company driver, I want stale offline route data to be automatically removed from my device when my route changes, so that I never accidentally navigate using an outdated route bundle.

- **Scope notes:** When a dispatcher cancels or reassigns a driver's route, a push invalidation signal is sent to the driver's device and the stale bundle is deleted. If the driver is actively navigating the cancelled route in offline mode, the invalidation notification surfaces at the next safe moment (not mid-manoeuvre). Automated cleanup also runs 24 hours after any route completes, regardless of cancellation.
- **PRD requirement(s):** FR-6
- **Priority:** Must
- **Owner:** [[TBD — Backend lead (invalidation signal) + Mobile lead (bundle removal) + Platform lead (storage cleanup)]]
- **Dependencies:** US-EP2-01 (download pipeline must exist); Backend cache invalidation push signal

- **Design required:** No
- **Design type:** No UI (background cache management — notification handled in US-EP3-03)

**Acceptance criteria (Given/When/Then):**
- Given a driver has an offline bundle downloaded for a route  
  When the dispatcher cancels that route in the dispatcher interface  
  Then a cache invalidation signal is pushed to the driver's device and the bundle is removed from local storage within 60 seconds of the cancellation

- Given a driver has an offline bundle downloaded for a route  
  When the dispatcher reassigns the route to a different driver  
  Then the original driver's bundle is invalidated and removed; the new driver's device begins a fresh download

- Given a driver has completed their route  
  When 24 hours have passed since route completion  
  Then the offline bundle for that route is automatically deleted from the driver's device

- Given a driver is actively navigating a route in offline mode  
  When the dispatcher cancels the route mid-trip  
  Then the invalidation notification is queued and displayed at the next moment the driver is not in an active manoeuvre (not immediately interrupting navigation)

---

#### US-EP2-06 — Manual offline storage management

- **User story:**  
  As a company driver, I want to be able to manually clear my offline route data from the app settings, so that I can free up device storage when I need to.

- **Scope notes:** A "Clear offline data" option in the driver app settings removes all cached route bundles from the device. This is a fallback for drivers managing storage manually (e.g., low-storage devices). Does not affect the driver's ability to re-download routes — it only removes locally cached bundles. This is a secondary feature; auto-invalidation (US-EP2-05) is the primary storage management mechanism.
- **PRD requirement(s):** FR-2
- **Priority:** Should
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP2-01, US-EP2-05

- **Design required:** Yes
- **Design type:** Existing screen update (app settings — add "Offline data" section)
- **Design intent:**
  - Give drivers visibility into how much storage offline data is using and a one-tap way to clear it
  - Must feel safe — driver should understand what they are deleting and that it does not affect their active trips
- **Design constraints:**
  - Must show total offline storage used before deletion
  - Must include a confirmation step before deleting all data
  - Must not delete data for routes currently in active navigation
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a driver has one or more offline bundles downloaded  
  When they navigate to app settings and tap "Offline data"  
  Then they see the total offline storage used (in MB) and a "Clear offline data" option

- Given the driver taps "Clear offline data"  
  When they confirm the deletion  
  Then all cached offline bundles are removed from the device and storage usage returns to zero

- Given the driver clears offline data  
  When a previously downloaded route is viewed  
  Then the "Offline ready" badge is removed and the route card shows no offline status (not failed — just not downloaded)

---

### EP-3 — Driver Offline UX & Status Layer

**Epic goal:** Driver always knows their offline mode state through a clear, non-intrusive indicator — no ambiguity about route availability.  
**PRD coverage:** FR-5

**Instrumentation / measurement notes (epic-level):**
- Track: Qualitative — pilot driver debrief responses on indicator clarity (0 pilot drivers should report confusion about offline status)
- Guardrails: Indicator must not trigger any CFR regression; must pass accessibility contrast review before pilot
- Reporting: Pilot debrief (CS-facilitated); App Store review monitoring post-GA for "I didn't know I was offline" complaint category

---

#### US-EP3-01 — Three-state connectivity status indicator

- **User story:**  
  As a company driver, I want to see a clear, always-visible indicator of my connectivity and offline route status during navigation, so that I always know whether my offline route is protecting me or whether I am exposed.

- **Scope notes:** Three states: (1) Online — default, no indicator or minimal indicator; (2) Offline — route downloaded — positive confirmation that offline navigation is active and working; (3) Offline — no downloaded route — warning state that navigation may fail. Must be persistent during navigation, non-interactive (driver cannot tap to dismiss), and legible in direct sunlight. Research context: drivers operate under cognitive load and cannot look away from the road — the indicator must communicate its state in under 1 second.
- **PRD requirement(s):** FR-5
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP1-02 (real connectivity states from the engine); EP-3 design (must be designed before this story is built)

- **Design required:** Yes
- **Design type:** New component (persistent navigation status indicator)
- **Design intent:**
  - State 2 (offline + downloaded) must feel reassuring — driver is safe
  - State 3 (offline + no route) must feel like a warning without causing panic — the driver should know they may need to reconnect
  - State 1 (online) should be visually quiet — not demand attention when everything is normal
- **Design constraints:**
  - Must not overlap turn arrows, road names, or speed limit display — safety-critical navigation elements are inviolable
  - Minimum font size and icon size must comply with driver app conventions (large font, high contrast)
  - Must be legible in bright daylight on all supported device screen profiles
  - Must not require driver interaction to display or dismiss at any time
  - Must meet WCAG AA contrast ratio for outdoor readability
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given the driver is navigating with a live network connection  
  When the navigation screen is active  
  Then the connectivity indicator is in state 1 (online) — visually subdued, not demanding attention

- Given the driver is navigating with zero network connectivity and an offline bundle is active  
  When the navigation screen is active  
  Then the indicator is in state 2 (offline — route downloaded) — clearly visible and reassuring, not alarming

- Given the driver loses network connectivity without a pre-downloaded bundle active  
  When the navigation screen is active  
  Then the indicator transitions to state 3 (offline — no route) — clearly a warning state, distinguishable from state 2

- Given the device transitions from offline back to online mid-trip  
  When connectivity is restored  
  Then the indicator returns to state 1 (online) within 10 seconds of connectivity restoration

- Given any of the three indicator states are active  
  When viewed on a test device in simulated direct sunlight conditions  
  Then the state label and icon pass WCAG AA contrast ratio

---

#### US-EP3-02 — Sub-2GB RAM device performance warning

- **User story:**  
  As a company driver on a low-spec Android device, I want to be warned if my device may experience degraded offline navigation performance, so that I can make an informed decision before relying on offline mode for a critical trip.

- **Scope notes:** Displays a one-time warning on first offline navigation attempt on devices with < 2GB RAM. The warning must not block access to offline navigation — it informs and lets the driver proceed. Devices below 2GB RAM may experience navigation rendering lag (observed in architecture spike) but must not be blocked from using the feature. Research context: many CEE drivers use budget Android devices (older Xiaomi, Samsung A-series) with < 2GB RAM.
- **PRD requirement(s):** FR-5 (connectivity and status layer includes device capability transparency)
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP1-02 (offline navigation session must exist); device RAM detection logic

- **Design required:** Yes
- **Design type:** Component update (one-time warning modal or banner before first offline session start on qualifying devices)
- **Design intent:**
  - Inform without discouraging — this is not an error; the feature works, it may just be slower
  - Driver should be able to dismiss and proceed in one tap
- **Design constraints:**
  - Must include a "Don't show again" or auto-dismiss after first occurrence — must not appear on every offline session start
  - Plain language; no technical terms like "RAM" — use language like "your device may navigate more slowly offline"
  - Must not block the "Start navigation" button — driver can always proceed
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a driver's device has < 2GB RAM  
  When the driver starts their first offline navigation session  
  Then a one-time performance warning is displayed before navigation starts, informing them that navigation may be slower on their device

- Given the driver dismisses the warning  
  When they start subsequent offline sessions  
  Then the warning is not shown again on that device

- Given a driver dismisses the warning and proceeds  
  When offline navigation starts  
  Then navigation starts normally — the warning does not block or delay the session start

- Given a driver's device has ≥ 2GB RAM  
  When they start an offline navigation session  
  Then no performance warning is displayed

---

#### US-EP3-03 — Mid-trip route invalidation notification

- **User story:**  
  As a company driver navigating offline, I want to be notified if my route has been cancelled or changed while I am driving, so that I can contact my dispatcher at the next safe opportunity without being surprised at the destination.

- **Scope notes:** When the dispatcher cancels or reassigns a route that a driver is actively navigating offline, a non-blocking notification surfaces at the next safe moment — not mid-manoeuvre, not during an active turn instruction. The notification must not interrupt active navigation. It informs the driver; it does not stop navigation or auto-reroute. The push notification is sent by the cache invalidation signal (US-EP2-05); this story covers only the in-app presentation logic and UI.
- **PRD requirement(s):** FR-5, FR-6 (notification aspect of cache invalidation)
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP2-05 (cache invalidation push signal must exist); US-EP3-01 (indicator may transition to a "route changed" state)

- **Design required:** Yes
- **Design type:** Component update (in-navigation non-blocking notification banner)
- **Design intent:**
  - Notify the driver calmly — their current navigation continues, but they should be aware of the change
  - Must never interrupt a turn instruction or distract the driver at a critical navigation moment
- **Design constraints:**
  - Must appear as a non-blocking banner at the top or bottom of the navigation screen — not a full-screen modal
  - Must not overlap the turn arrow, road name, or speed limit display
  - Must auto-dismiss after a configurable duration (suggest 8–10 seconds) without driver action
  - Must not be shown if the driver is in the middle of a manoeuvre (within 200m of a turn instruction)
  - Large, legible text; readable by a driver glancing at the screen briefly
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given a driver is navigating offline and the dispatcher cancels the route  
  When the driver is not within 200m of a turn instruction  
  Then a non-blocking banner notification appears informing the driver that their route has been updated, within 60 seconds of the cancellation signal reaching the device

- Given the notification appears  
  When 8 seconds pass without driver interaction  
  Then the notification auto-dismisses

- Given a driver is actively executing a turn instruction (within 200m of the turn)  
  When a route invalidation signal arrives  
  Then the notification is held and displayed at the next safe moment after the manoeuvre is complete

- Given the notification is displayed  
  When the driver taps the notification  
  Then they are taken to the route card or a relevant screen — navigation does not stop

---

### EP-4 — Offline Observability & Instrumentation

**Epic goal:** Offline Session Success Rate is measured and visible from day one of the pilot.  
**PRD coverage:** FR-8

**Instrumentation / measurement notes (epic-level):**
- Track: Offline Session Success Rate (primary KPI); session start count; session completion count; session abort count (system-forced); segmented by fleet, driver, device model
- Guardrails: Event schema must pass Data and Mobile review before any sprint — an incorrect schema invalidates all collected data
- Reporting: Internal dashboard; per-participant pilot view (CS access); available within 2 weeks of pilot start

---

#### US-EP4-01 — Offline session event emission

- **User story:**  
  As a product team, I need the driver app to emit structured events at each offline navigation session state transition, so that the Offline Session Success Rate can be accurately calculated from real session data.

- **Scope notes:** Three events required: (1) `offline_session_started` — emitted when navigation session begins with zero network connectivity; (2) `offline_session_completed` — emitted when the driver reaches the destination as recorded in FleetMate; (3) `offline_session_aborted` — emitted when the session ends before the destination due to a system failure (not a voluntary driver exit). The distinction between system abort and voluntary exit is critical — must be agreed between Mobile and Data before implementation. Events must include: driver ID, fleet ID, device model, device RAM tier (≥2GB / <2GB), route ID, timestamp, session duration.
- **PRD requirement(s):** FR-8
- **Priority:** Must
- **Owner:** [[TBD — Mobile lead]]
- **Dependencies:** US-EP1-02 (offline navigation session must exist before events can be emitted); event schema agreement with Data (US-EP4-02)

- **Design required:** No
- **Design type:** No UI (event instrumentation)

**Acceptance criteria (Given/When/Then):**
- Given a driver starts a navigation session with zero network connectivity  
  When the session begins  
  Then an `offline_session_started` event is emitted within 5 seconds, containing: driver ID, fleet ID, device model, RAM tier, route ID, and timestamp

- Given a driver on an offline session reaches the final destination recorded in FleetMate  
  When the destination is confirmed  
  Then an `offline_session_completed` event is emitted with the session ID and duration

- Given a driver's offline navigation session ends before the destination due to a system-forced exit (app crash, routing engine failure, forced abort by system)  
  When the session ends abnormally  
  Then an `offline_session_aborted` event is emitted with the session ID, abort reason code, and duration at time of abort

- Given a driver voluntarily exits offline navigation before reaching the destination (manually taps exit)  
  When the session ends via voluntary driver action  
  Then no `offline_session_aborted` event is emitted — the voluntary exit is not counted as a system abort

_Open question: Is "voluntary exit" reliably distinguishable from "system abort" using the existing navigation session lifecycle? If the current session termination path does not differentiate the two, Mobile must add an explicit `abort_type` field. This must be resolved before the story is implemented — an incorrect schema means all Phase 1 data is unreliable._

---

#### US-EP4-02 — Offline Session Success Rate dashboard

- **User story:**  
  As a product manager, I need an internal dashboard showing Offline Session Success Rate — overall and segmented by fleet, driver, and device — so that I can monitor Phase 1 performance and validate the 90% target hypothesis.

- **Scope notes:** Dashboard must show: Offline Session Success Rate (% = completed sessions / (completed + aborted sessions)); session volume (total started, completed, aborted); segmented views by fleet, driver, and device RAM tier (≥2GB / <2GB). Daily refresh is acceptable for Phase 1 — real-time is not required. Must be available to the internal product and CS team. Must be live before the German fleet pilot starts (EP-5).
- **PRD requirement(s):** FR-8
- **Priority:** Must
- **Owner:** [[TBD — Data lead]]
- **Dependencies:** US-EP4-01 (events must be emitted before they can be ingested); Backend event ingestion API

- **Design required:** No
- **Design type:** No UI (internal data dashboard — not a product feature)

**Acceptance criteria (Given/When/Then):**
- Given offline session events are being emitted by driver app instances  
  When the Data pipeline ingests these events  
  Then the dashboard shows Offline Session Success Rate updated within 24 hours of events being emitted

- Given the dashboard is open  
  When sessions are segmented by fleet  
  Then each fleet's individual Offline Session Success Rate is visible with session volume

- Given the dashboard is open  
  When sessions are segmented by device RAM tier  
  Then ≥2GB and <2GB device segments are shown separately

- Given a controlled offline test session is run (zero connectivity, route pre-downloaded, navigation completed successfully)  
  When the dashboard is checked the following day  
  Then the test session appears as a completed session and is counted in the success rate

---

#### US-EP4-03 — Per-participant pilot monitoring view

- **User story:**  
  As a Customer Success manager, I need a per-driver view of offline session outcomes during the German fleet pilot, so that I can monitor whether the pilot is meeting the renewal acceptance criteria and identify any drivers having issues.

- **Scope notes:** A per-participant view showing: each pilot driver's name/ID, number of offline sessions started, completed, aborted, and individual Offline Session Success Rate. Accessible to the CS pilot monitor. Daily refresh acceptable. Must be available before pilot day one. This view feeds directly into the post-pilot sign-off workflow in EP-5.
- **PRD requirement(s):** FR-8, FR-7 (instrumentation required by pilot)
- **Priority:** Must
- **Owner:** [[TBD — Data lead]]
- **Dependencies:** US-EP4-02 (dashboard must exist); EP-5 pilot cohort must be configured so pilot driver IDs are known

- **Design required:** No
- **Design type:** No UI (internal dashboard view — not a product feature)

**Acceptance criteria (Given/When/Then):**
- Given the German fleet pilot cohort is configured (driver IDs known)  
  When the CS monitor opens the per-participant view  
  Then each of the 10 pilot drivers is listed with their individual offline session count (started / completed / aborted) and success rate

- Given a pilot driver completes an offline session  
  When the dashboard is checked the following day  
  Then the session appears under that driver's individual row

- Given a pilot driver has zero offline sessions in the pilot window  
  When the CS monitor views the dashboard  
  Then the driver appears with a zero session count — they are not omitted from the view

---

### EP-5 — Pilot Enablement & GA Readiness

**Epic goal:** German fleet pilot runs on 10 drivers, renewal sign-off is obtained, and offline navigation reaches GA for all Growth-tier company driver accounts.  
**PRD coverage:** FR-7

**Instrumentation / measurement notes (epic-level):**
- Track: Pilot completion rate (target ≥ 8 of 10 pilot drivers complete ≥ 1 offline session); written renewal sign-off obtained; Growth-tier eligibility gate accuracy (0 Starter-tier accounts access offline mode)
- Guardrails: Eligibility gate must be verified in staging before pilot launches
- Reporting: EP-4 per-participant view; CS pilot debrief document post-pilot

---

#### US-EP5-01 — Fleet-level feature flag for pilot cohort

- **User story:**  
  As a Customer Success manager, I need to enable offline navigation for specific driver accounts within a single fleet without affecting any other users, so that I can run the German fleet controlled pilot before GA release.

- **Scope notes:** A fleet-level (or driver-level) feature flag that enables the offline navigation feature for a defined list of driver accounts. Must be operable by CS without a new engineering deployment. Pilot cohort is initially 10 German fleet drivers. Must support adding/removing drivers from the cohort without a new app release. This flag must be independent of the Growth-tier eligibility gate (US-EP5-02) — pilot drivers may be enabled regardless of their account tier for testing purposes.
- **PRD requirement(s):** FR-7
- **Priority:** Must
- **Owner:** [[TBD — Backend lead]]
- **Dependencies:** EP-1, EP-2, EP-3, EP-4 must all be complete before pilot can begin; feature flag infrastructure must support driver-level granularity (open question — see below)

- **Design required:** No
- **Design type:** No UI (server-side configuration; CS operates via internal admin tooling)

**Acceptance criteria (Given/When/Then):**
- Given the CS manager adds a driver account to the pilot cohort in the admin configuration  
  When the driver opens the FleetMate app  
  Then offline navigation features (download indicator, "Offline ready" state, offline session capability) are active for that driver

- Given a driver is in the pilot cohort  
  When a dispatcher assigns them a route  
  Then the auto-triggered download (US-EP2-01) initiates for that driver

- Given a driver is removed from the pilot cohort  
  When they open the app  
  Then offline navigation features are no longer visible or active for that driver

- Given 10 pilot drivers are added to the cohort  
  When any user outside the cohort (same fleet or different fleet) opens the app  
  Then they see no offline navigation features

_Open question: Does existing feature flag infrastructure support driver-level granularity (not just fleet-level)? Must be confirmed at story breakdown. If only fleet-level is supported, all drivers in the German fleet would receive the pilot feature — confirm whether this is acceptable with the account team, or whether driver-level control is mandatory._

---

#### US-EP5-02 — Growth-tier eligibility gate enforcement

- **User story:**  
  As a product manager, I need offline navigation to be accessible only to Growth-tier fleet accounts with employed company drivers, so that the feature is correctly positioned as a Growth-tier differentiator and Starter-tier users are not exposed to it prematurely.

- **Scope notes:** Two eligibility conditions must both be true for offline navigation to be surfaced in the driver app: (1) the driver's linked fleet account is on the Growth tier (or Scale tier); (2) the driver account type is "company driver" (employed) — not owner-operator. Starter-tier fleets and owner-operator accounts must not see offline navigation features: no download indicator, no "Offline ready" badge, no offline session capability. The gate must be enforced both server-side (bundle download API rejects ineligible requests) and client-side (UI elements not rendered for ineligible users). Pricing reference: Growth tier (€149/mo) lists "offline sync" as a feature per `product-context/05-pricing-and-monetization.md`.
- **PRD requirement(s):** FR-7
- **Priority:** Must
- **Owner:** [[TBD — Backend lead (server-side gate) + Mobile lead (client-side UI gate)]]
- **Dependencies:** Account tier and driver type data must be available in the driver app auth context; US-EP5-01 pilot flag operates independently of this gate

- **Design required:** No
- **Design type:** No UI (eligibility logic; ineligible users simply do not see offline UI elements)

**Acceptance criteria (Given/When/Then):**
- Given a driver is linked to a Growth-tier fleet account and has a company driver account type  
  When they open the FleetMate app  
  Then offline navigation features are available (download indicator, "Offline ready" state, offline session capability)

- Given a driver is linked to a Starter-tier fleet account  
  When they open the FleetMate app  
  Then no offline navigation UI elements are visible and no bundle downloads are triggered for this driver

- Given a driver has an owner-operator account type  
  When they open the FleetMate app  
  Then no offline navigation UI elements are visible, regardless of their fleet's subscription tier

- Given an ineligible driver's device attempts to request a bundle download from the Backend API  
  When the API receives the request  
  Then the API rejects the request with an appropriate error code — no bundle is served to ineligible accounts

- Given US-EP5-02 is deployed to staging  
  When a Starter-tier test account and an owner-operator test account are tested  
  Then both confirm zero visibility of offline navigation features — mandatory exit criterion before pilot launch

---

#### US-EP5-03 — Post-pilot GA release configuration

- **User story:**  
  As a product manager, I need to release offline navigation to all eligible Growth-tier company driver accounts after the German fleet pilot is successfully signed off, so that the commercial obligation is fulfilled and the feature reaches its full intended audience.

- **Scope notes:** After the German fleet pilot meets the written acceptance criteria (per `german-fleet-criteria.md`) and sign-off is obtained, offline navigation is released to GA: all Growth-tier fleet accounts with company driver account types. The pilot feature flag (US-EP5-01) is superseded by the GA configuration. GA release must not require a new app binary release — it must be a server-side configuration change so it can be deployed promptly after sign-off without App Store review delays.
- **PRD requirement(s):** FR-7
- **Priority:** Must
- **Owner:** [[TBD — Backend lead + Sergio Barguilla (sign-off authority)]]
- **Dependencies:** US-EP5-01 (pilot complete); US-EP5-02 (eligibility gate live); EP-4 dashboard showing pilot session data; written renewal sign-off from German fleet account team

- **Design required:** No
- **Design type:** No UI (server-side configuration change)

**Acceptance criteria (Given/When/Then):**
- Given the German fleet pilot has completed the minimum 2-week window  
  When the written acceptance criteria in `german-fleet-criteria.md` are confirmed as met  
  Then Sergio Barguilla approves GA release and the server-side configuration is updated to enable offline navigation for all Growth-tier company driver accounts

- Given GA configuration is live  
  When a Growth-tier company driver opens the app  
  Then offline navigation features are available without requiring an app update

- Given GA configuration is live  
  When a Starter-tier driver or owner-operator opens the app  
  Then no offline navigation features are visible — eligibility gate remains enforced

- Given GA configuration is deployed  
  When the Offline Session Success Rate dashboard (US-EP4-02) is checked 24 hours post-GA  
  Then session events from non-pilot Growth-tier drivers are appearing, confirming successful GA rollout

---

## 3. Non-Functional Stories

#### NFR-01 — CFR monitoring on sub-2GB RAM devices post-launch

- **Statement:** After Phase 1 GA release, Crash-Free Rate on driver app sessions originating from devices with < 2GB RAM must be monitored separately from the overall CFR metric. If the sub-2GB RAM CFR drops below 98.5%, a device-specific mitigation must be investigated and shipped within one sprint.
- **Related to:** EP-1 (offline engine), EP-3 (performance warning); FR-3 (non-functional guardrails)
- **Design required:** No
- **Acceptance criteria:**
  - Given Phase 1 GA is live  
    When crash monitoring data is reviewed weekly  
    Then < 2GB RAM device CFR is reported as a separate segment from the overall ≥ 2GB CFR
  - Given sub-2GB RAM CFR drops below 98.5% in any weekly review  
    When the threshold is breached  
    Then the engineering lead is notified and a mitigation is scoped for the following sprint

---

#### NFR-02 — Offline bundle download performance on constrained connections

- **Statement:** Offline route bundle download time must be measured and reported in the first 4 weeks post-GA. If the p75 download time for a 500km CEE route exceeds 3 minutes on a 4G connection, a compression or progressive download investigation must be initiated.
- **Related to:** EP-2 (download pipeline); FR-2 (performance constraint)
- **Design required:** No
- **Acceptance criteria:**
  - Given GA is live and auto-triggered downloads are occurring  
    When download performance data is reviewed after 4 weeks  
    Then p50 and p75 download completion times are available, segmented by connection type (4G, 3G, Wi-Fi)
  - Given p75 download time exceeds 3 minutes for a representative route  
    When the threshold is breached  
    Then the Mobile and Backend leads initiate a bundle compression investigation in the following sprint

---

## 4. Dependency & Integration Stories

#### INT-01 — Offline bundle format contract (Mobile ↔ Backend)

- **Statement:** Before US-EP1-02 and US-EP2-01 can be built in parallel, Mobile and Backend must agree and publish a formal offline bundle format contract: structure, versioning, field definitions, and validation rules. This contract must be produced as a shared technical spec artifact at the end of Sprint 1.
- **Depends on:** US-EP1-01 (Backend defines format); Mobile engineering lead must review and sign off
- **Design required:** No
- **Acceptance criteria:**
  - Given Sprint 1 is complete  
    When the bundle format spec is reviewed by both Mobile and Backend leads  
    Then a versioned bundle format spec document is published to the initiative folder and both leads have confirmed it
  - Given the spec is published  
    When US-EP1-02 (Mobile) and US-EP2-01 (Backend pipeline) begin  
    Then both stories reference the agreed spec — no assumptions about bundle structure are made independently

---

#### INT-02 — Offline session event schema agreement (Mobile ↔ Data)

- **Statement:** Before US-EP4-01 (event emission) can be implemented, Mobile and Data must agree on the exact event schema for `offline_session_started`, `offline_session_completed`, and `offline_session_aborted`, including the definition of "system abort" vs. "voluntary exit." This agreement must be documented before any instrumentation sprint begins.
- **Depends on:** US-EP4-01, US-EP4-02 — this agreement unblocks both
- **Design required:** No
- **Acceptance criteria:**
  - Given Sprint 1 is complete  
    When Mobile and Data have reviewed the session lifecycle for abort vs. voluntary exit differentiation  
    Then a formal event schema document is published with field definitions, enum values for abort type, and agreed session lifecycle mapping
  - Given the schema is published  
    When US-EP4-01 is implemented  
    Then all three events conform exactly to the agreed schema — no deviations

---

## 5. Open Questions & Follow-Ups

- **Push infrastructure (EP-2 / US-EP2-01):** Does the existing dispatcher push infrastructure support per-device driver targeting? Backend lead must confirm at Sprint 1 kickoff — if not, US-EP2-01 scope and MD estimate must be revised before the story is pointed.

- **Feature flag granularity (EP-5 / US-EP5-01):** Does the existing feature flag system support driver-level (not just fleet-level) cohort configuration? If only fleet-level is supported, confirm with the German fleet account team whether enabling offline for all their drivers during the pilot is acceptable.

- **Hazmat disclaimer (US-EP1-03):** Should a visible disclaimer be shown to drivers at offline session start informing them that hazmat restrictions are not enforced offline in Phase 1? This is a safety question — requires Legal and CS input before pilot launch.

- **Voluntary exit vs. system abort (US-EP4-01 / INT-02):** Mobile must confirm whether the existing navigation session lifecycle emits distinguishable termination events for system-forced vs. driver-initiated exits. If not, a new `abort_type` event field must be instrumented — failure to resolve this before implementation means all Phase 1 KPI data will be unreliable.

- **All story owners:** All 18 stories currently show `[[TBD]]` for owner. Sergio Barguilla must assign team leads before Sprint 1 begins — stories cannot be pointed or pulled into sprint planning without named owners.
