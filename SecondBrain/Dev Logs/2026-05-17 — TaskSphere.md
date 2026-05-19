---
date: 2026-05-17
type: devlog
tags: [devlog, tasksphere, angular, chat, signalr, side-panel, brainstorming, planning]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
session-duration: ~1 session
---

## For future Claude

This session was a brainstorming + planning session for [[Projects/TaskSphere/TaskSphere]]. No code was written. The team chat feature (currently a full-page route at `/chat/:projectId`) was redesigned as a persistent side panel. Three implementation options were evaluated; Option A (ChatPanelService + layout split) was chosen. A full implementation plan was written and saved to `docs/superpowers/plans/2026-05-17-chat-side-panel.md` in the repo.

---

## Feature: Chat Side Panel

### Problem
The existing chat is a full-page route (`/chat/:projectId`). The user wants it accessible as a side panel that slides in without losing context of the current page.

### Requirements gathered
- Panel slides in from the right, **pushes** main content left (not an overlay)
- Shows **current project's chat** only (no project switcher inside the panel)
- Supports **fullscreen mode** (expand to full width, hide main content)
- Toggle via the "Chat" button in the sub-header

### Options evaluated

| Option | Approach | Verdict |
|---|---|---|
| **A** | `ChatPanelService` signals + layout flex split | ✅ Chosen — simplest, no new deps |
| **B** | Angular auxiliary (named) router outlet | ❌ Routing complexity, URL changes on toggle |
| **C** | Angular CDK Overlay | ❌ Overkill, pulls in new dependency |

### Decisions made

1. **Option A chosen** — `ChatPanelService` with `isOpen`, `isFullscreen`, `projectId` signals. Sub-header calls `toggle()` instead of navigating. `ChatPanelComponent` mounts in the app shell via `effect()`.
2. **Panel width: 380px normal, 100% fullscreen** — fixed width (not %) so it doesn't grow awkwardly on wide screens.
3. **projectId flows through ChatPanelService** — sub-header sets it via `setProjectId()` on every navigation. Panel reads it. Avoids duplicating router listening.
4. **ChatPageComponent deleted** — the panel replaces it entirely. Route `/chat/:projectId` removed.
5. **No access check on panel mount** — `ChatService.connect()` already enforces membership at the hub level; the panel doesn't need its own guard.

### Implementation plan

Saved to `docs/superpowers/plans/2026-05-17-chat-side-panel.md`. Five tasks:

1. `ChatPanelService` (signals + methods)
2. `ChatPanelComponent` (panel UI — reuses chat logic from deleted `ChatPageComponent`)
3. App shell layout update (`app.html` flex row)
4. Sub-header update (Chat link → button calling `toggle()`)
5. Delete `ChatPageComponent` + remove route

### Files affected
- **Create**: `chat-panel.service.ts`, `chat-panel.component.ts/html`
- **Modify**: `app.ts`, `app.html`, `sub-header.component.ts/html`, `app.routes.ts`
- **Delete**: `chat-page.component.ts/html`
- **Backend**: no changes needed