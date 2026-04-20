# Webinar Demo Script
## "AI-Native Product Management with Cursor"
### 45-minute format — Product Manager audience (technical)

---

## Pre-webinar setup checklist

- [ ] Cursor open with `fleetmate/` as the workspace root
- [ ] `demo-prompt.md` open in a text editor side-by-side (NOT in Cursor — prevents accidental indexing)
- [ ] `product-context/` and `research/` collapsed in sidebar (reveal them live)
- [ ] `initiatives/` folder EMPTY — you will generate live (or have the pre-generated version as fallback)
- [ ] Browser tab ready: GitHub repo URL
- [ ] Font size bumped up in Cursor for screen sharing legibility

---

## PART 1 — Introduction & Theory (8 minutes)

### Slide 1 — Who am I (1.5 min)

> "My name is Sergio Barguilla. I'm a VP of Product and Managing Director with 15+ years building digital products — navigation platforms, SaaS, fleet management software. For the last 18 months I've been going deep on what AI actually changes about how product teams work. Not the hype version. The version where you open a terminal and build something."

**Talking points:**
- Former VP Product at [company] — navigation, mobility, SaaS
- Now: independent, building AI-native workflows for product organizations
- Today I'm going to show you something I use every day — not a prototype

---

### Slide 2 — The problem with how PMs work today (2 min)

> "Most product teams are still working like it's 2015. Vision lives in a deck someone made two years ago. Research is scattered across Notion, Confluence, and a folder named 'interviews_final_v3'. And the initiative process? A PM fills in a Word template, sends it to their manager, and waits three days for comments."

**Talking points:**
- No single source of truth for product context
- Research and strategy are disconnected from daily work
- AI is used ad hoc — Copilot here, ChatGPT there — no structure, no traceability
- The problem isn't the AI tools. It's the absence of a system.

**Key line:** *"Prioritization is not focus. Focus is doing fewer things and doing them right."* (reference their slide)

---

### Slide 3 — The framework (2 min)

> "What I'm showing you today is an end-to-end product workflow that lives in a single folder, is version-controlled on GitHub, and uses Cursor as the AI brain. The chain is: Mission → OKRs → Initiatives → Epics → User Stories. Everything traces."

**Reference their diagram:** Vision → OKRs → Initiatives → Epics → Tasks

**Talking points:**
- Product Vision and Strategy define WHAT problems to solve
- Product Discovery figures out HOW to solve them (RICE, prototypes, research)
- Product Delivery builds the solution (epics, stories, Jira)
- Each layer is a file. Each file references the ones above it.
- Cursor knows all of them simultaneously.

---

### Slide 4 — What we're building today (2.5 min)

> "We're going to work on a fictional product called FleetMate — a fleet management SaaS for truck companies. It's simple enough that I can explain the whole business in 30 seconds, and complex enough to generate real artifacts."

**FleetMate in 30 seconds:**  
> "SMB trucking company. 5 to 100 trucks. Drivers need navigation, dispatchers need to coordinate, owners need to see costs. Today it's spreadsheets and WhatsApp. FleetMate replaces all of that."

**Transition:** "Let me open Cursor."

---

## PART 2 — Live Demo in Cursor (30 minutes)

---

### Step 1 — Show the workspace (2 min)

**Action:** Open Cursor, show the folder structure in the sidebar.

> "This is the workspace. Notice what's here: `product-context/`, `research/`, `.cursor/commands/`, `initiatives/`. This is the entire operating system for this product."

**Click on `product-context/00-index.md`:**
> "The index tells Cursor what files to read before generating any artifact. This is the brain. Every initiative starts here."

**Pause:** "No Jira. No Confluence. No Notion. One folder, Git-versioned, available to anyone on the team."

---

### Step 2 — Show product context (3 min)

**Open `product-context/01-one-pager.md`:**
> "The one-pager answers three questions: what is the product, who is it for, and why does it matter strategically. 60 lines. That's all Cursor needs to understand the product."

**Open `product-context/04-personas-and-segments.md`:**
> "Personas are not marketing fluff here. They're structured data. Jobs-to-be-done, pain points, what they value. Cursor will reference this when generating requirements."

**Open `product-context/02-strategy-and-okrs.md`:**
> "And OKRs. Not as a separate strategy deck that gets forgotten — as a file that every initiative is checked against."

---

### Step 3 — Show the research layer (3 min)

**Open `research/master-summary.md`:**
> "This is the market research layer. Driver pain points, dispatcher pain points, fleet owner pain points — structured, aggregated, append-only. When Cursor generates a birth certificate, it reads this. When it challenges assumptions in the discovery review, it references this."

**Highlight the Driver section:**
> "Look at this: 'Offline gaps — navigation fails in tunnels, industrial zones, rural roads.' This was gathered from real driver interviews. It's going to show up in the artifact we're about to generate."

**Pause:** "This is the thing most AI-assisted product processes miss. They use AI on top of nothing. We're grounding it in evidence."

---

### Step 4 — Create the initiative LIVE (5 min)

**Action:** Open Cursor chat (Cmd+L), add context tags:
> "@product-context/00-index.md @research/master-summary.md"

Then type: `/create-initiative`

> "The `/create-initiative` command is a Cursor slash command — it's a prompt template stored in `.cursor/commands/`. Watch what happens."

**When Cursor asks for initiative setup — paste BLOCK A from `demo-prompt.md`**

**When Cursor asks the questionnaire — paste BLOCK B from `demo-prompt.md`**

> "I'm going to paste my pre-prepared answers. In a real team setting, a PM would fill these in from their notes, stakeholder interviews, and the research they've already done."

**Watch the birth certificate generate.**

**Highlight two sections when done:**
1. The **Hypothesis:** > "Notice the structure: if we do X, for Y users, we expect Z behavior, which moves KPI, because of this evidence. That's not a wish list. That's a falsifiable hypothesis."
2. The **Risk table:** > "And here — Value, Usability, Feasibility, Viability risk. With evidence status. Hypothesis only vs. Some evidence vs. Known. Most teams skip this entirely."

---

### Step 5 — Show the discovery review (4 min)

**Option A (if time allows): Run the agent**
> "Now I'll run the discovery thinking agent. This is an AI agent whose only job is to challenge assumptions."

Type: `@product-discovery-thinking-agent` in chat, point at the birth certificate.

**Option B (if time is tight): Show pre-generated**
> "Here's what the agent produced when I ran it before the webinar."

**Open `initiatives/2026-Q3/offline-navigation-mode/discovery-review.md`**

**Highlight:**
- Top 3 risks section: > "Risk #1 is behavior change — will drivers actually download the route before departure? The tech might work perfectly and the metric is still zero if no one uses the feature."
- Decision Points: > "These are not assumptions to validate. These are forks in the road. Offline on all tiers or Growth-tier only? This changes the revenue model, the UX, and the competitive positioning entirely."

> "A junior PM would ship without answering these. A good PM team would debate them for two sprints. The agent surfaces them in 30 seconds."

---

### Step 6 — Show the PRD (5 min)

**Open `initiatives/2026-Q3/offline-navigation-mode/prd/prd.md`**

> "The PRD is generated from the birth certificate and the discovery review. But notice what it's NOT doing: it's not designing UI, it's not writing code, it's not estimating effort. It's defining what the system must do — and what it explicitly must NOT do."

**Highlight:**
- FR-3 (offline navigation): > "Functional requirements phrased as system capabilities. Independently testable."
- NFR section: > "CFR guardrail — 99.2%. That's a hard rollback threshold. Any release that drops below this gets rolled back regardless of feature completeness. The AI put it here because it read the metrics file."
- Scope Out section: > "Explicit non-goals. 'Live rerouting while offline.' This prevents scope creep before a single story is written."

---

### Step 7 — Epics and User Stories (5 min)

**Open `initiatives/2026-Q3/offline-navigation-mode/epics/epics.md`**

> "4 epics. Each one is outcome-oriented. EP-1 is the engine. EP-2 is the UX. EP-3 is the business model. EP-4 is instrumentation. Notice: instrumentation is an epic, not an afterthought."

**Open `initiatives/2026-Q3/offline-navigation-mode/stories/user-stories.md`**

**Show US-EP1-02:**
> "User story. Given/When/Then acceptance criteria. Dependencies. Instrumentation events. This is executable. A developer can read this and build it. A QA engineer can test it. A data engineer can know what events to expect."

**Scroll to show the volume:** > "9 user stories across 4 epics. From a blank chat window to this in about 40 minutes of real work."

---

### Step 8 — The full chain (3 min)

**Show the initiative folder structure:**
```
offline-navigation-mode/
├── birth-certificate.md     ← Why we're doing this
├── discovery-review.md      ← What could go wrong
├── prd/prd.md               ← What to build
├── epics/epics.md           ← How to organize delivery
└── stories/user-stories.md  ← What to build first
```

> "Every artifact traces back to the one above it. The user story traces to an epic, which traces to a PRD requirement, which traces to a discovery finding, which traces to the birth certificate hypothesis, which traces to the research. Full chain."

> "And all of this" — show sidebar — "is in a folder. On GitHub. I'll share the link in a moment."

---

## PART 3 — Wrap-up & CTA (7 minutes)

---

### What you saw (2 min)

> "Let me summarize what just happened. We defined a product context. Loaded structured customer research. Created an initiative from a questionnaire. Got an AI agent to challenge our assumptions. Generated a PRD, 4 epics, and 9 user stories with acceptance criteria. All grounded in the same evidence. All traceable."

> "This is not a ChatGPT experiment. This is a system. It has discipline — hard gates, explicit non-goals, instrumentation requirements. And it runs inside Cursor, which means any PM on the team can use it."

---

### GitHub (2 min)

**Show the GitHub repo in browser:**
> "Everything you saw today is on GitHub. The commands, the agents, the product context templates, and this full sample initiative. Fork it, adapt the product-context files to your product, and you can run this workflow with your own team next week."

**Share the link:** [github.com/sergiobarguilla/fleetmate-product-os]

> "The README walks you through the full workflow. Start with the product-context files. That's the hardest part — it forces you to get alignment on vision, personas, and metrics before you write a single requirement."

---

### Self-sell (3 min)

> "One last thing — and I'll be direct about it."

> "If what you saw today is relevant to how your product organization works — or how you'd like it to work — I'm available."

> "I help organizations build exactly this. Not just the tooling — the operating model. Getting alignment on product context. Structuring discovery so it actually informs delivery. Building AI-native workflows that reduce waste without removing the judgment that humans have to make."

> "I'm currently independent — not employed by a company. Open to VP Product, Managing Director, and advisory conversations. The work speaks for itself."

> "Connect with me on LinkedIn — I post about this every week. And if you have questions about anything you saw today, I'm happy to go deeper."

**Final slide:** LinkedIn URL + GitHub repo link

---

## Timing summary

| Block | Duration | Notes |
|-------|----------|-------|
| Intro + theory (4 slides) | 8 min | Keep slide 1 short — audience wants the demo |
| Show workspace + context | 5 min | Don't rush — set the mental model |
| Show research layer | 3 min | This is the "why it's different" moment |
| Live birth certificate generation | 5 min | The wow moment — let it run |
| Discovery review | 4 min | Use pre-generated if running long |
| PRD walkthrough | 5 min | Highlight requirements + guardrails |
| Epics + stories | 5 min | Show the Given/When/Then structure |
| Full chain summary | 3 min | The "aha" moment — show the folder |
| Wrap-up + GitHub | 2 min | Get the link on screen |
| Self-sell | 3 min | Be direct, not apologetic |
| **Total** | **43 min** | 2 min buffer for questions |

---

## Contingency plan

**If Cursor is slow or the AI output is poor:**
> "Sometimes the AI takes a moment — this is live, so bear with me. While it processes, let me show you what the finished artifact looks like."
→ Open the pre-generated birth certificate in `initiatives/2026-Q3/offline-navigation-mode/`

**If the command doesn't trigger:**
> "Let me paste the prompt directly."
→ Paste BLOCK B from `demo-prompt.md` directly into Cursor chat with the context tags

**If running over time:**
→ Skip Step 5 (discovery review) and jump directly to showing the pre-generated PRD
→ The core message still lands: birth certificate → PRD → stories
