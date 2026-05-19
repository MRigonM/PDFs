---
date: 2026-05-18
type: reference
tags: [reference, meta]
ai-first: true
---

# Claude Operating Manual — Rigon's Dev Second Brain

> Read this file before doing anything in this vault.
> This is the single source of truth for how Claude operates here.

---

## Section 0 — AI-First Vault Rule

This vault is designed for **future-Claude** to read and reason over. Rigon rarely reads notes directly — he calls Claude to retrieve, synthesize, and connect dots across coding sessions and projects.

Every note Claude writes must follow these rules:

1. **Self-contained context** — each note explains itself; don't rely on backlinks alone
2. **"For future Claude" preamble** — 2–3 sentence summary at top so Claude can judge relevance in 10 seconds
3. **Rich frontmatter** — `type`, `date`, `tags`, `ai-first: true`, plus type-specific fields
4. **Recency markers** — attach dates to external facts: "X was true (as of 2026-05)"
5. **Sources verbatim** — every external claim has its source URL inline
6. **Wikilinks mandatory** — every project, pattern, tool, or concept uses `[[wikilinks]]`
7. **Confidence levels** — mark claims as `stated | high | medium | speculation` where applicable

---

## Vault Identity

- **Owner:** Rigon
- **Purpose:** Dev-only knowledge base — decisions, solutions, patterns, session logs
- **Location:** `D:\.rigonsecondbrain\RigonSecondBrain`
- **Last updated:** 2026-05-18

---

## Folder Map

| Folder | Purpose |
|---|---|
| `Dev Logs/` | Session logs per project. Named `YYYY-MM-DD — <Project>.md`. The single source for what was worked on. |
| `Projects/TaskSphere/` | Architecture, decisions, bug log for TaskSphere |
| `Projects/StayEase/` | Architecture, decisions, bug log for StayEase |
| `Knowledge/` | Reusable patterns, concepts, and solutions across projects |
| `Inbox/` | Raw dumps: errors, snippets, questions. Should be empty at session end. |

---

## Key Files

- `[[index]]` — vault catalog; read first when navigating
- `[[log]]` — chronological record of every vault operation
- `[[Dashboard]]` — Dataview dashboard: active projects, recent sessions, knowledge, decisions
- `[[CRITICAL_FACTS]]` — owner name, email, timezone

---

## Session Protocol

### On session start:
1. Read the latest note in `Dev Logs/`
2. Read `[[Projects/TaskSphere/decisions]]` or `[[Projects/StayEase/decisions]]` as relevant
3. Check `Inbox/` for anything unprocessed

### On session end:
1. Log what was worked on → `Dev Logs/YYYY-MM-DD — <Project>.md`
2. Move decisions → `Projects/<name>/decisions.md`
3. Move reusable patterns → `Knowledge/`
4. Append to `[[log]]`

---

## Auto-Save Rules

Claude should auto-save **without asking**:
- Decisions made in conversation → project `decisions.md` + dev log
- Bugs solved → project `bugs.md` + dev log
- Reusable patterns → `Knowledge/` + dev log
- Dev session work → `Dev Logs/YYYY-MM-DD — <Project>.md`

Claude should **ask before saving**:
- Anything personal or non-coding
- Deleting or archiving an existing note

---

## Note Rules

- `[[wikilink]]` every related note
- Tags: `#bug` `#solved` `#decision` `#pattern` `#question`
- For decisions: always record WHY, not just what
- No bloated notes — one idea per note when possible

## Naming Conventions

- Dev logs: `YYYY-MM-DD — <Project>.md`
- Decisions: `decisions.md` (one per project, append-only)
- Patterns: descriptive title, no date prefix (e.g., `repository-pattern.md`)
- Bug logs: `bugs.md` (one per project, append-only)

---

## Frontmatter Requirements

```yaml
---
date: YYYY-MM-DD
type: <note-type>
tags: [<type>, ...]
ai-first: true
---
```

Note types: `project` | `decision` | `pattern` | `devlog` | `bug` | `reference`

---

## Active Projects

- `[[Projects/TaskSphere/TaskSphere]]` — active
- `[[Projects/StayEase/StayEase]]` — active (no sessions yet)

---

## What NOT to Save

- Anything personal
- Trivial tasks already solved and forgotten
- Notes you'll never query again

---

## Do Not Touch

- `Inbox/README.md` — placeholder file, do not modify
