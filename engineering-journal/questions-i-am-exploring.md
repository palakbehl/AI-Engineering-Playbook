# Questions I Am Exploring

This document compiles the open-ended systems design and architectural questions I am actively exploring. I do not have final answers to these questions; they are the puzzles I study in my sandbox projects.

---

## 1. When is a Modular Monolith too large?

I prefer Modular Monoliths because they avoid network latencies and container complexity. However, as the number of modules grows, I run into questions:
*   At what point does the compilation time of a single codebase slow down developer cycles enough to justify microservices?
*   How do we prevent developers from bypassing boundary checks (e.g., calling another module's classes directly instead of going through interfaces) in a shared codebase?
*   I am currently exploring static analysis tools to block circular imports and boundary violations at compile time.

---

## 2. Zero-Downtime Database Migrations

In early projects, applying database migrations (e.g., adding a table column) involved pausing the server, running the migration, and starting the server. This causes downtime.
*   I am studying the **Expand-and-Contract Pattern** to run migrations without system downtime:
    1.  *Expand*: Add the new column to the database, but keep the old column active.
    2.  *Deploy*: Deploy the updated code that writes to both columns but reads from the old one.
    3.  *Backfill*: Run a background script to copy legacy data to the new column.
    4.  *Update*: Deploy code that reads and writes only from the new column.
    5.  *Contract*: Delete the old column from the database.
*   I want to build an automated pipeline to run this flow safely.

---

## 3. Client-Side Machine Learning vs. Server-Side Safety

In **CogniTrace**, running TensorFlow.js in the browser protected user privacy (mouse coordinates stayed on the client) and reduced server CPU loads.
*   However, client-side execution is vulnerable to tampering. A malicious user can modify the JavaScript code in their browser to bypass model predictions and fake anomaly results.
*   I am exploring how to balance this: Can we run fast client-side predictions for real-time alerts, while executing secondary audits on a subset of coordinate logs on the backend?
