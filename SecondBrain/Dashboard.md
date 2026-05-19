---
date: 2026-05-18
type: dashboard
tags: [dashboard]
ai-first: true
---

# Dev Second Brain

## Active Projects

```dataview
TABLE status, stack, repo
FROM "Projects"
WHERE type = "project" AND status = "active"
SORT date DESC
```

## Recent Dev Sessions

```dataview
TABLE file.mtime AS "Last Modified"
FROM "Dev Logs"
SORT file.mtime DESC
LIMIT 5
```

## Recent Knowledge Captured

```dataview
TABLE file.mtime AS "Saved"
FROM "Knowledge"
SORT file.mtime DESC
LIMIT 8
```

## Recent Decisions

```dataview
TABLE file.mtime AS "Last Updated"
FROM "Projects"
WHERE type = "decisions"
SORT file.mtime DESC
```
