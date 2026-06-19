# Learning System Design: My Journal

This journal documents my approach to learning system design, moving beyond theoretical structures to practical implementation.

---

## My Learning Strategy: Build First, Read Second

System design is easy to read about but difficult to execute. You can memorize definitions of load balancers, database sharding, and caching, but until you run into a system bottleneck, those concepts remain abstract.

*   *My Practice*: I do not study system design in isolation. I design architectures for the systems I build (like the telemetry queues in CogniTrace or the task queues in SmartFood).
*   *My Goal*: I want to build enough software that I naturally run into performance bottlenecks. Encountering a connection pool limit or a thread lock teaches me more than reading a chapter in a textbook.

---

## Architectural Principles That Clicked

### 1. The Database is Always the Bottleneck
No matter how fast your web server is, the database is constrained by disk input/output, locks, and network latency.
*   *My Practice*: When designing a system, I check the database query paths first. I look for ways to reduce query count (e.g., caching static results, batching writes, or using composite indexes) before considering scaling the server nodes.

### 2. Caching introduces Consistency Problems
Adding a cache (like Redis) makes reads faster, but it creates a sync issue. If data changes in the database, when does the cache update?
*   *My Practice*: I avoid caching data that changes frequently. I use caching only for:
    *   Highly requested, static values (e.g., user profiles or campaign configurations).
    *   Slow, pre-aggregated metrics where minor consistency delays are acceptable (e.g., repository technical debt scores in RepoLens).

### 3. Decouple Write Streams Asynchronously
If an operation does not need to return a result immediately, it should not run in the HTTP request path.
*   *My Practice*: I offload CPU-heavy operations (e.g., generating forecast results in SmartFood or compiling codebase files in RepoLens) to background queues, keeping the web API responsive for users.

---

## System Design Concepts I Am Still Mastering

*   **Distributed Consensus (Raft/Paxos)**: I understand the theory, but I have not yet built a system that coordinates state across distributed nodes using consensus protocols.
*   **Partitioning / Sharding Mechanics**: While I understand database indexing and replication, I need to gain experience configuring horizontal database sharding in high-scale environments.
*   **Observability at Scale**: Configuring distributed tracing across microservices to monitor system behavior and trace request paths is an area I am active in learning.
