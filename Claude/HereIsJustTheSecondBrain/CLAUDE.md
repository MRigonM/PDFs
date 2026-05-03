# CLAUDE.md — Rigon's Dev Second Brain

## Vault Purpose
Coding knowledge base only. No personal notes.
Captures decisions, solutions, patterns, and session context
across all coding projects.

## Vault Location
D:\.rigonsecondbrain\RigonSecondBrain

## Folder Structure
- Inbox/                    → raw dumps: errors, snippets, questions
- Projects/StayEase/        → architecture, decisions, bug log
- Projects/TaskSphere/      → architecture, decisions, bug log
- Knowledge/                → reusable patterns learned across projects
- Daily/                    → session logs (what I worked on, what I learned)

## Session Protocol

### On start:
1. Read the latest note in Daily/ if it exists
2. Read the relevant Projects/<name>/decisions.md
3. Check Inbox/ for anything unprocessed

### On end:
1. Log what was worked on to Daily/YYYY-MM-DD.md
2. Move any decisions to Projects/<name>/decisions.md
3. Move any reusable patterns to Knowledge/

## Note Rules
- [[wikilink]] related notes always
- Tags: #bug #solved #decision #pattern #question
- For decisions: always record WHY, not just what
- No bloated notes. One idea per note when possible.

## What NOT to Save
- Anything personal
- Trivial tasks already solved and forgotten
- Notes you'll never query again