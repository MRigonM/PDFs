---
date: 2026-05-19
type: pattern
tags: [pattern, unitofwork, clean-architecture, csharp, dotnet]
ai-first: true
---

## For future Claude
Pattern for splitting IUnitOfWork into a read-only interface (IReadOnlyUnitOfWork) and a full write interface (IUnitOfWork). Validation services inject IReadOnlyUnitOfWork — they cannot accidentally call SaveChanges. IUnitOfWork extends the read-only interface. Both are registered against the same UnitOfWork concrete class in DI. Used in [[Projects/TaskSphere/TaskSphere]] on 2026-05-19. Confidence: stated.

---

# Pattern: IReadOnlyUnitOfWork / IUnitOfWork Interface Split

**Problem:** Services that only read data (e.g., validation services) inject `IUnitOfWork` and gain access to write methods (`SaveChanges`) they should never call. Unnecessary surface area; intent unclear.

**Solution:** Split the interface. Read-only consumers get `IReadOnlyUnitOfWork`, write consumers get `IUnitOfWork`.

## Interface definitions

```csharp
public interface IReadOnlyUnitOfWork
{
    IProjectRepository Projects { get; }
    ITaskRepository Tasks { get; }
    ISprintRepository Sprints { get; }
    IMemberRepository Members { get; }
    ICompanyRepository Companies { get; }
}

public interface IUnitOfWork : IReadOnlyUnitOfWork
{
    IGenericRepository<ChatMessage, int> ChatMessages { get; }

    int SaveChanges();
    Task<int> SaveChangesAsync();
    Task<int> SaveChangesAsync(CancellationToken cancellationToken);
    Task<int> BulkSaveChangesAsync(CancellationToken cancellationToken);
}
```

## Concrete class (lazy-init repos)

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly ApplicationDbContext _context;

    public UnitOfWork(ApplicationDbContext context)
    {
        _context = context;
    }

    private IProjectRepository? _projects;
    public IProjectRepository Projects => _projects ??= new ProjectRepository(_context);

    // ... same pattern for Tasks, Sprints, Members, Companies, ChatMessages ...

    public int SaveChanges() => _context.SaveChanges();
    public async Task<int> SaveChangesAsync() => await _context.SaveChangesAsync();
    public async Task<int> SaveChangesAsync(CancellationToken ct) => await _context.SaveChangesAsync(ct);

    public async Task<int> BulkSaveChangesAsync(CancellationToken ct)
    {
        _context.ChangeTracker.DetectChanges();
        var result = await _context.SaveChangesAsync(ct);
        _context.ChangeTracker.AutoDetectChangesEnabled = true;
        _context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.TrackAll;
        return result;
    }
}
```

## DI registration

```csharp
services.AddScoped<IUnitOfWork, UnitOfWork>();
services.AddScoped<IReadOnlyUnitOfWork, UnitOfWork>();
```

Both map to the same concrete class. DI creates separate scoped instances per request — no shared state issue. Each request gets its own `UnitOfWork` with its own `DbContext`.

## Who injects what

| Service type | Inject |
|---|---|
| Business services (write ops) | `IUnitOfWork` |
| Validation services (read-only) | `IReadOnlyUnitOfWork` |

## Why lazy init (??=)?

Repos are only instantiated when first accessed in a request. If a request only touches Tasks and Projects, Sprint/Member/Company repos are never allocated.

## Alternative considered (rejected)

Option B: inject individual repositories directly into validation services — simpler but reverses the centralization decision. Rejected in favor of interface split.

## Tags
`#pattern` `#clean-architecture` `#unitofwork`