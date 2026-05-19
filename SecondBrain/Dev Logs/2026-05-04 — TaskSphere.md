---
date: 2026-05-04
type: devlog
tags: [devlog, tasksphere, signalr, chat, angular, dotnet, file-upload, static-files]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
session-duration: ~1 session
---

## For future Claude

This session implemented a full real-time team chat feature for [[Projects/TaskSphere/TaskSphere]] using [[SignalR]] on the backend and `@microsoft/signalr` on the Angular frontend. Chat is scoped per project — only project members can access it. Access control reuses the existing `IAccessControlService.CanAccessProjectAsync()` pattern. A second session added image sharing (paste + file picker), redesigned the chat UI to match the dark theme, and fixed static file serving for uploaded images.

---

## What was worked on

### Feature: Project Team Chat (SignalR real-time)

**Backend (ASP.NET Core / .NET 9):**
- Created `ChatMessage` entity (`ProjectId`, `SenderId`, `Content`) extending `BaseEntity<int>`
- Added `ChatMessages` DbSet + EF Core entity configuration (max 2000 chars, cascade on project delete, restrict on user delete)
- Created `IChatService` / `ChatService` — sends messages (with access check), returns paged history with sender names
- Created `ChatHub` (SignalR) — `JoinProject`, `LeaveProject`, `SendMessage` methods; authenticates via JWT claims
- Created `ChatController` — REST endpoint `GET /api/Chat/projects/{projectId}/messages` for history
- Configured JWT `OnMessageReceived` event to pass `access_token` from query string (required for WebSocket auth)
- Registered `AddSignalR()` + `MapHub<ChatHub>("/hubs/chat")` in `Program.cs`
- EF migration `AddChatMessages` created

**Frontend (Angular 21):**
- Installed `@microsoft/signalr` package
- Created `ChatService` — manages hub connection, signals for messages/connected state, loads history via REST
- Created `ChatPageComponent` at route `/chat/:projectId` — real-time chat UI with Tailwind styling
- Added "Chat" link to sub-header nav (visible when a project is selected)
- Added access validation — on page load, checks membership via API; redirects to `/dashboard/projects` on 403

---

## Problems encountered

1. **`Error.Message` vs `Error.Description`** — The domain `Error` record uses `Description`, not `Message`. Caught at build time.
2. **JWT claim casing** — Token stores `"companyId"` (lowercase c), initially wrote `"CompanyId"` in hub. Fixed by checking `AccountService` claim generation.
3. **No `Name` claim in JWT** — Solved by having `ChatService` load sender name from `UserManager` after persisting the message, rather than reading it from token claims.

---

## Decisions made

1. **SignalR over REST polling** — Real-time UX is worth the added complexity; JWT auth already works with SignalR's query string token pattern.
2. **One hub, groups per project** — `project-{id}` groups. Simple, scales to many projects without multiple hub classes.
3. **Access check on component init, not a route guard** — Avoids creating a new guard + extra API endpoint. The messages endpoint already returns 403, so we reuse it.
4. **Messages ordered newest-first in DB, reversed on client** — Allows efficient paged loading while displaying oldest-first in UI.
5. **2000 char message limit** — Reasonable for team chat, enforced at DB level.

---

## Session 2: Chat UI polish + Image sharing

### What was worked on

**UI/UX redesign:**
- Redesigned chat page to match the app's dark theme (`bg-white/5`, `border-white/10`, `text-white`, indigo accents)
- Own messages: indigo bubbles. Others' messages: `bg-white/10` with `text-slate-200`
- Input field and buttons match project page styling (rounded-xl, same color palette)
- Height constrained to viewport minus nav/sub-header

**Image sharing feature:**
- Added `ImageUrl` optional property to `ChatMessage` entity
- Created `POST /api/Chat/upload` endpoint — accepts jpeg/png/gif/webp, max 5MB, saves to `wwwroot/uploads/chat/` with GUID filename
- Frontend: paste handler (`onPaste`) detects clipboard images, uploads automatically, sends as message
- Frontend: attach button (image icon) for file picker selection
- Images render inline in chat bubbles (max-h-64, clickable to open full-size in new tab)
- "Uploading..." state disables input during upload
- Migration `AddChatMessageImageUrl` created

**Security review:**
- Ran full security audit of chat implementation — no high-confidence vulnerabilities found
- Cross-tenant isolation confirmed via `companyId` check in `CanAccessProjectAsync`
- Angular template rendering auto-escapes content (no XSS risk)

### Problems encountered

4. **Static files 404** — `UseStaticFiles()` didn't serve uploaded images because `wwwroot` was created at runtime (by the upload endpoint) after the app had already booted. The `WebRootFileProvider` was null. Fixed by explicitly creating a `PhysicalFileProvider` pointing at the `wwwroot` directory with `Directory.CreateDirectory()` at startup.
5. **Image URL resolution** — Upload returns relative path `/uploads/chat/...` which the Angular dev server can't serve. Fixed by prepending the backend base URL (`https://localhost:5001`) via `resolveImageUrl()` helper in the component.

### Decisions made

6. **File upload to local disk** — Simplest approach for dev. Images saved as `{GUID}.{ext}` to avoid path traversal and filename collisions. Production would use Azure Blob/S3.
7. **5MB file size limit** — Reasonable for chat images, enforced at controller level via `[RequestSizeLimit]`.
8. **Whitelist image MIME types** — Only allow jpeg/png/gif/webp. Prevents arbitrary file upload.

---

## Next steps

- Apply migrations: `AddChatMessages` + `AddChatMessageImageUrl`
- Auto-scroll to bottom when new messages arrive
- Consider typing indicators or read receipts (future enhancement)
- Consider message notifications (toast when message arrives while on another page)
- For production: move image storage to cloud (Azure Blob Storage)