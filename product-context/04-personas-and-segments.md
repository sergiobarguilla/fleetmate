# Personas & Segments — FleetMate

This document defines the **canonical user segments and personas** for FleetMate.
All initiatives must explicitly state **which persona(s)** they target.

---

## Primary Personas

### 1. The Driver

**Description**
- Professional truck driver, employed by a transport company or self-employed (owner-operator)
- Uses the FleetMate driver app daily during trip execution
- Operates on personal phone or company-provided tablet
- Often works in conditions of limited connectivity, time pressure, and physical fatigue

**Key jobs-to-be-done**
- Navigate truck-safe routes accurately (height, weight, hazmat restrictions)
- Receive and execute trip assignments from dispatcher without friction
- Stay informed of changes mid-trip without distraction
- Complete paperwork and proof-of-delivery as simply as possible

**Pain points**
- Navigation loses signal in tunnels, industrial areas, or rural roads — causes missed turns and stress
- Route changes communicated via WhatsApp mid-trip = dangerous distraction
- App crashes or freezes during navigation = loss of trust
- Complicated UI requiring too many taps while driving

**What they value**
- Reliability above all else — "just works"
- Minimal interaction while driving
- Clear, large-font instructions
- Autonomy in routing decisions when unexpected events occur

**Segmentation note**
- Company driver: high dependence on dispatcher; follows assigned routes
- Owner-operator: combines driver + business owner role; more autonomy, more stress

---

### 2. The Dispatcher

**Description**
- Operations coordinator at an SMB transport company
- Manages 5–40 drivers simultaneously
- Works primarily on web (desktop), sometimes mobile
- Responsible for assigning trips, monitoring execution, and resolving problems in real time

**Key jobs-to-be-done**
- Assign and update trips across the fleet with confidence they'll be received and executed
- Track all vehicles in real time on a single map view
- React fast to delays, breakdowns, or route deviations
- Communicate with drivers without leaving the planning tool

**Pain points**
- No confirmation that the driver actually received and accepted a route change
- Constant context-switching between map, phone, WhatsApp, and spreadsheet
- Manual status updates: drivers call in rather than updating automatically
- Blind spots: no real-time ETA for customers unless calling the driver

**What they value**
- Full situational awareness in one screen
- Fast, reliable two-way communication with drivers
- Confidence that what was planned is what's being executed
- Minimal manual data entry

---

### 3. The Fleet Owner / Manager

**Description**
- Owner or senior manager of an SMB transport company (5–100 trucks)
- Primary decision-maker and paying customer
- Works across web and mobile; checks status on the go
- Wears multiple hats: sales, operations, finance, compliance

**Key jobs-to-be-done**
- Know at a glance where every truck is and whether operations are running on time
- Understand fuel, toll, and overtime costs per trip or per driver
- Ensure compliance (tachograph, driving time regulations)
- Make better hiring, routing, and investment decisions from data

**Pain points**
- No single source of truth for costs — data lives in multiple disconnected systems
- Compliance tracking is manual and error-prone
- Hard to see which routes or drivers are profitable vs. loss-making
- Too much time spent on operational coordination that should be automated

**What they value**
- Business-level visibility without requiring technical skills
- Cost and profitability transparency
- Tools that save time across the whole team
- Clear ROI for any subscription investment

---

## Non-target users (explicit)

- Passenger car or van fleets (different regulations, different needs)
- Enterprise carriers (100+ trucks) — require TMS integrations out of scope
- End customers / shippers — not a user of FleetMate today

---

## Segmentation dimensions (used in initiatives)

| Dimension | Values |
|-----------|--------|
| Role | Driver / Dispatcher / Fleet Owner |
| Employment type | Company employee / Owner-operator |
| Fleet size | Micro (5–15) / Small (16–40) / Mid (41–100) |
| Device ownership | Company-provided / BYOD |
| Connectivity profile | Urban (reliable) / Mixed / Rural (frequent offline) |
| Subscription tier | Free (driver only) / Trial / Starter / Growth |
