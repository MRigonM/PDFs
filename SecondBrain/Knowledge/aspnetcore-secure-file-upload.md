---
date: 2026-05-17
type: pattern
tags: [pattern, dotnet, aspnetcore, file-upload, security, static-files]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
---

## For future Claude

ASP.NET Core pattern for a secure file upload endpoint with MIME type whitelisting, GUID-based rename, size limit, and correct static file serving for runtime-created upload directories. Used in [[Projects/TaskSphere/TaskSphere]] chat image upload. The static file serving fix is non-obvious: if `wwwroot/` doesn't exist at app startup, `UseStaticFiles()` sets `WebRootFileProvider` to null — you must explicitly register a `PhysicalFileProvider` at startup.

---

## Pattern: Secure File Upload Endpoint (ASP.NET Core)

### Controller endpoint

```csharp
[HttpPost("upload")]
[RequestSizeLimit(5 * 1024 * 1024)]          // 5 MB hard limit
public async Task<IActionResult> Upload(IFormFile file)
{
    if (file is null || file.Length == 0)
        return BadRequest("No file provided.");

    var allowedTypes = new[] { "image/jpeg", "image/png", "image/gif", "image/webp" };
    if (!allowedTypes.Contains(file.ContentType.ToLowerInvariant()))
        return BadRequest("Only image files (jpeg, png, gif, webp) are allowed.");

    var uploadsDir = Path.Combine(_env.ContentRootPath, "wwwroot", "uploads", "chat");
    Directory.CreateDirectory(uploadsDir);     // idempotent — safe to call every request

    var fileName = $"{Guid.NewGuid()}{Path.GetExtension(file.FileName)}";
    var filePath = Path.Combine(uploadsDir, fileName);

    await using var stream = new FileStream(filePath, FileMode.Create);
    await file.CopyToAsync(stream);

    return Ok(new { url = $"/uploads/chat/{fileName}" });
}
```

### Security properties

| Concern | Mitigation |
|---|---|
| Path traversal | GUID rename — original filename never used in path |
| Arbitrary file type | MIME type whitelist (jpeg/png/gif/webp only) |
| Oversized upload | `[RequestSizeLimit(5MB)]` — enforced before controller runs |
| Filename collision | `Guid.NewGuid()` prefix guarantees uniqueness |

### Static file serving fix for runtime-created wwwroot

**Problem:** If `wwwroot/` doesn't exist when the app boots, `UseStaticFiles()` sets `WebRootFileProvider = NullFileProvider`. Files uploaded after boot are never served — 404 on every image.

**Fix in `Program.cs` (before `app.UseStaticFiles()`):**
```csharp
var wwwroot = Path.Combine(builder.Environment.ContentRootPath, "wwwroot");
Directory.CreateDirectory(wwwroot);
builder.Environment.WebRootPath = wwwroot;
builder.Environment.WebRootFileProvider = new PhysicalFileProvider(wwwroot);

app.UseStaticFiles();
```

### Production note
Local disk storage is dev-only. For production, swap upload logic for Azure Blob Storage or S3 and return the CDN URL. The controller interface stays the same.

### Related
- [[Knowledge/angular-clipboard-image-paste]] — Angular side of this upload pattern
- [[Projects/TaskSphere/TaskSphere]] — first use
