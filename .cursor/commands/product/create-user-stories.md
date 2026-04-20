# create-user-stories

You are helping the user create **User Stories** for a product initiative.

This document breaks epics into **deliverable user stories** that are ready to be copied into Jira (or used as a source for Jira creation).

You are also responsible for explicitly identifying **whether each user story requires UI / UX design work**, and if so, what type of design work is needed.

---

## Preconditions (HARD GATE)

Before proceeding, confirm access to **all** of the following:

- `birth-certificate.md`
- `discovery-summary.md`
- `prd/prd.md`
- `epics/epics.md`
- `@product-context/00-index.md`

If any are missing, STOP and ask the user to provide them.

---

## What You Must Read (mandatory)

- PRD (requirements and constraints)
- Epic Plan (epic structure, scope in/out, sequencing, dependencies)
- Product context (personas, metrics definitions, glossary)
- Relevant research:
  - /research/master-summary.md
  - Persona-specific research folders (if applicable)

User stories must reflect validated persona pain points and real behavioral patterns from research.
If no research exists for the persona, proceed but do not invent behavioral assumptions.

---

## What You Must NOT Do

- Do NOT invent new product scope
- Do NOT design UI screens or technical implementation
- Do NOT estimate story points unless explicitly asked
- Do NOT browse the web

If details are missing:
- Use `[[TBD]]`
- Add `_Open question: ..._` under the relevant story

---

## Clarification Questions (optional, at most ONE message)

Ask only if needed to reduce TBDs:

1) Default story owner (name) and team (if you use teams)  
2) Preferred Jira labels/components (if any)  
3) Preferred story granularity (small / medium / large)  

If user doesn’t answer, proceed with:
- Owner: `[[TBD]]`
- Granularity: medium

---

## Folder & File Creation Rules

Create (if missing):

/initiatives/{YYYY-QX}/{initiative-name}/stories/user-stories.md

- Create the `stories/` folder if it does not exist
- Create `user-stories.md` as the first story artifact
- Do NOT create separate files per epic unless explicitly requested

---

## Writing Rules

- Stories must be **user/outcome-oriented**
- Keep each story independently deliverable
- Tie each story back to:
  - Epic ID
  - PRD requirement ID(s) (FR-x)
- Include acceptance criteria using **Given/When/Then**
- Include explicit dependencies if any
- Include at least one **instrumentation / measurement note** per epic (not per story unless critical)
- Explicitly declare whether **UI / UX design is required**
- Keep language consistent with glossary and personas

---

## Output Rules (STRICT)

- Produce **only** the contents of `stories/user-stories.md`
- No commentary before or after

---

<!-- ====================== -->
<!-- USER STORIES TEMPLATE -->
<!-- ====================== -->

# User Stories — [[Initiative name]]

**Initiative:** [[Initiative name]]  
**Owner:** [[Name]]  
**Last updated:** [[YYYY-MM-DD]]

**Related artifacts:**
- Birth Certificate
- Discovery Summary
- PRD
- Epic Plan

---

## 1. Story Map Overview

> Stories grouped by epic. Sequencing is suggestive and should follow the Epic Plan.

---

## 2. Stories by Epic

### EP-1 — [[Epic name]]

**Epic goal (from Epic Plan):** [[…]]  
**PRD coverage:** [[FR-x, FR-y]]

**Instrumentation / measurement notes (epic-level):**
- Track: [[event(s) / metric(s)]]
- Guardrails: [[CFR / ANR / other]]
- Reporting: [[where / dashboard link if known]]

---

#### US-EP1-01 — [[Story title]]

- **User story:**  
  As a [[persona]], I want [[capability]], so that [[outcome]].

- **Scope notes:** [[What is included / excluded]]
- **PRD requirement(s):** [[FR-x]]
- **Priority:** Must / Should / Could
- **Owner:** [[Name or TBD]]
- **Dependencies:** [[Team/system or None]]

- **Design required:** Yes / No
- **Design type:** 
  - New screen
  - Existing screen update
  - Component update
  - No UI (logic / backend)
- **Design intent (1–2 bullets):**
  - [[What user problem the design must solve]]
- **Design constraints (if any):**
  - [[Safety, platform, performance, brand, accessibility, etc.]]
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given [[context]]  
  When [[action]]  
  Then [[expected result]]

- Given [[context]]  
  When [[action]]  
  Then [[expected result]]

_Open question (if any):_ [[…]]

---

#### US-EP1-02 — [[Story title]]
*(Repeat as needed)*

---

### EP-2 — [[Epic name]]

**Epic goal:** [[…]]  
**PRD coverage:** [[…]]

**Instrumentation / measurement notes (epic-level):**
- Track: [[…]]
- Guardrails: [[…]]
- Reporting: [[…]]

---

#### US-EP2-01 — [[Story title]]

- **User story:**  
  As a [[persona]], I want [[capability]], so that [[outcome]].

- **Scope notes:** [[…]]
- **PRD requirement(s):** [[FR-x]]
- **Priority:** Must / Should / Could
- **Owner:** [[…]]
- **Dependencies:** [[…]]

- **Design required:** Yes / No
- **Design type:** 
  - New screen
  - Existing screen update
  - Component update
  - No UI (logic / backend)
- **Design intent (1–2 bullets):**
  - [[What user problem the design must solve]]
- **Design constraints (if any):**
  - [[…]]
- **Figma link:** [[TBD]]

**Acceptance criteria (Given/When/Then):**
- Given [[…]]  
  When [[…]]  
  Then [[…]]

_Open question (if any):_ [[…]]

---

*(Repeat for all epics in epics.md)*

---

## 3. Non-Functional Stories (only if required)

> Create NFR stories only when they require explicit work (instrumentation, performance, stability, policy compliance).

#### NFR-01 — [[Title]]

- **Statement:** [[Non-functional requirement expressed as deliverable work]]
- **Related to:** [[Epic(s) / FR(s)]]
- **Design required:** No
- **Acceptance criteria:**
  - Given [[…]] When [[…]] Then [[…]]

---

## 4. Dependency & Integration Stories (only if required)

> Use this section for cross-team integration work that doesn’t fit a single epic cleanly.

#### INT-01 — [[Title]]
- **Statement:** [[…]]
- **Depends on:** [[…]]
- **Design required:** No
- **Acceptance criteria:** [[…]]

---

## 5. Open Questions & Follow-Ups

- [[Question]]
- [[Question]]
- [[Question]]