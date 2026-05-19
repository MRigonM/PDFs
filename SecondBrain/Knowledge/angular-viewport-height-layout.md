---
type: knowledge
date: 2026-05-17
tags: [angular, css, tailwind, layout, viewport, flex]
ai-first: true
---

## For future Claude

This pattern locks an Angular app shell to exactly the viewport height so that inner regions can scroll independently without the page itself growing. The failure mode is using `min-h-screen` on the root, which allows the page to overflow and breaks `overflow-hidden` on flex children because they never get a real height boundary.

## Problem

- `min-h-screen` lets the page grow beyond 100vh; flex children with `overflow-hidden` don't clip correctly because their parent has no fixed upper bound.
- Side panels using `flex-1 overflow-y-auto` end up with unconstrained height and don't scroll — they just expand.

## Solution

Use `h-screen overflow-hidden` on the root wrapper so it is **exactly** viewport height and cannot grow. Then build downward with flex containers that each carry `overflow-hidden` until you reach the scrollable leaf.

## Class Reference

| Element | Classes |
|---|---|
| Root wrapper | `h-screen flex flex-col overflow-hidden` |
| Flex row wrapper (main + panel) | `flex flex-1 overflow-hidden` |
| Main content area | `flex-1 min-w-0 overflow-auto` |
| Side panel outer div | `h-full flex flex-col overflow-hidden` |
| Panel scrollable area | `flex-1 overflow-y-auto` |

### Why `min-w-0` on main?

Flex children default to `min-width: auto`, which means they won't shrink below their content width. `min-w-0` overrides this so the main column actually yields space to the side panel.

## Used In

- [[Projects/TaskSphere/TaskSphere]] chat side panel