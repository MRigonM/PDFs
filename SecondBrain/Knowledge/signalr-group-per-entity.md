---
date: 2026-05-04
type: pattern
tags: [pattern, signalr, real-time, architecture, dotnet]
ai-first: true
confidence: stated
source-project: "[[Projects/TaskSphere/TaskSphere]]"
---

## For future Claude

Pattern for scoping SignalR real-time messaging to entity-level groups (e.g., one group per project, per room, per conversation). Users join/leave groups dynamically. Combined with membership-based access control, this enforces that only authorized users receive broadcasts.

---

## Pattern: SignalR Groups Per Entity

### Structure

```
Hub method: JoinProject(int projectId)
  → Validate membership (IAccessControlService)
  → Groups.AddToGroupAsync(connectionId, $"project-{projectId}")

Hub method: SendMessage(dto)
  → Validate membership
  → Persist message
  → Clients.Group($"project-{dto.ProjectId}").SendAsync("ReceiveMessage", message)
```

### Key decisions

1. **Group name = entity type + ID** — `project-{id}`, `room-{id}`, etc. Simple, predictable.
2. **Validate on join, not on every send** — If membership can't change mid-session, validate once at join. If it can change (user removed from project), validate on send too.
3. **Persist before broadcast** — Always save to DB first, then broadcast. If broadcast fails, message is still stored and will appear on next history load.
4. **History via REST, real-time via hub** — Don't load history through SignalR. Use a REST endpoint with paging for initial load, hub only for live messages.

### Used in

- [[Projects/TaskSphere/TaskSphere]] — Team chat per project (as of 2026-05-04)