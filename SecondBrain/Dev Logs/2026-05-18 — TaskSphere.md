---
date: 2026-05-18
type: devlog
tags: [devlog, tasksphere, angular, chat, signalr, side-panel, signals]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
session-duration: ~1 session
---

## For future Claude

This session implemented the chat side panel planned on 2026-05-17 for [[Projects/TaskSphere/TaskSphere]]. All five tasks from `docs/superpowers/plans/2026-05-17-chat-side-panel.md` were completed and `ChatPageComponent` was deleted. Several post-implementation fixes were needed around panel height, auto-scroll, and expanded width — details below.

---

## Feature: Chat Side Panel — Implementation

### What was built

#### 1. `ChatPanelService`
Angular [[Signals]] service with three signals: `isOpen`, `isFullscreen`, `projectId`. Methods: `toggle()`, `close()`, `toggleFullscreen()`, `setProjectId()`. Written TDD-first: 6 tests written and passing before implementation.

#### 2. `ChatPanelComponent`
Replaces the deleted `ChatPageComponent`. Mounts in the app shell. Connects `[[SignalR]]` via `ChatService` using an `effect()` that fires whenever `projectId` changes. Clipboard paste and file picker image upload ported from the old component.

#### 3. App shell layout (`app.html`)
- Outer wrapper: `min-h-screen` → `h-screen overflow-hidden`
- `<main>` and `<app-chat-panel>` wrapped in `div.flex.flex-1.overflow-hidden` for side-by-side layout

#### 4. Sub-header update
Chat `<a routerLink>` replaced with `<button>` calling `chatPanel.toggle()`. `SubHeaderComponent` now injects `ChatPanelService` and calls `setProjectId()` on every navigation to keep the panel's projectId in sync.

#### 5. Cleanup
`ChatPageComponent` deleted, route `chat/:projectId` removed from `app.routes.ts`.

---

### Post-implementation fixes

| Issue | Fix |
|---|---|
| Messages area had no scroll boundary | Added `h-full` to panel div so `overflow-y-auto` had a real container height |
| New messages didn't auto-scroll | `effect(() => { messages(); setTimeout(() => scrollToBottom(), 0); })` — `setTimeout` defers scroll until after Angular renders the new DOM node |
| Reopening panel didn't scroll to bottom | Second `effect()` watches `isOpen()`, scrolls when panel opens — needed because `@if` destroys/recreates the container |
| Expanded width broke in flex row | Changed fullscreen width from `100%` to `600px`; normal: `380px`, expanded: `600px` |

> Message grouping was implemented but **reverted** at user request.

---

### Files changed

| Action | File |
|---|---|
| Created | `chat-panel.service.ts`, `chat-panel.service.spec.ts` |
| Created | `chat-panel.component.ts`, `chat-panel.component.html` |
| Modified | `app.ts`, `app.html` |
| Modified | `sub-header.component.ts`, `sub-header.component.html` |
| Modified | `app.routes.ts` |
| Deleted | `chat-page.component.ts`, `chat-page.component.html` |
| Backend | No changes needed |
