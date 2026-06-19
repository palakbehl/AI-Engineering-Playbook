# Things I Am Learning

This document records the technologies and software design patterns I am actively studying. It tracks my transition from theory to practice.

---

## Active Learning Tracks

### 1. ASP.NET Core & EF Core (Advanced Concepts)
*   **Focus**: EF Core Interceptors and Query Filters.
*   **What I am doing**: I am implementing global query filters in my **RepoLens** database context to handle soft-deletes (`IsDeleted = false`) automatically. I am also writing EF Core interceptors to automatically set audits (e.g., `CreatedDate`, `ModifiedDate`) before rows are written.
*   **Why**: This reduces boilerplate code in my repositories and prevents developers from forgetting audit properties.

### 2. Event-Driven Systems & Redis Streams
*   **Focus**: Asynchronous event streams.
*   **What I am doing**: I am exploring Redis Streams to build a real-time event pipeline. I want to understand consumer groups, message acknowledgments, and event retention limits.
*   **Why**: Redis Streams will allow me to build low-latency event processing services, which is a step toward designing messaging layers (RabbitMQ/Kafka).

### 3. Linux Systems & Bash Scripting
*   **Focus**: Linux file systems, shell commands, and automated scripting.
*   **What I am doing**: I am moving my development servers from Windows-centric local environments to Linux virtual machines. I am writing Bash scripts to automate local Docker container deployments, database backups, and log file sweeps.
*   **Why**: The cloud runs on Linux. Understanding system shells, file permissions, and process management is a requirement for deploying and debugging systems in production.
