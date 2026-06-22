# Discovery Summary — Offline Navigation Mode

**Initiative:** Offline Navigation Mode  
**Discovery owner:** Sergio Barguilla  
**Discovery window:** 2026-06-22 to 2026-07-17 (4 weeks)  
**Last updated:** 2026-07-17

> 🧪 **Note:** This discovery summary is based on a **simulated discovery run** for demo purposes. Findings are illustrative and designed to reflect a realistic outcome — not all assumptions validated, scope adjustments required.

**Related artifacts:**
- `birth-certificate.md`
- `discovery-review.md`
- `discovery-learning-plan.md`
- `spike-routing-engine.md` _(simulated)_
- `german-fleet-criteria.md` _(simulated)_
- `appstore-review-analysis.md` _(simulated)_
- `prototype-test-report.md` _(simulated)_
- `tier-gating-brief.md` _(simulated)_

---

## 1. Discovery Objectives Recap

1. Learn whether drivers will proactively download routes before departure, or whether an automatic trigger is required
2. Validate that a truck-capable offline routing engine with CEE coverage fits within the 150MB constraint on low-end Android devices
3. Understand what percentage of negative App Store reviews are attributable to connectivity/signal loss
4. Learn whether Growth-tier-only gating is commercially viable given Fleetmate Truck's all-tier offering
5. Validate what the German fleet account team requires from Phase 1 for renewal — in writing
6. Understand whether route-specific downloads are sufficient for the owner-operator segment

---

## 2. Activities Completed

| Activity | Method | Owner | Status |
|----------|--------|-------|--------|
| Architecture spike: offline routing engine | Technical spike | Mobile engineering lead | Completed |
| German fleet commercial alignment | Stakeholder interview + written documentation | Commercial lead (assigned Week 1) | Completed |
| App Store review root cause analysis | Data analysis | Product analyst | Completed |
| Driver prototype test: activation behaviour | Moderated prototype test (5 drivers) | Product Designer + PM | Completed |
| Tier gating commercial research | Fleet owner interviews + internal data analysis | Commercial lead + PM | Completed |

---

## 3. Key Learnings (Evidence-Based)

### Value
- **38% of 1–2 star App Store reviews in the last 90 days cite navigation/signal loss as the primary failure** — above the 30% success threshold defined in the learning plan. This is the single largest failure category (next: app crashes at 31%, route accuracy errors at 18%).  
  **Evidence:** `appstore-review-analysis.md` — root cause breakdown of 214 reviews, iOS App Store + Google Play, last 90 days

- **"Navigation drops signal" complaints are concentrated in drivers operating CEE rural and long-haul corridors** — consistent with 3-quarter churn survey pattern. Offline mode addresses the specific failure mode these reviews describe.  
  **Evidence:** `appstore-review-analysis.md` cross-referenced with Q1 2026 churn survey open-text responses

### Usability
- **Manual download activation is insufficient for meaningful adoption.** Only 2 of 5 drivers in prototype variant (a) initiated a route download without prompting — below the 3/5 success threshold. Under simulated pre-departure time pressure, drivers consistently skipped the download step, citing "too many things to do before a trip."  
  **Evidence:** `prototype-test-report.md` — variant (a) task completion rate: 40%

- **Auto-trigger on route assignment is the preferred activation model.** Variant (b) — download triggered automatically when the dispatcher assigns the route — achieved 5/5 driver satisfaction. Drivers described it as "invisible" and said they "wouldn't have to think about it." This model requires a dispatcher-to-device trigger in Phase 1, which was not in the original Phase 1 scope.  
  **Evidence:** `prototype-test-report.md` — variant (b) satisfaction rate: 100%; variant (c) rated useful but "too late — I'm already stressed by then"

- **Owner-operators require corridor-level coverage, not route-specific download.** Both owner-operator drivers tested expressed that they frequently deviate from the planned route or don't know the exact route until close to departure. Route-specific download is not sufficient for this sub-segment.  
  **Evidence:** `prototype-test-report.md` — owner-operator session notes, participants O1 and O2

### Feasibility
- **HERE SDK with full truck restriction data exceeds the 150MB constraint at +185MB for a representative 500km CEE route.** The embedded truck restriction dataset for Poland, Germany, and Czech Republic alone adds ~130MB on top of the base navigation tiles.  
  **Evidence:** `spike-routing-engine.md` — binary size measurements per engine option

- **OSM offline bundles with a FleetMate-managed truck restriction overlay come in at +118MB for the same representative route** — within the 150MB constraint. Accuracy for standard height/weight restrictions is acceptable; hazmat routing accuracy is lower than HERE SDK but within tolerable bounds for Phase 1.  
  **Evidence:** `spike-routing-engine.md` — accuracy test results on 12 CEE test routes

- **OSM engine performs within CFR guardrail on low-end Android test devices (2GB RAM, 32GB storage)** — slight navigation rendering lag observed on devices below 2GB RAM, but no crashes recorded in 48-hour soak test.  
  **Evidence:** `spike-routing-engine.md` — device compatibility matrix, 6 device models tested

### Viability
- **German fleet Phase 1 acceptance criteria confirmed in writing.** Offline turn-by-turn navigation with truck restrictions is sufficient for Phase 1 renewal — no Phase 2 features (offline POD, dispatcher offline indicator) are required in Q3. However, the account team requires a **pilot rollout to 10 of their drivers** in Q3 before signing renewal — not just a GA release. This adds a pilot coordination dependency not previously scoped.  
  **Evidence:** `german-fleet-criteria.md` — signed acceptance criteria document, German fleet account team, 2026-07-10

- **Growth-tier-only gating is viable but the upgrade rate is lower than assumed.** 4 of 7 Starter fleet owners interviewed expressed upgrade intent. Internal data analysis shows 340 current Starter fleets; approximately 180 operate CEE routes (high offline risk). Realistic upgrade rate: 25–30% (vs. the 40% assumption in the birth certificate). Year 2 revenue projection should be revised to €48k–€90k (40–75 fleet upgrades), not €80k–€150k.  
  **Evidence:** `tier-gating-brief.md` — interview synthesis (7 fleet owners) + commercial data pull

---

## 4. Assumptions Status

| Assumption | Risk Area | Status | Evidence |
|------------|-----------|--------|----------|
| Drivers will proactively download routes without an automatic trigger | Usability | **Invalidated** | `prototype-test-report.md` — 2/5 task completion in manual variant |
| Offline engine fits within +150MB with truck restrictions on low-end Android | Feasibility | **Validated (with constraint)** — OSM path only; HERE SDK exceeds limit | `spike-routing-engine.md` |
| Connectivity failures are the primary driver of negative App Store reviews | Value | **Validated** — 38% of 1–2 star reviews cite signal/connectivity as primary issue | `appstore-review-analysis.md` |
| Growth-tier-only gating is commercially viable | Viability | **Partially validated** — viable but upgrade rate lower than assumed (25–30%, not 40%) | `tier-gating-brief.md` |
| Route-specific download is sufficient for all driver segments | Usability | **Invalidated for owner-operators** — corridor-level coverage needed | `prototype-test-report.md` |
| Phase 1 technical delivery (GA release) is sufficient for German fleet renewal | Viability | **Partially validated** — offline nav alone is sufficient, but a pilot rollout (not just GA) is required | `german-fleet-criteria.md` |
| Offline engine CFR is within guardrail on low-end devices | Feasibility | **Validated** — no crashes on 2GB+ RAM devices; lag below 2GB noted | `spike-routing-engine.md` |

---

## 5. Impact on Hypothesis

**Original hypothesis (summary):**  
Delivering offline route download and turn-by-turn navigation will enable drivers to complete trips without aborting due to connectivity loss, moving Offline Session Success Rate to ≥ 90% and App Store Rating from 4.1 to ≥ 4.5 within 6 months.

**What changed:**
- The activation model assumption is **invalidated** — auto-trigger on route assignment must replace manual download as the primary activation model. This is a Phase 1 scope addition (requires dispatcher-to-device trigger).
- The OSM-only routing path is now the committed engine choice — truck restriction accuracy is acceptable for Phase 1 but a HERE SDK upgrade should be planned for Phase 2 to close the hazmat accuracy gap.
- Owner-operators are **descoped from Phase 1** — corridor-level map coverage is not feasible within the 150MB constraint at Phase 1 quality. Route-specific download only in Phase 1; corridor-level in Phase 2.
- The App Store rating hypothesis is strengthened — connectivity is the #1 review category, validating the causal chain.

**Updated hypothesis:**  
If we deliver **offline route download triggered automatically on dispatcher route assignment, with OSM-based turn-by-turn navigation and truck restrictions**  
For **company drivers (employed) on pre-assigned long-haul and rural routes in CEE** _(owner-operators deferred to Phase 2)_  
We expect **drivers to complete trips without aborting due to connectivity loss**  
Which will move **Offline Session Success Rate to ≥ 90%** and **App Store Rating from 4.1 to ≥ 4.5**  
By **6 months post-Phase 1 launch**  
Because **38% of negative reviews cite connectivity as the primary failure, and auto-trigger activation removes the adoption barrier identified in prototype testing.**

---

## 6. Impact on Scope & Direction

**In scope (confirmed for Phase 1):**
- Offline turn-by-turn navigation using OSM offline bundles with FleetMate truck restriction overlay
- Route-specific download (pre-assigned routes only)
- Auto-trigger download on dispatcher route assignment (new addition — required for adoption)
- Signal status indicator in driver UI
- Pilot rollout to German fleet (10 drivers) before GA

**Out of scope (confirmed or newly added):**
- HERE SDK in Phase 1 (exceeds 150MB constraint — deferred to Phase 2)
- Corridor/region-level map coverage in Phase 1 (deferred to Phase 2 for owner-operator support)
- Manual download-only activation model (replaced by auto-trigger as primary model)
- Owner-operator segment in Phase 1 (insufficient coverage model for their routing behaviour)
- Hazmat routing accuracy parity with HERE SDK in Phase 1 (accepted gap, documented)

**New constraints or considerations:**
- Auto-trigger requires dispatcher-to-device push API — Backend dependency increases; Backend MD estimate for Phase 1 should be reviewed (was 15 MDs, may need +5–8 MDs)
- German fleet pilot coordination requires a Customer Success owner and a monitoring setup — new operational dependency not in original plan
- OSM truck restriction data accuracy gap for hazmat routes must be disclosed to fleet customers and monitored post-launch

---

## 7. Metrics & Success Criteria Revisited

**Primary metric:** Offline Session Success Rate  
- Baseline: 0 (no offline capability exists)  
- Target: ≥ 90% of offline-initiated sessions complete — unchanged  
- Measurement note: Metric will only be meaningful once auto-trigger adoption drives sufficient offline session volume; adoption monitoring required in first 4 weeks post-launch

**Secondary metric:** App Store Rating  
- Baseline: 4.1  
- Target: ≥ 4.5 — unchanged  
- Timeline note: Rating recovery is slow post-fix; 6-month window remains appropriate

**Guardrails:**
- CFR ≥ 99.2% — unchanged; validated on 2GB+ RAM devices; add monitoring flag for sub-2GB RAM devices in Phase 1
- App download size ≤ +150MB — unchanged; OSM path confirmed at +118MB

**Changes after discovery:**
- Revenue model updated: Year 2 revised to €48k–€90k (from €80k–€150k) based on 25–30% Starter upgrade rate
- Owner-operator segment removed from Phase 1 success metrics; to be added in Phase 2 baseline
- Pilot rollout to German fleet added as Phase 1 milestone (not previously in scope)

---

## 8. Residual Risks

| Risk Area | Risk | Why It Remains | Next Mitigation |
|-----------|------|----------------|-----------------|
| Value | App Store rating recovery may lag even after offline complaints are resolved | Rating inertia — negative reviewers rarely return to re-rate even when the issue is fixed | Plan a targeted in-app rating prompt to satisfied offline users post-Phase 1 launch; do not rely on organic re-rating |
| Usability | Auto-trigger may download routes too early or for cancelled trips, consuming storage and frustrating drivers | Auto-trigger on assignment is net better than manual, but edge cases (cancelled trips, reassigned routes) not tested | Design cache invalidation flow for cancelled/reassigned routes in Phase 1; add to Design scope |
| Feasibility | OSM truck restriction accuracy for hazmat routes is below HERE SDK level | OSM data quality gap documented but not closed; hazmat carriers in the German fleet pilot could experience routing errors | Disclose to German fleet pilot drivers; instrument accuracy complaint rate post-launch; plan HERE upgrade for Phase 2 |
| Viability | Year 2 revenue model depends on Growth-tier gating holding under commercial pressure | Fleetmate Truck competitive pressure remains; if gating is reversed post-launch, upsell revenue disappears | Commercial team must own the gating decision publicly before Phase 1 ships; no post-launch reversal without formal re-review |

---

## 9. Readiness Assessment

Based on discovery outcomes, this initiative is:

- [x] Ready for PRD with constraints

**Rationale:** Core value case is validated (38% of negative reviews addressable, German fleet criteria confirmed, OSM engine feasibility proven). Three scope constraints must be carried into the PRD explicitly: (1) company drivers only in Phase 1 — owner-operators deferred, (2) auto-trigger activation replaces manual download as the primary model, (3) German fleet pilot is a Phase 1 milestone, not a post-GA optional activity. If these constraints are not reflected in the PRD, Phase 1 will under-deliver on adoption and the renewal condition.

---

## 10. Required Updates to Birth Certificate

| Section | Required Update | Owner | Deadline |
|---------|----------------|-------|----------|
| Hypothesis | Narrow target segment to company drivers (employed) for Phase 1; update activation model from manual to auto-trigger | Sergio Barguilla | Before PRD kickoff |
| Constraints / assumptions | Add: OSM engine selected for Phase 1; HERE upgrade deferred to Phase 2. Add: pilot rollout to German fleet required (not just GA) | Sergio Barguilla | Before PRD kickoff |
| General Scope — Phase 1 | Add auto-trigger (download on route assignment) as Phase 1 deliverable. Remove owner-operator from Phase 1 scope. Add German fleet pilot milestone. | Sergio Barguilla | Before PRD kickoff |
| Dependencies — Phase 1 | Backend MD estimate: review +5–8 MDs for dispatcher-to-device trigger API. Add Customer Success dependency for German fleet pilot coordination. | Engineering lead + Sergio | Before PRD kickoff |
| 💰 Impact | Revise Year 2 from €80k–€150k to €48k–€90k; revise Year 3 accordingly; document revised 25–30% upgrade rate assumption | Sergio Barguilla + Finance | Before PRD kickoff |
| Stakeholders | Fill all four TBD names (Marketing, Operations, Commercial, Finance) | Sergio Barguilla | Before PRD kickoff |

---

## 11. Decision & Ownership

**Decision owner:** Sergio Barguilla  
**Decision date:** 2026-07-17

**Final decision:**
- [x] Proceed to PRD with constraints listed in Section 10
