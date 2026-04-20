# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

FleetMate is an **AI-native product workflow system** — not a software codebase. It contains no code to build, test, or deploy. Instead, it is a structured set of markdown artifacts and AI agent/command definitions that guide product work from strategy through delivery planning.

The fictional product (FleetMate fleet management SaaS) is used as a demo substrate. All product data is illustrative.

---

## Artifact Chain

Every initiative must follow this sequence — no step can be skipped:

```
/create-initiative          → birth-certificate.md
@product-discovery-agent    → discovery-review.md
/create-discovery-learning-plan → discovery-learning-plan.md
/create-discovery-summary   → discovery-summary.md
/create-prd                 → prd/prd.md
/create-epics               → epics/epics.md
/create-user-stories        → stories/user-stories.md
/create-jira-import-csv     → jira/jira-import.csv
```

Output location convention: `initiatives/{YYYY-QX}/{initiative-name}/`

---

## Mandatory Context Before Any Artifact

Before generating any initiative artifact, always read these files in order:

1. `product-context/01-one-pager.md` — what the product is
2. `product-context/04-personas-and-segments.md` — who is affected
3. `product-context/02-strategy-and-okrs.md` — strategic alignment
4. `product-context/03-metrics-and-definitions.md` — use exact metric names (never invent new ones)

The index at `product-context/00-index.md` summarizes these rules.

Also read `research/master-summary.md` and relevant persona folders under `research/` before generating any artifact that involves hypotheses, assumptions, or user behavior.

---

## Cursor Commands (`.cursor/commands/product/`)

Each file defines a slash command's full behavior including questioning rules, output format, and file creation rules. When asked to run a command, follow its instructions exactly — including the strict output format and "do not add commentary" rules.

Key constraints shared across all commands:
- Never browse the web
- Never invent metrics, names, revenue figures, or scope
- Use `[[TBD]]` for unknowns, not placeholders or guesses
- Output only the artifact content — no preamble or explanation

---

## Agent Definitions (`.cursor/agents/`)

- **`product-discovery-thinking-agent.md`** — challenges assumptions in a birth certificate; produces a structured discovery review. Acts as a critical reviewer, not a solution designer. Requires `birth-certificate.md` and `product-context/00-index.md` as inputs.
- **`delivery-decomposition-agent.md`** — decomposes a PRD into epics with sequencing and dependencies. Requires `prd/prd.md`, `birth-certificate.md`, `discovery-summary.md`, and product context.

Both agents have strict output formats defined in their files — follow them exactly.

---

## Key Principles

- **Intent before execution** — no PRD without a birth certificate
- **Discovery before commitment** — validate assumptions before writing requirements  
- **AI derives, never invents** — all outputs must be grounded in product context and research
- **Non-goals are explicit** — every artifact has an explicit "Not in Scope" section
- **Traceability** — every user story links to an epic → PRD requirement → metric → persona
