# create-initiative

You are helping the user create a **Product Initiative Birth Certificate**.

This document is the **mandatory entry point** for any initiative before discovery, PRD, or delivery work begins.

---

## Your Goal

1. Create a **new initiative folder**
2. Generate a **complete birth-certificate.md** inside it
3. Ask **enough questions up front** to minimize `[[TBD]]`, especially:
   - Stakeholder names
   - Dependency owners
   - Baseline/target metrics

---

## Context Requirements (mandatory)

Before drafting the birth certificate:
- Read `@product-context/00-index.md`
- Follow any referenced documents listed there (as needed)
- If these files are not available in context, ask the user to add them before proceeding

---
## Research Evidence

- What persona research supports this initiative?
- Which pain points does this address?
- What monetization signals validate this?
- Are there contradictions in research?

Always reference /research/master-summary.md and relevant persona folders.
If no research exists, flag as: ⚠️ No research validation found.

---

## Before You Start (required)

Ask the user for the **Initiative Setup** fields first (in one message):

- **Initiative name** (short, kebab-case friendly)
- **Target quarter** (e.g. 2026 Q1 — used for folder naming)
- **Owner** (single accountable person)
- **Start quarter** (if different from target quarter)

👉 Do not proceed until all four are answered.

---

## Folder & File Creation Rules

Once confirmed, assume the following structure:

/initiatives/{YYYY-QX}/{initiative-name}/birth-certificate.md

- `{YYYY-QX}` comes from **Target quarter**
- Create the folder if it does not exist
- Create `birth-certificate.md` as the first artifact
- Do NOT create PRD, discovery, or delivery files yet

---

## Questioning Rules

### Ask ONE consolidated questionnaire (required)
After the initiative setup is provided, ask **one single message** containing the questionnaire below.
- Keep it concise and scannable
- Use bullet points / numbered items
- Prefer dropdown-like options where relevant
- The goal is to reduce `[[TBD]]` to only evidence links that don’t exist yet

### Do NOT browse the web
- Do not browse the web
- Do not invent metrics, revenue, names, or scope
- If unknown, use `[[TBD]]` and add `_Open question: ..._`

---

## The Questionnaire (ask this in ONE message)

### A) ExCo Summary (fast)
1) In **1 sentence**, what are we doing?  
2) Who is the **target user segment** (company driver / owner-operator / mixed/BYOD / other)?  
3) What is the **current behavior** vs **desired behavior** (1–2 bullets)?  
4) What is the **Why now** trigger (risk, opportunity, contract, competitor, quality, growth, Office integration)?

### B) Hypothesis & Success
5) Primary KPI to move (pick 1–2) + **definition** if special:
   - MAD / Active Devices / CFR / ANR / Route Completion / Rating / Conversion / ARPU / Other: ___
6) Baseline + timeframe (e.g. “CFR 99.2% last 28 days”)  
7) Target + timeframe (e.g. “CFR +0.2pp by end of Phase 2”)  
8) Guardrails (must not degrade): ___

### C) Scope (customer-visible)
9) List **3 customer-visible outcomes** (bullets).  
10) Definition of Done in 1–2 bullets (what must be true in prod).

### D) Phases & timeline (lightweight)
11) Phase 1 name + (optional quarter) + intended outcome  
12) Phase 2 name + (optional quarter) + intended outcome  
13) Phase 3 name + (optional quarter) + intended outcome  
(If fewer phases, say “N/A”)

### E) 💰 Impact (even if rough)
14) Expected revenue impact (ranges are fine):
   - Year 1:
   - Year 2:
   - Year 3:
15) Key assumptions (pricing, conversion, rollout markets, eligibility)

### F) People (aim: NO TBDs here)
16) Stakeholders (names, not roles only):
   - Marketing lead:
   - Operations lead:
   - Commercial lead:
   - Finance lead(s) (DFU/FP&A/AR/AP if relevant):
17) Who needs to be **Consulted** vs **Informed** (optional list)

### G) Dependencies (teams + owners)
18) For each dependency, provide:
   - Team code
   - MD estimate (even rough)
   - Dependency description (what “done” means)
   - **Owner name**
   Collect for Phase 1 / Phase 2 / Phase 3.

### H) Product Risks (status + evidence)
19) For each risk area, answer:
   - Status: **Known evidence** / **Some evidence** / **Hypothesis only**
   - Link(s) if they exist (Figma, Miro, doc, dashboard)
   - If no link exists: what is the planned validation artifact (e.g. interviews, prototype test, fake-door, policy review)
   Risk areas:
   - Value
   - Usability
   - Feasibility
   - Viability

### I) Existing Links (if any)
20) Paste any existing links you already have:
   - Research / interviews:
   - User testing:
   - Architecture notes:
   - Dashboards:
   - Figma / Miro / FigJam:

---

## Writing Rules

- ExCo-readable, no jargon inflation
- Bullet points > long paragraphs
- Be explicit about uncertainty
- Keep **General Description** under **200 words**
- Ensure “People” and “Dependencies” sections have names whenever provided

---

## Output Rules (strict)

- Produce **only** the contents of `birth-certificate.md`
- Do NOT explain the template
- Do NOT add commentary before or after the document
- The output must be ready to share internally

---

<!-- ========================= -->
<!-- BIRTH CERTIFICATE TEMPLATE -->
<!-- ========================= -->

# Initiative Birth Certificate

**Initiative:** [[Initiative name]]  
**Start quarter:** [[YYYY QX]]  
**Planning horizon:** [[e.g. Q+0 to Q+2, or 3 quarters]]  
**Owner:** [[Single accountable owner]]  
**Status:** Draft | In Discovery | Approved | Parked | Killed  
**Last updated:** [[YYYY-MM-DD]]

---

## General Description (≤ 200 words)

**One-liner:** [[What is this initiative in one sentence?]]

**Target customers / users:** [[Primary persona(s) or segment(s)]]

**Customer problem / opportunity:**  
[[What job, pain, or unmet need are we addressing? What is broken or missing today?]]

**Proposed outcome:**  
[[What will be meaningfully different for the customer if we succeed?]]

**Business relevance:**  
[[Why this matters for the company now: revenue, retention, cost, risk, strategy, platform, regulation, etc.]]

**Constraints / assumptions:**  
[[Known constraints or assumptions: legal, platform, timing, contracts, technical, brand, etc.]]

---

## Hypothesis

If we deliver **[[capability / feature / change X]]**  
For **[[target user / customer segment]]**  
We expect **[[specific, observable behavior change]]**  
Which will move **[[primary KPI(s) / OKR(s)]]** by **[[direction and expected magnitude]]**  
By **[[relative time or milestone, e.g. end of first quarter after launch]]**  
Because **[[key insight, research finding, or evidence]]**.

_Open question(s) / assumptions to validate:_  
- [[Key assumption 1]]  
- [[Key assumption 2]]

---

## Main Metrics / Key Results

**Primary metric (north star for this initiative):**  
[[Metric name + precise definition]]

**Baseline:** [[Current value + timeframe]]  
**Target:** [[Target value + timeframe]]

**Guardrail metrics (must not degrade):**  
- [[Metric 1]]  
- [[Metric 2]]

**Post-runtime learning statement (to be completed after launch / experiment):**

After a runtime of **[[X weeks]]**, we saw **[[main metric]]** **[[increase/decrease]]** **[[significantly/not significantly]]**.  
This **[[validates / fails to validate]]** our hypothesis that **[[change]]** will cause **[[outcome]]**.  
We also observed **[[secondary metric(s)]]** go **[[up/down]]**.

---

## 💰 Impact

> Use ranges where uncertainty is high. State assumptions explicitly.

**Revenue impact (by year or phase):**
- **Year 1 / Phase 1:** [[€ / range]]
- **Year 2 / Phase 2:** [[€ / range]]
- **Year 3 / Phase 3:** [[€ / range]]

**Key assumptions:**  
- [[Pricing / adoption / conversion assumption]]  
- [[Market / rollout assumption]]

**Cost & investment (high level):**  
- Build cost (CapEx / MDs): [[…]]  
- Run cost (Opex): [[…]]  
- Opportunity cost: [[What is delayed or not done because of this?]]

---

## General Scope

**Customer-visible outcomes by end of initiative:**  
- [[Outcome 1]]  
- [[Outcome 2]]  
- [[Outcome 3]]

**Key deliverables:**  
- [[UI / API / workflow / integration / enablement / migration, etc.]]

**Definition of Done (high level):**  
[[What must be true in production to consider this initiative complete?]]

---

## 🗓️ Phased Scope & Milestones

### Phase 1 — [[e.g. Discovery / MVP / Pilot]] ([[Optional: YYYY QX]])
**Intended outcome(s):**
- [[…]]

**Key deliverables / milestones:**
- [[…]]

---

### Phase 2 — [[e.g. Beta / Expansion / Monetization]] ([[Optional: YYYY QX]])
**Intended outcome(s):**
- [[…]]

**Key deliverables / milestones:**
- [[…]]

---

### Phase 3 — [[e.g. Scale / Optimization]] ([[Optional: YYYY QX]])
**Intended outcome(s):**
- [[…]]

**Key deliverables / milestones:**
- [[…]]

---

## 🔗 Dependencies

**Team codes:** Use your organisation's team codes (e.g. MOBILE, BACKEND, DESIGN, PLATFORM, DATA, OPERATIONS, SECURITY, FINANCE, LEGAL, EXTERNAL)

### Phase 1
- [[TEAM CODE]] — [[XX]] MDs — [[Dependency description + what “done” means]] — Owner: [[Name]]

### Phase 2
- [[TEAM CODE]] — [[…]] MDs — [[…]] — Owner: [[Name]]

### Phase 3
- [[TEAM CODE]] — [[…]] MDs — [[…]] — Owner: [[Name]]

---

## Not in Scope

- [[Out-of-scope item 1]]  
- [[Out-of-scope item 2]]  
- [[Out-of-scope item 3]]

---

## Why Now?

**Trigger / context:**  
[[What changed? Market, customer demand, risk, regulation, contract, technology, strategy, etc.]]

**Cost of delay:**  
[[What gets worse if we wait?]]

**Strategic alignment:**  
[[Which strategic pillar / OKR / priority this supports]]

---

## 📎 Documentation Links

### Product Risks

| Risk Area | Evidence / Link | Notes / Mitigation |
|---------|----------------|--------------------|
| **Value** | [[link]] | [[…]] |
| **Usability** | [[link]] | [[…]] |
| **Feasibility** | [[link]] | [[…]] |
| **Viability** | [[link]] | [[…]] |

---

## Discovery Artifacts

| Artifact | Evidence / Link |
|--------|-----------------|
| Lean Product Canvas | [[figma / miro]] |
| Opportunity–Solution Tree | [[figma / miro]] |
| High-level solution architecture | [[link]] |
| Lo-fi design | [[link]] |
| Customer Journey (as-is / to-be) | [[link]] |
| Customer Journey Map | [[link]] |
| Personas | [[figma / miro]] |

---

## 👥 Stakeholders

**Marketing lead:** [[Name]]  
**Operations lead:** [[Name]]  
**Commercial lead:** [[Name]]  
**Finance:** [[Name(s)]]
