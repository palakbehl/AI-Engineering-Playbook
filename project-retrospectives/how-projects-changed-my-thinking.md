# How My Projects Changed My Thinking

This document is a reflection of my growth as an engineer. Looking back at the evolution of my code across my projects, I can trace how my thinking shifted from "writing code that works" to "designing systems that can be maintained and operated."

---

## The Growth Timeline

```mermaid
chronology
    title Project Influence on My Engineering Mental Models
    2024 : SmartFood : Asynchronous execution, Python ML integrations, data quality constraints.
    2024 : Ajaysinh Foundation : Payment gateway integrity, transactional safety, server webhooks.
    2025 : CogniTrace : Performance boundaries, telemetry, client-side ML isolation via Web Workers.
    2025 : RepoLens : Clean Architecture, strict static typing C#, database optimization and CTEs.
```

---

## Project Lessons Summary

### 1. SmartFood (Asynchronous Processing & Data Realities)
*   *Before*: I thought machine learning was about training accurate models. I assumed integrating ML into web apps was just a matter of running scripts.
*   *After*: SmartFood taught me that **the integration is the hard part**. I learned that running heavy computations synchronously blocks the main runtime loop. It forced me to learn about Redis queues, task workers, and how to build fallback moving averages when historical data is sparse.

### 2. Ajaysinh NGO Platform (Reliability & Transaction Safety)
*   *Before*: I assumed networks were stable and database writes would succeed if my code compiled cleanly.
*   *After*: This project taught me that **networks are unreliable and users close browser tabs midway**. I learned that payment processing requires cryptographic verification via server-to-server webhooks, database transactions to avoid inconsistent data states, and idempotent operations to prevent double-crediting campaign totals.

### 3. CogniTrace (Performance Isolation & Telemetry)
*   *Before*: I thought CPU limits were only a concern on the server. I assumed modern web browsers could handle execution loops without rendering delays.
*   *After*: CogniTrace taught me that **client-side performance boundaries are strict**. Offloading telemetry analysis to Web Workers taught me how to keep the main render thread free. It also taught me database retention frameworks—I learned that unstructured logs grow fast and databases need capped collections and TTL indexes.

### 4. RepoLens (Clean Architecture & Strict Type Safety)
*   *Before*: I favored JavaScript/Node.js for its speed and lack of boilerplate. I found compile-time type-safety rules tedious.
*   *After*: RepoLens taught me the value of **strict C# type safety and Clean Architecture**. Building a codebase analysis engine forced me to write testable domains decoupled from databases. I gained a deep appreciation for C# Task Parallel structures and PostgreSQL window queries for analytics.

---

## My Shift in Focus

When I look at my early code compared to my recent architectures, my focus has pivoted:

| Early Projects (SmartFood) | Current Projects (RepoLens) |
| :--- | :--- |
| Writing code to solve problems quickly. | Writing clean, decoupled code that can be tested. |
| Choosing NoSQL (MongoDB) for convenience. | Choosing PostgreSQL for structural relational safety. |
| Running tasks synchronously in the API path. | Offloading CPU tasks to background queues. |
| Trusting external libraries without review. | Auditing dependencies for size, safety, and updates. |
