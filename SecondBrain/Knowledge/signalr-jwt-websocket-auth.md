---
date: 2026-05-04
type: pattern
tags: [pattern, signalr, jwt, websocket, dotnet, authentication]
ai-first: true
confidence: stated
source-project: "[[Projects/TaskSphere/TaskSphere]]"
---

## For future Claude

SignalR WebSocket connections cannot send HTTP headers after the initial handshake, so JWT tokens must be passed via query string. This pattern shows how to configure ASP.NET Core JWT bearer to read the token from `?access_token=` on hub routes, and how the Angular client passes it via `accessTokenFactory`.

---

## Pattern: SignalR JWT Authentication via Query String

### Problem

WebSocket connections don't support custom headers after the upgrade handshake. Standard `Authorization: Bearer <token>` header doesn't work for SignalR's WebSocket transport.

### Solution

**Backend (`JwtBearerEvents.OnMessageReceived`):**

```csharp
options.Events = new JwtBearerEvents
{
    OnMessageReceived = context =>
    {
        var accessToken = context.Request.Query["access_token"];
        var path = context.HttpContext.Request.Path;
        if (!string.IsNullOrEmpty(accessToken) && path.StartsWithSegments("/hubs"))
        {
            context.Token = accessToken;
        }
        return Task.CompletedTask;
    }
};
```

**Frontend (Angular / `@microsoft/signalr`):**

```typescript
new HubConnectionBuilder()
  .withUrl(hubUrl, { accessTokenFactory: () => token })
  .withAutomaticReconnect()
  .build();
```

### Key details

- The path check (`StartsWithSegments("/hubs")`) ensures you only read query tokens for hub routes, not regular API calls
- `accessTokenFactory` is a function (not a static value) so it can return a fresh token on reconnect
- CORS must have `.AllowCredentials()` enabled for the SignalR origin

### Used in

- [[Projects/TaskSphere/TaskSphere]] — `ChatHub` at `/hubs/chat` (as of 2026-05-04)