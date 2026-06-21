# PROJECT.md — FleetMate Product OS

- **Slug:** fleetmate
- **Type:** tooling
- **Status:** live
- **Repo (git remote — never a local path):** git@github-sergiobarguilla:sergiobarguilla/fleetmate.git
- **Live URL:** https://github.com/sergiobarguilla/fleetmate
- **Belongs to:** [[01 - AI Track]]
- **Last updated:** 2026-06-05

## What it is
A fully structured, AI-native product workflow system built in Cursor — from product context and customer research through initiative, PRD, epics, and user stories. Uses a fictional fleet management SaaS (FleetMate) as the demo substrate. Built to demonstrate that disciplined, traceable product work can run inside an IDE with no new tools.

## Current state
Complete end-to-end artifact chain implemented and demonstrated. The `initiatives/2026-Q3/offline-navigation-mode/` folder contains a full reference chain: birth certificate → discovery review → PRD → epics → user stories. Cursor slash commands and two agent definitions are production-ready.

## Key decisions & trade-offs
- Fictional product used deliberately — separates the workflow from any real IP or NDA concerns, making it fully shareable.
- Cursor-native (slash commands + agents) — no external tooling dependency; any team can fork and adapt.
- Hard gates at each artifact step prevent skipping discovery — a deliberate constraint that forces intent before execution.
- `[[TBD]]` convention for unknowns — prevents AI hallucination of metrics or scope.

## Learnings worth sharing
- AI-native product workflows need discipline architecture, not just prompts — structure is the differentiator.
- Traceable artifacts (every user story → epic → PRD requirement → metric → persona) are achievable with markdown alone.
- Built as a webinar companion; strong resonance with product teams looking to operationalise AI without tool sprawl.

## Stack / architecture
Pure markdown + Cursor slash commands and agent definitions — no code, no build system.
