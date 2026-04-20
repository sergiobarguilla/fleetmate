# create-epics

You are helping the user create an **Epic Plan** for a product initiative.

This document converts a PRD (and the Delivery Decomposition output) into a set of **delivery epics** that are ready to be broken into user stories.

---

## Preconditions (HARD GATE)

Before proceeding, confirm access to **all** of the following:

- `birth-certificate.md`
- `discovery-summary.md`
- `prd/prd.md`
- Output from `delivery-decomposition-agent` (can be pasted text or a file)
- `@product-context/00-index.md`

If any are missing, STOP and ask the user to provide them.

---

## What You Must Read (mandatory)

- PRD (functional + non-functional requirements)
- Discovery Summary (assumptions, residual risks)
- Delivery Decomposition output (proposed epic structure and sequencing)
- Product context (glossary / constraints as needed)

---

## What You Must NOT Do

- Do NOT invent new scope beyond PRD
- Do NOT write user stories or acceptance criteria
- Do NOT design UI or implementation
- Do NOT estimate story points
- Do NOT browse the web

If something is unclear, leave `[[TBD]]` and add `_Open question: ..._`.

---

## Questioning Rules

Ask **one single clarification message only if needed**, prioritizing:
- Epic owners (names)
- Dependency owners (names)
- Epic success metric per epic (1 metric max)

Otherwise proceed.

---

## Folder & File Creation Rules

Create (if missing):

/initiatives/{YYYY-QX}/{initiative-name}/epics/epics.md

- Create the `epics/` folder if it does not exist
- Create `epics.md` as the first epic artifact
- Do NOT create user story files yet

---

## Writing Rules

- Epics must be **outcome-oriented**
- Each epic should map to PRD requirements (FR-IDs)
- Keep epics small enough to deliver incrementally
- Include sequencing and dependencies
- Ensure owners are captured where provided

---

## Output Rules (STRICT)

- Produce **only** the contents of `epics/epics.md`
- No commentary before or after

---

<!-- ================= -->
<!-- EPICS TEMPLATE -->
<!-- ================= -->

# Epic Plan — [[Initiative name]]

**Initiative:** [[Initiative name]]  
**Owner:** [[Name]]  
**Last updated:** [[YYYY-MM-DD]]

**Related artifacts:**
- Birth Certificate
- Discovery Summary
- PRD
- Delivery Decomposition

---

## 1. Epic Overview

| Epic ID | Epic Name | Outcome / Goal | PRD Coverage | Primary Owner | Depends On |
|-------|-----------|----------------|-------------|--------------|------------|
| EP-1 | [[Name]] | [[Outcome]] | [[FR-x, FR-y]] | [[Name]] | [[None / EP-x / External]] |
| EP-2 | [[Name]] | [[…]] | [[…]] | [[…]] | [[…]] |
| EP-3 | [[Name]] | [[…]] | [[…]] | [[…]] | [[…]] |

---

## 2. Epic Details

### EP-1 — [[Epic name]]

**Outcome / Goal:**  
[[Outcome-focused statement]]

**Problem slice addressed:**  
[[What part of the problem this epic solves]]

**Scope (in):**
- [[…]]
- [[…]]

**Scope (out):**
- [[…]]
- [[…]]

**PRD requirements covered:**
- FR-x
- FR-y

**Non-functional considerations (if relevant):**
- [[CFR/ANR guardrails, performance, policy constraints]]

**Key dependencies:**
- [[Team/system + what “done” means]]
- [[Dependency owner]]

**Risks / assumptions carried into delivery:**
- [[Assumption or risk]]  
  _Mitigation / note: [[…]]_

**How we measure success (one metric max):**
- [[Metric + expected direction]]

**Epic exit criteria (high level):**
- [[What must be true in prod to consider this epic complete]]

---

### EP-2 — [[Epic name]]
*(Repeat as needed)*

---

## 3. Sequencing & Milestones (High Level)

**Proposed execution order:**
1. EP-x — [[Rationale]]
2. EP-y — [[Rationale]]
3. EP-z — [[Rationale]]

**Parallelization opportunities:**
- [[Epic A]] can run in parallel with [[Epic B]] because [[…]]

**Key milestone points (optional):**
- [[Milestone]] — [[Epic(s)]]

---

## 4. Cross-Team Dependency Matrix

| Team / System | Dependent Epics | Dependency Summary | Owner |
|--------------|----------------|--------------------|-------|
| [[Team]] | [[EP-x]] | [[…]] | [[Name]] |
| [[Team]] | [[…]] | [[…]] | [[…]] |

---

## 5. Open Questions

- [[Open question]]  
- [[Open question]]  
- [[Open question]]