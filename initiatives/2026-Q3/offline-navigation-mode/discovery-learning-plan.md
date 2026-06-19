# Discovery Learning Plan — Offline Navigation Mode

**Initiative:** Offline Navigation Mode  
**Owner:** Sergio Barguilla  
**Related artifact(s):**
- `birth-certificate.md`
- `discovery-review.md`

**Discovery window:** 4 weeks (assumed — constrained by Q3 2026 Phase 1 delivery deadline)  
**Last updated:** 2026-06-19

> ⚠️ **Critical path note:** This initiative has a hard Q3 2026 deadline tied to a German fleet renewal (~€18k ARR). Activities 1 and 2 are blocking — they must start in Week 1 in parallel. Discovery cannot run sequentially.

---

## 1. Discovery Objectives

> What must we learn before moving to PRD?

1. Learn whether drivers will proactively download routes before departure, or whether an automatic trigger is required to achieve meaningful adoption
2. Validate that a truck-capable offline routing engine with CEE coverage can be delivered within the 150MB app size constraint and perform reliably on low-end Android devices
3. Understand what percentage of current negative App Store reviews cite connectivity or signal loss as the primary failure — to validate that offline mode will materially move the rating
4. Learn whether Growth-tier-only gating is commercially viable given Sygic Truck's all-tier offline offering
5. Validate what the German fleet account team specifically requires from Phase 1 to release their renewal commitment — in writing
6. Understand whether route-specific downloads are sufficient for the owner-operator segment, or whether region/corridor-level coverage is required

---

## 2. Assumptions Being Tested

| Assumption | Risk Area | Why It Matters |
|------------|-----------|----------------|
| Drivers will proactively download routes before departure without an automated trigger | Usability | If false, adoption will be near-zero and the Offline Session Success Rate KPI will be unmeasurable at scale — feature ships, KPI doesn't move |
| A truck-capable offline engine + CEE map data fits within +150MB on low-end Android devices | Feasibility | If false, either the size constraint must be relaxed (triggering a product and commercial conversation) or the engine architecture must change — with direct risk to the Q3 deadline |
| Connectivity failures (not reconnection errors or UI freezes) are the root cause of the majority of negative App Store reviews citing navigation | Value | If false, offline mode will not move the App Store rating and the secondary success metric is invalidated |
| Growth-tier-only gating will hold commercially — Starter fleet owners will upgrade rather than churn or complain | Viability | If false, Year 2–3 revenue model ($80k–$400k) collapses; gating may need to be reversed under commercial pressure post-launch |
| Route-specific download (pre-assigned route only) is sufficient for company drivers and owner-operators alike | Usability / Feasibility | If false, Phase 1 scope must expand to corridor/region-level bundles, changing storage requirements and backend complexity significantly |
| Phase 1 technical delivery (offline nav live in production) is sufficient to satisfy the German fleet renewal condition | Viability | If false, additional Phase 2 features (POD offline, dispatcher visibility) may be required in Q3 — scope and MD estimates are wrong |

---

## 3. Planned Discovery Activities

> Activities are ordered by criticality and must run in parallel where indicated.

---

### Activity 1 — Architecture Spike: Offline Routing Engine Evaluation

- **Learning objective(s):** Objective 2 (engine feasibility + device constraints); also informs Objective 6 (coverage scope)
- **Risk area:** Feasibility
- **Method:** Technical spike
- **Description:** Evaluate three candidate offline routing engine approaches — (a) embedded HERE SDK, (b) OSM offline bundles, (c) custom tile server — against the following criteria: truck restriction data accuracy for CEE routes, binary size impact on iOS and Android, RAM and storage performance on low-end Android devices (target: devices ≤ 3GB RAM, ≤ 32GB storage), integration complexity within Q3, and Opex cost profile. Produce a decision recommendation with supporting data per criterion. Also estimate map bundle sizes for route-specific vs. corridor-level coverage to inform Activity 6 and DP-5.
- **Participants / data source:** Mobile engineering lead, Platform lead; internal device test pool; HERE/OSM documentation
- **Owner:** [[TBD — Mobile engineering lead]]
- **Estimated effort:** 5–7 days
- **Dependencies:** Must start Week 1 — gates all Phase 1 MD estimates and DP-2 resolution. No other engineering planning should begin until this spike is complete.

---

### Activity 2 — Commercial Alignment: German Fleet Acceptance Criteria

- **Learning objective(s):** Objective 5 (German fleet Phase 1 requirements)
- **Risk area:** Viability
- **Method:** Stakeholder interview + written documentation
- **Description:** The commercial lead (or account owner) must arrange a structured conversation with the German fleet account team to extract and document the exact feature set required for renewal. Output must be a written list of Phase 1 acceptance criteria — not a verbal summary. Specifically: does offline navigation alone satisfy the condition, or are Phase 2 features (offline POD, dispatcher "driver is offline" indicator) required within Q3? What does "live in production" mean to them — GA release, or a pilot rollout to their specific drivers?
- **Participants / data source:** German fleet account team; FleetMate commercial lead (account owner)
- **Owner:** [[TBD — Commercial lead; urgently needs to be assigned]]
- **Estimated effort:** 2–3 days (conversation + documentation)
- **Dependencies:** Must start Week 1. Blocks Phase 1 scope lock. If commercial lead is not assigned by end of Week 1, Sergio Barguilla must own this activity directly.

---

### Activity 3 — Data Analysis: App Store Review Root Cause

- **Learning objective(s):** Objective 3 (review causation analysis)
- **Risk area:** Value
- **Method:** Data analysis
- **Description:** Pull all 1-star and 2-star App Store reviews (iOS + Google Play) from the last 90 days. Categorise by stated failure type: (a) navigation/signal loss, (b) app crash/freeze, (c) route accuracy error, (d) sync/data issue, (e) UX/usability, (f) other. Calculate what % of negative reviews are primarily attributable to offline/connectivity failures. Cross-reference with the Q1 2026 churn survey data ("navigation drops signal" complaint rate) to assess overlap. Output: a single data brief with the % breakdown and a confidence assessment for the App Store rating hypothesis.
- **Participants / data source:** App Store Connect data, Google Play Console, Q1 2026 churn survey raw data
- **Owner:** [[TBD — Product analyst or PM]]
- **Estimated effort:** 2 days
- **Dependencies:** None — can start Week 1 in parallel with Activities 1 and 2.

---

### Activity 4 — Driver Prototype Test: Offline Activation Behaviour

- **Learning objective(s):** Objective 1 (download adoption behaviour); Objective 6 (route-specific vs. corridor coverage need)
- **Risk area:** Usability
- **Method:** Prototype test (moderated, task-based)
- **Description:** Design a lo-fi prototype of the offline download flow in three variants: (a) manual download button on the route screen, (b) automatic download prompt triggered at route assignment, (c) pre-departure reminder notification ("You're departing in 1 hour — download route for offline use?"). Test with 5 drivers — mix of company driver and owner-operator profiles — under simulated pre-departure conditions (time pressure, gloves, phone mounted in cab). Observe: do drivers initiate the download in variant (a) without prompting? Which variant produces the highest completion rate? Do owner-operators express a need for corridor/region-level coverage beyond the assigned route? Note: recruit from existing driver base, not strangers — this tests realistic FleetMate user behaviour.
- **Participants / data source:** 5 drivers — minimum 2 company drivers (CEE routes), 2 owner-operators, 1 mixed-use; recruited from existing FleetMate driver base
- **Owner:** [[TBD — Product Designer + PM]]
- **Estimated effort:** 8–10 days (2 days prototype design, 3 days recruit + schedule, 2 days sessions, 2–3 days synthesis)
- **Dependencies:** Requires at least a conceptual engine decision from Activity 1 to make the prototype technically grounded (e.g., "download will take approximately X minutes for a Y km route"). Can begin prototype design in Week 1; sessions in Week 2–3.

---

### Activity 5 — Commercial Research: Tier Gating Viability

- **Learning objective(s):** Objective 4 (Growth-tier gating commercial viability)
- **Risk area:** Viability
- **Method:** Stakeholder interviews + internal data analysis
- **Description:** Two parallel workstreams: (1) Commercial analysis — pull current Starter fleet count, identify how many have routes with offline risk (CEE, rural, long-haul corridors), estimate realistic upgrade addressable pool. Cross-check the 40% upgrade assumption against any prior upsell conversion data for Growth tier features. (2) 5–8 fleet owner conversations (Starter tier) — ask directly: if offline navigation were a Growth-only feature, would you upgrade? What would you pay? What would you do if you didn't upgrade (stay on Starter, use Sygic Truck instead, churn)? Do not mention pricing specifics — test intent and trade-off framing only.
- **Participants / data source:** Commercial team for data pull; 5–8 Starter-tier fleet owners (existing customers); Growth tier upsell history if available
- **Owner:** [[TBD — Commercial lead + PM]]
- **Estimated effort:** 5–7 days
- **Dependencies:** Commercial lead must be assigned (see Activity 2). Can run in parallel with Activities 3 and 4 from Week 2.

---

## 4. Success & Failure Signals

> Defined upfront — assessed at the end of discovery before PRD gate.

| Learning Objective | Success Signal | Failure Signal |
|--------------------|----------------|----------------|
| Driver download adoption behaviour (Obj. 1) | ≥ 3 of 5 drivers successfully initiate a route download in the manual variant (a); OR a clear preference emerges for auto-trigger variant (b) that is feasible in Phase 1 | < 3 of 5 drivers initiate download in variant (a) AND auto-trigger variants are not feasible in Phase 1 scope — activation model is unresolved |
| Offline engine feasibility (Obj. 2) | At least one engine option fits within +150MB, handles truck restrictions accurately for CEE routes, and performs within CFR guardrail on low-end Android test devices | No option fits within 150MB with truck-restriction data for CEE, OR all options exceed CFR guardrail on low-end devices |
| App Store review root cause (Obj. 3) | ≥ 30% of 1–2 star reviews are primarily attributable to connectivity/signal loss — sufficient to move the aggregate rating materially if resolved | < 15% of negative reviews cite connectivity — offline mode will not materially move App Store rating; secondary metric must be reconsidered |
| Tier gating viability (Obj. 4) | ≥ 50% of Starter fleet owners interviewed express upgrade intent for Growth tier if offline is gated; commercial data supports a ≥ 25% realistic upgrade rate | < 25% of Starter fleet owners express upgrade intent; or commercial data shows the upgrade addressable pool is too small to generate Year 2 revenue projections |
| German fleet acceptance criteria (Obj. 5) | Written list of Phase 1 acceptance criteria obtained; offline navigation alone (no Phase 2 features) is sufficient for renewal | Phase 2 features (offline POD, dispatcher visibility) are required within Q3 — Phase 1 scope is materially larger than planned |
| Route-specific vs. corridor coverage need (Obj. 6) | ≥ 4 of 5 prototype test drivers confirm route-specific download is sufficient; architecture spike confirms route-level bundles fit within 150MB | Owner-operator drivers express need for corridor/region-level coverage; or architecture spike shows route-specific bundles still exceed 150MB for multi-country CEE routes |

---

## 5. Metrics & Evidence to Capture

**Metrics to observe:**
- % of drivers completing route download without prompting (prototype task completion rate by variant)
- % of negative App Store reviews attributable to connectivity/signal loss by root cause category
- % of Starter fleet owners expressing upgrade intent if offline is Growth-only
- Bundle size (MB) per engine option for representative CEE route (500 km, multi-country)
- CFR on low-end Android device pool during offline engine prototype

**Qualitative signals to capture:**
- Driver language around when/why they anticipate connectivity loss — do they predict it accurately?
- Fleet owner framing of offline — "table stakes" or "premium" language
- German fleet account team's specific language around renewal criteria
- Driver confusion or hesitation patterns during download flow in prototype

**Artifacts to produce:**
- Architecture spike decision brief (Activity 1) — engine recommendation with data per criterion
- German fleet written acceptance criteria document (Activity 2)
- App Store review root cause analysis brief (Activity 3)
- Driver prototype test report — variant comparison, behavioural observations, owner-operator coverage findings (Activity 4)
- Tier gating commercial brief — addressable pool analysis + interview synthesis (Activity 5)
- Decision log — one entry per DP resolved, with chosen option and rationale

---

## 6. Risks & Mitigations (Discovery Phase)

| Risk | Impact on Discovery | Mitigation |
|------|---------------------|------------|
| Commercial lead not assigned in Week 1 | Activity 2 (German fleet criteria) and Activity 5 (tier gating research) are blocked — both are on critical path | Sergio Barguilla takes ownership of Activity 2 directly until commercial lead is confirmed; escalate immediately |
| Unable to recruit 5 drivers for prototype test within 2 weeks | Activity 4 delayed; driver activation behaviour remains hypothesis only at PRD gate | Begin recruitment in Week 1 via Customer Success / account managers; prepare a backup plan of 3 drivers minimum |
| Architecture spike exceeds 7 days | DP-2 is unresolved at PRD gate; Phase 1 scope cannot be locked | Time-box the spike to 7 days with a "best available recommendation" output — an imperfect decision is better than no decision at the PRD gate |
| German fleet account team is unresponsive or unwilling to commit in writing | DP-4 is unresolved; Phase 1 scope may be over- or under-built for the renewal condition | Commercial lead to escalate to executive sponsor; set a hard deadline of end of Week 2 for written criteria — if not received, Phase 1 scope must be defined conservatively (assume Phase 2 features may be needed) |
| App Store review data is insufficient for root cause analysis (reviews too vague) | Objective 3 remains unvalidated; App Store rating hypothesis cannot be confirmed or denied | Supplement with qualitative analysis of the Q1 2026 churn survey open-text responses as a proxy |

---

## 7. Decision Criteria

> What decisions will this discovery enable?

The discovery activities directly resolve the five decision points identified in the Discovery Review:

| Decision Point | Activity That Resolves It | Decision Owner |
|----------------|--------------------------|----------------|
| DP-1: Tier placement | Activity 5 | Sergio Barguilla + Commercial lead |
| DP-2: Routing engine technology | Activity 1 | Mobile engineering lead + Sergio Barguilla |
| DP-3: Activation model | Activity 4 | Sergio Barguilla |
| DP-4: German fleet acceptance criteria | Activity 2 | Commercial lead |
| DP-5: Geographic coverage scope | Activities 1 + 4 | Sergio Barguilla |

At the end of discovery, we expect to decide:

- [ ] Proceed to PRD as planned
- [ ] Narrow or change scope
- [ ] Iterate discovery
- [ ] Park or kill the initiative

**Decision owner:** Sergio Barguilla

---

## 8. Outputs & Where They Will Live

| Output | Path / Location |
|--------|-----------------|
| Architecture spike brief | `initiatives/2026-Q3/offline-navigation-mode/spike-routing-engine.md` |
| German fleet acceptance criteria | `initiatives/2026-Q3/offline-navigation-mode/german-fleet-criteria.md` |
| App Store review root cause brief | `initiatives/2026-Q3/offline-navigation-mode/appstore-review-analysis.md` |
| Driver prototype test report | `initiatives/2026-Q3/offline-navigation-mode/prototype-test-report.md` |
| Tier gating commercial brief | `initiatives/2026-Q3/offline-navigation-mode/tier-gating-brief.md` |
| Decision log | `initiatives/2026-Q3/offline-navigation-mode/decision-log.md` |
| Discovery summary (post-discovery) | `initiatives/2026-Q3/offline-navigation-mode/discovery-summary.md` |

**Updates to Birth Certificate after discovery:**
- Stakeholder names (all four leads) — must be filled before PRD
- All dependency owner names
- German fleet Phase 1 acceptance criteria (Constraints / assumptions section)
- Tier placement decision (Constraints / assumptions section)
- Revenue model (if tier gating decision changes)
- Routing engine technology (Dependencies section, Phase 1)
- Offline Session Success Rate 90% target — may need recalibration based on engine spike findings

---

## 9. Suggested Weekly Schedule

> Assumes discovery starts week of 2026-06-22. All activities run in parallel.

| Week | Activities Running | Key Output |
|------|--------------------|------------|
| Week 1 (Jun 22–26) | Activity 1 start (spike), Activity 2 start (commercial), Activity 3 (review analysis) | App Store root cause brief; German fleet meeting scheduled |
| Week 2 (Jun 29–Jul 3) | Activity 1 complete, Activity 2 complete, Activity 4 start (prototype design + recruit), Activity 5 start (fleet owner interviews) | Engine recommendation; German fleet criteria doc; Interview scheduling underway |
| Week 3 (Jul 6–10) | Activity 4 sessions, Activity 5 fleet owner interviews | Driver prototype sessions complete |
| Week 4 (Jul 13–17) | Activity 4 synthesis, Activity 5 synthesis, all decision points resolved | All briefs delivered; decision log complete; PRD gate meeting |

---

## 10. Next Step After Discovery

- [ ] Create PRD
- [ ] Run additional discovery
- [ ] Escalate decision
- [ ] Park / kill initiative
