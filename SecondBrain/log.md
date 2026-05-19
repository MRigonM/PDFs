---
date: 2026-05-03
type: log
tags: [log]
ai-first: true
---

## For future Claude
Chronological activity log of every vault operation. Each entry records the date, the command or action, and what changed. Read this to understand what has happened in the vault without scanning every note. Append — never overwrite.

---

# Vault Activity Log

Format: `## [YYYY-MM-DD] action | Description`

---

## [2026-05-18] devlog | Logged vault maintenance session

- Created `Dev Logs/2026-05-18 — Vault.md` — full session log: graph colors, Dataview dashboard, restructure, skill fixes

## [2026-05-18] restructure | Simplified vault for pure dev use

- Deleted `Daily/` folder (4 notes) — all content was thin wrappers pointing to Dev Logs; redundant
- Deleted `CLAUDE.md` — merged into `_CLAUDE.md` as single operating manual
- Updated `_CLAUDE.md` — session protocol now uses `Dev Logs/` as sole session log; merged Note Rules and What NOT to Save
- Updated `index.md` — removed Daily section
- Updated `graph.json` — removed Daily color group

## [2026-05-18] organize | Vault structure cleanup

- Created `Projects/StayEase/StayEase.md` + `Projects/StayEase/decisions.md` (proper folder, scaffolded)
- Deleted empty stubs: `Projects/StayEase.md`, `Projects/TaskSphere.md`, `SignalR.md`, `wikilink.md`
- Updated `_CLAUDE.md` — added `Dev Logs/` to folder map, updated active project status
- Regenerated `index.md` — now reflects all 6 Knowledge notes, both projects, all Dev Logs

## [2026-05-18] devlog | Implemented TaskSphere chat side panel

- Created `Dev Logs/2026-05-18 — TaskSphere.md` — full implementation: ChatPanelService, ChatPanelComponent, app shell layout, sub-header wiring, ChatPageComponent deleted
- Created `Knowledge/angular-scroll-to-bottom.md` — effect + setTimeout(0) scroll pattern
- Created `Knowledge/angular-viewport-height-layout.md` — h-screen flex layout pattern
- Updated `Projects/TaskSphere/TaskSphere.md` — recent activity
- Appended `Projects/TaskSphere/decisions.md` — 7 decisions logged
- Created `Daily/2026-05-18.md`

## [2026-05-17] devlog | Brainstormed + planned TaskSphere chat side panel feature

- Created `Dev Logs/2026-05-17 — TaskSphere.md` — options evaluated, Option A chosen, plan written
- Updated `Projects/TaskSphere/TaskSphere.md` with planned feature + new activity entry
- Updated `Daily/2026-05-17.md` with brainstorming work + decision

## [2026-05-17] save | Extracted knowledge patterns from TaskSphere chat image support

- Created `Knowledge/angular-clipboard-image-paste.md` — Angular clipboard paste + file picker upload pattern
- Created `Knowledge/aspnetcore-secure-file-upload.md` — Secure upload endpoint + PhysicalFileProvider fix
- Created `Daily/2026-05-17.md` — daily note

## [2026-05-04] devlog | Updated TaskSphere dev log — session 2 (chat UI + image upload + static files fix)

## [2026-05-04] save | Full conversation save — patterns extracted

- Created `Knowledge/signalr-jwt-websocket-auth.md` — JWT auth pattern for SignalR WebSocket
- Created `Knowledge/signalr-group-per-entity.md` — Group-per-entity broadcast pattern
- Updated `Daily/2026-05-04.md` with knowledge links

## [2026-05-04] devlog | Logged TaskSphere team chat session

- Created `Dev Logs/2026-05-04 — TaskSphere.md` — full SignalR chat feature implementation
- Created `Projects/TaskSphere/TaskSphere.md` — project note with recent activity
- Created `Daily/2026-05-04.md` — daily note linking to dev log

## [2026-05-03] daily | Created daily note for 2026-05-03 — first session

## [2026-05-03] init | Vault initialized with _CLAUDE.md, index.md, log.md

- Generated `_CLAUDE.md` operating manual from vault scan
- Generated `index.md` catalog of all current vault notes
- Generated `log.md` (this file)
- Vault state at init: scaffold only — Inbox/, Projects/TaskSphere/, Knowledge/, Daily/ folders exist but contain only README placeholders
- obsidian-second-brain skill installed from `D:/GitHub/~/.claude/skills/obsidian-second-brain/`

## [2026-05-19] devlog | UnitOfWork refactor — TaskSphere

- Created `Dev Logs/2026-05-19 — TaskSphere.md` — full session: repos moved into UoW lazy-init properties, IReadOnlyUnitOfWork split, all services updated, DI cleaned, build green
- Appended `Projects/TaskSphere/decisions.md` — 6 decisions logged (UoW refactor)
- Created `Knowledge/unit-of-work-readonly-split.md` — IReadOnlyUnitOfWork/IUnitOfWork interface split pattern
- Updated `Projects/TaskSphere/TaskSphere.md` — recent activity entry added
