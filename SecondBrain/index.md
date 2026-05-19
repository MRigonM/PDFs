---
date: 2026-05-18
type: index
tags: [index]
ai-first: true
---

## For future Claude
This is the vault catalog. Read this file first when navigating the vault — cheaper and faster than searching. Lists every note grouped by folder with a one-line description.

---

# Vault Index

*Last regenerated: 2026-05-19*

---

## Root

- [[_CLAUDE.md]] — single operating manual: vault identity, folder map, session protocol, auto-save rules, note rules
- [[index]] — this file; full catalog of all vault notes by folder
- [[log]] — chronological activity log of every vault operation
- [[CRITICAL_FACTS]] — owner name, email, timezone, active projects
- [[Dashboard]] — Dataview dashboard: active projects, recent dev logs, knowledge, decisions

---

## Templates/

- [[Templates/Dev Log]] — template used by `/obsidian-log` for new dev session notes

---

## Inbox/

- [[Inbox/README]] — placeholder; raw dump zone for errors, snippets, questions

---

## Projects/

### TaskSphere/
- [[Projects/TaskSphere/TaskSphere]] — project overview: stack (.NET 9, Angular 21, SignalR, EF Core), features, recent activity
- [[Projects/TaskSphere/decisions]] — append-only decision log: chat panel, routing, SignalR auth, scroll behavior

### StayEase/
- [[Projects/StayEase/StayEase]] — project scaffold (no sessions logged yet)
- [[Projects/StayEase/decisions]] — append-only decision log (empty, awaiting first session)

---

## Knowledge/

- [[Knowledge/signalr-jwt-websocket-auth]] — JWT auth pattern for SignalR WebSocket (token via query string)
- [[Knowledge/signalr-group-per-entity]] — Group-per-entity broadcast pattern for SignalR
- [[Knowledge/angular-clipboard-image-paste]] — Angular clipboard paste + file picker upload pattern
- [[Knowledge/aspnetcore-secure-file-upload]] — Secure upload endpoint + PhysicalFileProvider fix
- [[Knowledge/angular-scroll-to-bottom]] — effect() + setTimeout(0) scroll pattern for Angular
- [[Knowledge/angular-viewport-height-layout]] — h-screen flex layout pattern for full-viewport Angular apps
- [[Knowledge/unit-of-work-readonly-split]] — IReadOnlyUnitOfWork/IUnitOfWork interface split pattern for enforcing read-only access in validation services

---

## Dev Logs/

- [[Dev Logs/2026-05-19 — TaskSphere]] — UnitOfWork refactor: repos as lazy UoW properties, IReadOnlyUnitOfWork interface split
- [[Dev Logs/2026-05-18 — Vault]] — Vault maintenance: graph colors, Dataview dashboard, restructure, skill fixes
- [[Dev Logs/2026-05-18 — TaskSphere]] — Implemented chat side panel: ChatPanelService, app shell layout, sub-header wiring
- [[Dev Logs/2026-05-17 — TaskSphere]] — Options evaluated, Option A chosen, implementation plan written
- [[Dev Logs/2026-05-04 — TaskSphere]] — SignalR chat feature, image upload, static files fix

---

*Claude: when new notes are added, append them to the correct section above.*
