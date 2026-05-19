---
type: knowledge
date: 2026-05-17
tags: [angular, signals, dom, scroll, chat]
ai-first: true
---

## For future Claude

This pattern solves the timing mismatch between Angular's signal reactivity and DOM updates when auto-scrolling a chat container. The key insight is that Angular's `effect()` runs before the DOM reflects the new content, so `scrollHeight` is stale at that point. Always defer the scroll with `setTimeout(..., 0)` and add a separate reopen effect when `@if` blocks recreate the DOM.

## Problem

Calling `el.scrollTop = el.scrollHeight` inside an Angular `effect()` fires **before** the DOM updates. At the time the effect runs, the new message hasn't been painted yet, so `scrollHeight` is stale and the container doesn't scroll to the actual bottom.

## Solution

Wrap the scroll in `setTimeout(..., 0)` to defer until after the current event loop tick — by which point Angular has already flushed its DOM writes.

```ts
effect(() => {
  this.chatService.messages(); // track signal
  setTimeout(() => {
    const el = this.messagesContainer?.nativeElement;
    if (el) el.scrollTop = el.scrollHeight;
  }, 0);
});
```

## Reopen Effect

When a panel is toggled with `@if`, Angular **destroys and recreates the DOM node**. The messages-tracking effect above won't fire on reopen because the signal value hasn't changed. Add a second effect keyed on the open state:

```ts
effect(() => {
  if (this.panelService.isOpen()) {
    setTimeout(() => this.scrollToBottom(), 0);
  }
});
```

## What NOT to Do

- Do **not** use `ngAfterViewChecked` — it fires on every change detection cycle, causing unnecessary noise and potential performance issues.

## Used In

- [[Projects/TaskSphere/TaskSphere]] chat panel