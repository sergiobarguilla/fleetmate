# create-jira-import-csv

You are helping the user generate a **Jira-ready CSV import file** from the initiative artifacts.

The CSV must be suitable for Jira’s **CSV import** and represent:
- Epics
- Stories linked to those epics
- Optional tasks (NFR/INT) if present

---

## Preconditions (HARD GATE)

Before proceeding, confirm access to:

- `prd/prd.md`
- `epics/epics.md`
- `stories/user-stories.md`

If any are missing, STOP and ask the user to provide them.

---

## What You Must Read (mandatory)

- `epics/epics.md` (Epic IDs, names, goals, owners)
- `stories/user-stories.md` (Story IDs, titles, persona, AC, dependencies, owner, priority, PRD mapping)
- `prd/prd.md` (only to cross-check requirement IDs like FR-x; do not invent requirements)

---

## What You Must NOT Do

- Do NOT invent scope, owners, dates, estimates
- Do NOT browse the web
- Do NOT assume Jira field configuration beyond generic defaults
- Do NOT include sensitive data

If a value is missing, leave it blank (not `[[TBD]]`) in the CSV.

---

## Clarification Questions (at most ONE message)

If not provided in existing docs, ask the user:

1) Jira **Project Key** (e.g., NAV)  
2) Issue Type names used in their Jira (e.g., “Epic”, “Story”, “Task”)  
3) Whether they use a specific field for epic linking:
   - Option A: `Epic Link`
   - Option B: `Parent`
   - Option C: “Not sure” → default to `Epic Link`

If user doesn’t answer, proceed with:
- Issue Types: Epic / Story / Task
- Epic linking column: `Epic Link`

---

## Output Rules (STRICT)

- Produce **one file content only**: `jira/jira-import.csv`
- Output must be valid CSV (comma-separated, quoted where needed)
- No commentary before or after
- Ensure acceptance criteria and descriptions preserve line breaks using `\n`

---

## Folder & File Creation Rules

Create (if missing):

/initiatives/{YYYY-QX}/{initiative-name}/jira/jira-import.csv

---

## CSV Format Rules

### Required columns (generic Jira-compatible)
Include these columns in this order:

1. `Issue Type`
2. `Summary`
3. `Description`
4. `Epic Name`               (used for Epic creation)
5. `Epic Link`               (used to link Stories → Epic by Epic Name)
6. `Priority`
7. `Assignee`
8. `Labels`

### Mapping rules

**Epics**
- `Issue Type` = Epic
- `Summary` = `EP-x — <Epic Name>`
- `Epic Name` = `<Epic Name>` (MUST be populated for epics)
- `Epic Link` = empty

**Stories**
- `Issue Type` = Story
- `Summary` = `US-EPx-yy — <Story title>`
- `Epic Link` = `<Epic Name>` (must match exactly an epic’s `Epic Name`)
- `Epic Name` = empty

**Tasks (NFR/INT, if included)**
- `Issue Type` = Task
- If linked to a specific epic, set `Epic Link` to that epic’s name, else leave blank

**Labels**
Always include:
- `initiative_<initiative-name>`
- `source_cursor`
Optionally include:
- `fr_FR-1` (one label per FR mapping if present)
- `ep_EP-1` (one label per epic id)
- `prio_must|should|could` (derived from Priority field)

---

## Description construction

### For Epics (Description)
Include:
- Epic goal
- Scope in/out (if available)
- PRD coverage (FR-x list)
- Dependencies (teams/systems)
- Success metric (if stated)
- Risks/assumptions (brief)

### For Stories (Description)
Include:
- User story statement
- Scope notes
- PRD requirement mapping
- Dependencies
- Acceptance criteria (Given/When/Then)
- Open questions (if any)

Use `\n` for line breaks. Keep it concise but complete.

---

<!-- ================= -->
<!-- OUTPUT: CSV ONLY -->
<!-- ================= -->