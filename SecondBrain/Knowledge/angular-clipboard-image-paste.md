---
date: 2026-05-17
type: pattern
tags: [pattern, angular, clipboard, file-upload, image, chat]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
---

## For future Claude

Angular pattern for detecting pasted images from clipboard in a chat/editor input and uploading them automatically. Used in [[Projects/TaskSphere/TaskSphere]] chat feature. Handles both clipboard paste (`Ctrl+V`) and file picker selection. The key API is `ClipboardEvent.clipboardData.items`, checking `item.type.startsWith('image/')`.

---

## Pattern: Clipboard Image Paste + File Picker Upload (Angular)

### Use case
User pastes a screenshot or image into a chat input → auto-upload → send as message with image URL.

### Component (TypeScript)

```typescript
uploading = false;

onPaste(event: ClipboardEvent): void {
  const items = event.clipboardData?.items;
  if (!items) return;

  for (let i = 0; i < items.length; i++) {
    if (items[i].type.startsWith('image/')) {
      event.preventDefault();        // stop image appearing as text
      const file = items[i].getAsFile();
      if (file) this.uploadAndSend(file);
      return;
    }
  }
}

onFileSelected(event: Event): void {
  const input = event.target as HTMLInputElement;
  const file = input.files?.[0];
  if (file) this.uploadAndSend(file);
  input.value = '';                  // reset so same file can be re-selected
}

private uploadAndSend(file: File): void {
  this.uploading = true;
  this.service.uploadImage(file).subscribe({
    next: (res) => {
      this.service.sendMessage(this.input.trim(), res.url);
      this.input = '';
      this.uploading = false;
    },
    error: () => { this.uploading = false; },
  });
}
```

### Template (HTML)

```html
<textarea (paste)="onPaste($event)" [disabled]="uploading"></textarea>

<input #fileInput type="file" accept="image/*" class="hidden" (change)="onFileSelected($event)">
<button (click)="fileInput.click()" [disabled]="uploading">📎</button>
```

### Key details
- `event.preventDefault()` on paste stops the browser from inserting raw image data as text
- Reset `input.value = ''` after file selection so the same file can be picked again (change event won't fire twice otherwise)
- Disable input + button during upload to prevent double-sends
- `uploading` flag doubles as UI state (show "Uploading..." placeholder)

### URL resolution
Backend returns relative path (e.g., `/uploads/chat/abc.png`). Angular dev server can't proxy this — prepend backend base URL:

```typescript
resolveImageUrl(url: string): string {
  if (url.startsWith('http')) return url;
  return environment.apiUrl.replace('/api/', '') + url;
}
```

### Related
- [[Knowledge/aspnetcore-secure-file-upload]] — backend upload endpoint pattern
- [[Knowledge/signalr-group-per-entity]] — the SignalR chat this was built for
- [[Projects/TaskSphere/TaskSphere]] — first use of this pattern
