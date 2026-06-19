# Engineering Rules I Actually Follow

This is the collection of heuristics and rules I use to guide my daily coding decisions. They are not textbook requirements, but guidelines I have adopted to keep my codebases clean, readable, and performant.

---

## My Coding Heuristics

### 1. The One-Sentence Component Rule
> **If I cannot explain a component's responsibility in a single, simple sentence without using the word "and," the component is doing too much.**
*   *Application*: When writing services in **RepoLens**, if I find myself describing a class as "it parses the abstract syntax tree *and* updates the PostgreSQL database," I immediately split it. I extract an `IAstParser` service and a `IRepositoryDebtWriter` service.

### 2. The One-Screen Controller Rule
> **If a web API controller method exceeds the vertical height of my IDE screen (about 30-40 lines), I have leaked business logic into the transport layer.**
*   *Application*: In my **Ajaysinh NGO Platform** Express backend, routes should only extract parameters, validate request formats, call a service handler, and return an HTTP status. The moment I see an array mapping loop or database logic inside an Express route handler, I extract it to a service layer.

### 3. The Database Query Paranoia Rule
> **Every database query eventually becomes a performance problem.**
*   *Application*: I write code assuming database tables will contain millions of records. When writing LINQ queries in ASP.NET Core or Mongoose queries in MongoDB, I never allow queries without limits (`Take()`, `Limit()`), nor do I write nested loops that trigger queries (the N+1 trap).

### 4. The Shortcut Contamination Rule
> **Every shortcut is technical debt with compounding interest.**
*   *Application*: Committing code with hardcoded configurations, skipping write transactions for dual-document updates, or writing a quick "hack" to bypass CORS are choices I always regret. Doing it right takes 10 minutes longer; fixing it later takes days.

### 5. The "No Magic strings/Numbers" Rule
> **If a value has meaning beyond its literal representation, it must be represented by a constant or an enum.**
*   *Application*: In **SmartFood**, instead of checking `if (status === 2)`, I use an enum: `if (status === InventoryStatus.Spillage)`. This simple step eliminates reading hurdles and logic bugs.

---

## OOP Rules I Actually Follow

I do not write academic OOP code. I follow only the rules that keep my applications extensible:

### 1. Program to Interfaces for Boundaries, Not Internal Implementations
I only write interfaces (`interface`) when:
*   I need to mock a component for testing (e.g., mocking an external API client like GitHub or Gemini).
*   I have polymorphic behavior (e.g., multiple payment gateways like Stripe and Razorpay).
*   *Rule of thumb*: I do not write `IService` and `Service` implementations for internal, stateless helpers that will never have alternative implementations. That is boilerplate clutter.

### 2. Composition Over Inheritance (Always)
I almost never use class inheritance (`extends` in TS, `:` in C#) for business logic. Deep class trees are brittle; changing a base class breaks downstream classes in unexpected ways.
*   *My Practice*: I build small, focused classes and inject them as collaborators. The only inheritance I accept is framework-required (such as inheriting from `DbContext` in EF Core or `BackgroundService` in C#).

### 3. Immutability by Default
Mutable state is the root cause of multi-threaded race conditions and frontend state rendering bugs.
*   *My Practice*: When writing C#, I define record types using `init` properties or read-only properties:
    ```csharp
    public record FileAnalysisResult(string Path, double ComplexityScore);
    ```
    In React, I treat state as read-only, using state updates exclusively rather than mutating objects in-place.
