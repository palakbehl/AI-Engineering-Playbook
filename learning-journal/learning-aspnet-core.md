# Learning ASP.NET Core: My Journal

This journal documents my experience learning **ASP.NET Core** and **C#** while building the backend for **RepoLens**.

---

## Why I'm Learning ASP.NET Core

I spent my early developer years building APIs in Node.js and Express. While Node is fast for prototyping, JavaScript's lack of strict types led to runtime mapping errors. I chose to learn ASP.NET Core because I wanted to master an enterprise-grade, strongly typed backend framework that handles high concurrent workloads and enforces clean architectural boundaries.

---

## What Surprised Me

*   **It is incredibly fast**: Coming from Node.js, I assumed .NET would feel heavy and slow. I was wrong. The Compiled Intermediate Language (IL) execution and the Kestrel web server are highly optimized, running loops and thread pool tasks faster than the V8 JavaScript engine.
*   **Dependency Injection is native**: In Node, you have to write manual setup files or import heavy libraries to implement DI. In ASP.NET Core, it is a core framework feature.
*   **Convulsive Boilerplate**: The amount of setup code required to build a basic API (interfaces, models, controllers, configurations, DbContext mappings) was overwhelming compared to Express.js.

---

## Common Mistakes I Keep Making

*   **Middleware Registration Mistakes**: I regularly register middlewares in the wrong order in `Program.cs`. I have spent hours debugging why authorization claims were null, only to find I called `app.UseAuthorization()` before `app.UseAuthentication()`.
*   **EF Core Captive Dependencies**: I have accidentally injected a scoped dependency (like `DbContext`) directly into a singleton background worker. This kept the database connection open indefinitely, eventually causing connection pool starvation.
*   **Async Await Omissions**: When writing asynchronous repository calls, I occasionally forget the `await` keyword. Because the code compiles without warnings, it leads to silent thread race conditions.

---

## How I Compare It with Node.js

*   **TypeScript is compile-time only; C# is runtime-safe**: In Node, TypeScript types are erased after compilation. If an external API returns malformed JSON, the server can crash. C# type safety is enforced at runtime, allowing me to catch schema mismatches during serialization.
*   **Single-Thread vs. Multi-Thread**: Node's single-threaded event loop is easy to write, but CPU-intensive tasks (like AST code parsing in RepoLens) block the loop. ASP.NET Core's multi-threaded thread pool handles concurrent CPU tasks efficiently.
*   **Development Velocity**: Node.js/Express is faster to bootstrap. I can write a working API in Express in 10 minutes. In ASP.NET Core, setting up the configuration, migrations, and layers takes longer, but the resulting codebase is easier to maintain as it grows.

---

## Things I Still Don't Understand

*   **IL Code optimizations**: I don't yet understand how the Just-In-Time (JIT) compiler optimizes IL code into assembly under different runtime environments.
*   **Advanced Garbage Collection Configurations**: I am still learning how to configure garbage collector heuristics (Server GC vs. Workstation GC) to optimize memory allocations for background task workers.

---

## What I Would Build Next

To apply my C# skills, I want to build a **Distributed Rate Limiting Gateway**. It will use ASP.NET Core middleware, Redis token-bucket arrays, and custom headers to rate-limit incoming API calls across multiple backend microservices, testing my understanding of multi-threaded thread synchronization and low-latency network calls.
