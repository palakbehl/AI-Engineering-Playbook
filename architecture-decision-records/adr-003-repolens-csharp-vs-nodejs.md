# ADR-003: C# / ASP.NET Core vs. Node.js for Backend Architecture

## The Decision
I chose **ASP.NET Core (C#)** for the **RepoLens** backend database and analysis services, rejecting Node.js/Express.

---

## Context & Trade-offs

The RepoLens backend parses and analyzes code syntax structures, computes complexity metrics, and aggregates metrics. This work requires parallel tasks and strict data models when mapping raw Git commits and file trees.

### Why ASP.NET Core
*   **Compile-Time & Runtime Type Safety**: C# ensures that my domain entities, database records, and API data models match exactly. This eliminates runtime mapping crashes common in Node.js when processing raw external data.
*   **Parallel Processing**: Analyzing code trees can block CPU threads. C# supports multi-threading through the Task Parallel Library (`Task.Run`), enabling concurrent file analysis without blocking the main web server thread.
*   **Entity Framework Core**: EF Core's database migration systems and LINQ integration allow me to write type-safe queries that are verified at compile time.

### Why not Node.js / Express.js
*   Node.js runs on a single thread. CPU-intensive operations (such as calculating AST structures of files) can block the event loop, causing API requests to hang. Additionally, TypeScript type safety is lost after compilation, leaving the server vulnerable to runtime data schema mismatches.

---

## What I Would Choose Today
I would still choose **ASP.NET Core**. The structural discipline it enforces makes it the optimal framework for long-term project maintenance and complex data models.

---

## What I Would Choose If Starting Again
If starting again, I would use **MediatR** to implement the mediator pattern in my controllers. This would decouple my API routes from the service implementations, turning controllers into simple event dispatchers and keeping business logic organized in separate command/query handlers (CQRS).

---

## What Still Makes Me Uncomfortable
**Boilerplate Complexity**: ASP.NET Core requires a large amount of setup code compared to Node.js. Defining DbContext models, interfaces, repository implementations, and mapping layers increases initial development overhead, slowing down early prototyping.
