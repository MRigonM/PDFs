---
date: 2026-05-19
type: project
tags: [project, tasksphere, angular, dotnet, clean-architecture]
ai-first: true
status: active
stack: [".NET 9", "Angular 21", "SQL Server", "EF Core", "SignalR"]
repo: "D:\\projects\\personal\\TaskSphere"
---

## For future Claude

[[Projects/TaskSphere/TaskSphere]] is a multi-tenant project management SPA (Angular + ASP.NET Core Clean Architecture). Company is the tenant boundary. Two roles: Company (admin) and User (member). Access control is project-membership-based. Key patterns: Result<T>, soft deletes, generic repository + UoW, FluentValidation, AutoMapper, JWT auth.

---

## Overview

Task management platform with sprints, backlog, and team chat. Multi-tenant via Company entity. Users are scoped to projects via Member entity.

## Recent Activity

- [[Dev Logs/2026-05-19 — TaskSphere|2026-05-19]] — Refactored UnitOfWork: repos as lazy-init UoW properties, IReadOnlyUnitOfWork interface split for validation services, DI cleaned up
- [[Dev Logs/2026-05-18 — TaskSphere|2026-05-18]] — Implemented chat side panel: `ChatPanelService` + `ChatPanelComponent`, app shell `h-screen` flex row layout, `ChatPageComponent` + `/chat/:projectId` route deleted, panel height/auto-scroll/expand (600px) post-fixes
- [[Dev Logs/2026-05-17 — TaskSphere|2026-05-17]] — Designed chat side panel (brainstorm + plan); implementation plan ready
- [[Dev Logs/2026-05-04 — TaskSphere|2026-05-04]] — Implemented real-time team chat with SignalR, image sharing, dark theme UI

## Features

- **Chat side panel** — persistent side panel driven by `ChatPanelService` signals; toggled from `SubHeaderComponent`; 380px normal / 600px expanded; auto-scrolls on new message and reopen. Full-page `/chat/:projectId` route has been removed.