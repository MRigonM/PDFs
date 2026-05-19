---
date: 2026-05-19
type: devlog
tags: [devlog, tasksphere, unitofwork, repository-pattern, clean-architecture]
ai-first: true
project: "[[Projects/TaskSphere/TaskSphere]]"
---

## For future Claude
Session on 2026-05-19: refactored the repository/UnitOfWork pattern — repos moved from separate DI registrations into UnitOfWork as lazy-initialized properties. Added IReadOnlyUnitOfWork interface so validation services cannot call SaveChanges. Build succeeded clean with 0 new errors.

---

## Session: 2026-05-19 — UnitOfWork Refactor

### What was worked on

Refactored the entire repository access pattern. Previously services injected individual repositories (IProjectRepository, ITaskRepository, etc.) directly from DI. Now all repos are accessed exclusively through `IUnitOfWork`.

#### IUnitOfWork changes
- Added 6 repo properties: `Projects`, `Tasks`, `Sprints`, `Members`, `Companies`, `ChatMessages`
- Added `SaveChanges()`, `SaveChangesAsync()` (no CT overload), kept `SaveChangesAsync(CancellationToken)`, added `BulkSaveChangesAsync(CancellationToken)`
- Now extends new `IReadOnlyUnitOfWork` interface

#### IReadOnlyUnitOfWork (new interface)
- 5 read-only repo properties: `Projects`, `Tasks`, `Sprints`, `Members`, `Companies`
- No save methods — enforces read-only access boundary at the type level
- `IUnitOfWork` extends it, adding `ChatMessages` + all save overloads

#### UnitOfWork class changes
- Lazy-init all 6 repos via `??=` null-coalescing assignment
- All repos share same `ApplicationDbContext` instance (correct for EF change tracking)
- No try/catch blocks — stack trace preserved on exceptions

#### Services updated
- `ProjectService`, `TaskService`, `SprintService`, `CompanyService`, `ChatService` — now inject `IUnitOfWork`
- `TaskValidationService`, `SprintValidationService` — inject `IReadOnlyUnitOfWork` (read-only boundary enforced)

#### DI registrations
- Removed 6 individual `AddScoped<IXRepository, XRepository>()` calls
- Kept `services.AddScoped<IUnitOfWork, UnitOfWork>()`
- Added `services.AddScoped<IReadOnlyUnitOfWork, UnitOfWork>()`

### Decisions made
See [[Projects/TaskSphere/decisions]]

### Build result
Build succeeded — 8 pre-existing warnings (null refs in AccountService, Member.cs, BaseEntity.cs), 0 new errors introduced by this change.

### Next steps
No outstanding items from this session.
