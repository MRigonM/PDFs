---
date: 2026-05-18
type: decisions
tags: [decisions, tasksphere, angular, chat, side-panel]
ai-first: true
---

## For future Claude

This file logs architectural and implementation decisions made during [[Projects/TaskSphere/TaskSphere]] development. Append new entries — never overwrite existing ones.

---

## [2026-05-18] Chat side panel implementation

- **Panel approach:** `ChatPanelService` signals + app shell flex row (Option A). Rejected: Angular auxiliary router outlet (routing complexity), CDK Overlay (overkill/new dep).
- **Panel widths:** 380px normal, 600px expanded. Rejected: `width: 100%` for expanded — broken in flex row context (fights with `flex-1` main content).
- **projectId flow:** `SubHeaderComponent` calls `chatPanel.setProjectId()` on every NavigationEnd. Panel reads it via signal. No duplicate router listening.
- **ChatPageComponent deleted:** Route `/chat/:projectId` removed. Old URL redirects to home via wildcard.
- **No access check on panel mount:** `ChatService.connect()` enforces membership at hub level.
- **Auto-scroll:** `effect()` + `setTimeout(..., 0)` — defers scroll until after Angular DOM write. `ngAfterViewChecked` rejected (fires too frequently).
- **Message grouping reverted:** Implemented but removed at user request.

## [2026-05-19] UnitOfWork + Repository refactor

- **Repos moved into UoW:** All repositories accessed via `IUnitOfWork` properties instead of separate DI injections. Reason: single constructor injection point, less DI noise, repos share one DbContext scope automatically.
- **Lazy init via ??=:** `private IProjectRepository? _projects; public IProjectRepository Projects => _projects ??= new ProjectRepository(_context);` — repos only instantiated when actually used in a request.
- **No catch(Exception e) { throw e; } blocks:** These swallow the stack trace and add no value. Skipped.
- **IReadOnlyUnitOfWork interface split:** Created `IReadOnlyUnitOfWork` with 5 read-only repo properties. `IUnitOfWork` extends it and adds `ChatMessages` + save methods. `TaskValidationService` and `SprintValidationService` inject `IReadOnlyUnitOfWork` — `SaveChanges` is not callable from validation layer. Reason: validation services only read data; exposing write methods is unnecessary surface area.
- **UnitOfWork registered for both interfaces:** `services.AddScoped<IUnitOfWork, UnitOfWork>()` and `services.AddScoped<IReadOnlyUnitOfWork, UnitOfWork>()` — same concrete type, DI creates separate scoped instances per request (no shared state issue).
- **ChatMessages on IUnitOfWork only (not IReadOnlyUnitOfWork):** ChatMessages excluded from read-only interface because ChatService writes. The 5 read-only repos (Projects, Tasks, Sprints, Members, Companies) are the cross-cutting read concern.