# Things That Clicked

This document records the concepts that finally clicked for me after months of struggle and confusion. These are the moments when a theoretical concept became an active tool in my development workflow.

---

## 1. Why Dependency Injection Actually Exists

For a long time, I associated Dependency Injection (DI) with framework boilerplate. I wrote C# constructors that accepted interfaces because the ASP.NET Core documentation told me to, but I did not understand *why* I was doing it.

### The Realization
**The goal of DI is not dependency injection; the goal is to reduce coupling between classes.** 

If Class A instantiates Class B directly (`var b = new ClassB()`), Class A is locked to that specific implementation. If Class B changes, or if I need to mock it during testing, I have to rewrite Class A.

By injecting an interface (`ClassA(IClassB b)`), Class A does not care how Class B works. It only cares that Class B implements the interface contract. The DI container (like the built-in ASP.NET Core container) is simply the implementation detail that handles instantiating these dependencies.

```csharp
// Before: Hardcoded dependency (High Coupling)
public class RepoScanner {
    private readonly GitHubClient _client = new GitHubClient(); // Tight coupling
}

// After: Injected interface (Low Coupling, High Testability)
public class RepoScanner {
    private readonly IGitHubClient _client;
    public RepoScanner(IGitHubClient client) { // Loose coupling
        _client = client;
    }
}
```

---

## 2. Separating Reads and Writes (CQRS)

I used to use the same database model for writing records and rendering dashboard tables. 

### The Realization
**The data models required to update a database are fundamentally different from the models required to display data to users.**

*   **Writes (Commands)**: Need transactional integrity, validation constraints, and normalized relations to prevent data corruption.
*   **Reads (Queries)**: Need flat, pre-aggregated, and denormalized data models optimized for fast UI rendering.

By separating my read query paths from my write command paths, I can write simpler code. In RepoLens, instead of querying complex joined database tables to render repo summaries, I project data into simple read-only DTOs using `.AsNoTracking()`, keeping queries fast.

---

## 3. Git is a Directed Acyclic Graph (DAG)

I used to think of Git as a chronological folder of file backups.

### The Realization
**Git is a Graph Database.** 
A commit is a node. It contains a pointer to its content state, metadata, and pointers to one or more parent commits. Branches are pointers to specific nodes in this graph. Understanding this graph model allowed me to write code history analysis algorithms for **RepoLens**, as I could traverse nodes recursively to find branch offsets and count commit histories.
