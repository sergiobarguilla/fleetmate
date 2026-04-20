# FleetMate Product OS

An **AI-native product workflow** built with Cursor — from product context and customer research to initiative, epics, and user stories. Fully version-controlled, traceable, and designed for real product teams.

Built by [Sergio Barguilla](https://linkedin.com/in/sergiobarguilla) — VP of Product | AI-native product leadership

---

## What this is

FleetMate is a **fictional fleet management SaaS** used as the demo product for a structured, end-to-end AI-assisted product workflow.

The workflow covers:

```
Product Context + Research
        ↓
  Initiative (Birth Certificate)
        ↓
  Discovery Review (AI Agent)
        ↓
         PRD
        ↓
       Epics
        ↓
   User Stories
```

Every artifact traces back to the one above it. Every requirement traces to a metric. Every metric traces to a persona.

---

## Why this workflow exists

Most AI-assisted product work is ad hoc: ChatGPT here, a summary there, no structure, no traceability.

This system changes that by:
- **Grounding AI in evidence** — Cursor reads your product context and research before generating anything
- **Enforcing discipline** — hard gates prevent discovery from being skipped; non-goals are explicit
- **Producing traceable artifacts** — every user story links to an epic, a PRD requirement, and a metric
- **Running inside your IDE** — no new tools; Cursor slash commands and agents are the interface

---

## Folder structure

```
fleetmate/
├── README.md                          ← you are here
├── product-context/                   ← stable product knowledge (the "brain")
│   ├── 00-index.md                    ← index + usage rules
│   ├── 01-one-pager.md                ← what the product is
│   ├── 02-strategy-and-okrs.md        ← strategic direction + OKRs
│   ├── 03-metrics-and-definitions.md  ← canonical metric definitions
│   ├── 04-personas-and-segments.md    ← user personas + jobs-to-be-done
│   └── 05-pricing-and-monetization.md ← business model + revenue assumptions
│
├── research/                          ← structured customer research
│   ├── master-summary.md              ← aggregated insights across all personas
│   └── readme.md                      ← how to add research rounds
│
├── initiatives/                       ← all initiative artifacts
│   └── 2026-Q3/
│       └── offline-navigation-mode/   ← example: full initiative artifact chain
│           ├── birth-certificate.md
│           ├── discovery-review.md
│           ├── prd/prd.md
│           ├── epics/epics.md
│           └── stories/user-stories.md
│
├── .cursor/
│   ├── commands/product/              ← Cursor slash commands
│   │   ├── create-initiative.md
│   │   ├── create-prd.md
│   │   ├── create-epics.md
│   │   ├── create-user-stories.md
│   │   ├── create-jira-import-csv.md
│   │   ├── create-discovery-learning-plan.md
│   │   └── create-discovery-summary.md
│   └── agents/
│       ├── product-discovery-thinking-agent.md  ← challenges assumptions
│       └── delivery-decomposition-agent.md       ← breaks initiatives into delivery structure
│
└── demo/
    ├── demo-script.md                 ← webinar script with timing
    └── demo-prompt.md                 ← pre-filled initiative questionnaire for live demo
```

---

## How to use this

### 1. Fork this repo

```bash
git clone https://github.com/sergiobarguilla/fleetmate-product-os
cd fleetmate-product-os
```

### 2. Open in Cursor

Open the `fleetmate/` folder as your Cursor workspace.

### 3. Adapt the product context

Replace the FleetMate product-context files with your own product:
- `product-context/01-one-pager.md` — your product
- `product-context/04-personas-and-segments.md` — your users
- `product-context/02-strategy-and-okrs.md` — your OKRs
- `product-context/03-metrics-and-definitions.md` — your canonical metrics

### 4. Add your research

Add customer research summaries to `research/master-summary.md`.  
Follow the structure in the existing file: Core Jobs, Pain Points, Behavioral Signals, Monetization Signals.

### 5. Create your first initiative

In Cursor chat, type:
```
/create-initiative
```

Add context:
```
@product-context/00-index.md @research/master-summary.md
```

Follow the questionnaire. The birth certificate will be generated in:
```
initiatives/{YYYY-QX}/{initiative-name}/birth-certificate.md
```

### 6. Run the full chain

| Step | Command | Output |
|------|---------|--------|
| 1 | `/create-initiative` | `birth-certificate.md` |
| 2 | `@product-discovery-thinking-agent` | `discovery-review.md` |
| 3 | `/create-discovery-learning-plan` | `discovery-learning-plan.md` |
| 4 | `/create-discovery-summary` | `discovery-summary.md` |
| 5 | `/create-prd` | `prd/prd.md` |
| 6 | `/create-epics` | `epics/epics.md` |
| 7 | `/create-user-stories` | `stories/user-stories.md` |
| 8 | `/create-jira-import-csv` | `jira/jira-import.csv` |

---

## Key principles

- **Intent before execution** — no PRD without a birth certificate
- **Discovery before commitment** — validate assumptions before writing requirements
- **Decisions before design** — resolve model choices before specifying solutions
- **AI derives, never invents** — constrained to product context and research evidence
- **Outputs are auditable** — every artifact traces to its inputs
- **Non-goals are explicit** — scope creep is prevented at the artifact level

---

## The example initiative

The `initiatives/2026-Q3/offline-navigation-mode/` folder contains a **complete, pre-generated artifact chain** for a real-world product initiative:

> **Problem:** FleetMate drivers lose navigation when mobile signal drops in tunnels, rural roads, and industrial zones — the #1 driver quality complaint for 3 consecutive quarters.

> **Initiative:** Enable full offline navigation and trip execution with automatic data sync on reconnect.

The full chain — birth certificate → discovery review → PRD → 4 epics → 9 user stories — is ready to read, adapt, and use as a reference.

---

## About the author

**Sergio Barguilla** — VP of Product | Managing Director | AI-native product leadership

15+ years building digital products in navigation, mobility, and SaaS. Currently independent and available for VP Product, Managing Director, and advisory engagements.

- LinkedIn: [linkedin.com/in/sergiobarguilla](https://linkedin.com/in/sergiobarguilla)
- This repo: [github.com/sergiobarguilla/fleetmate-product-os](https://github.com/sergiobarguilla/fleetmate-product-os)

---

*This repository was created as a companion to a product management webinar on AI-native workflows with Cursor. All product data (FleetMate) is fictional.*
