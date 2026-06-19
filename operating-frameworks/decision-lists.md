# My Operating Frameworks & Decision Lists

This document details the questions and criteria I use to evaluate code changes before they are integrated into a system. These are not general checklists, but frameworks to prevent regressions, bloat, and technical debt.

---

## 1. Before Creating a New Database Table

When I design a new table, I ask myself:

*   **Will this entity still make sense in six months?** 
    *   *Why*: I want to avoid creating tables that duplicate existing records or represent temporary states. For example, in **SmartFood**, instead of creating a `TemporaryStockOut` table, I used a `StockTransaction` ledger with state flags.
*   **What indexes will I need?**
    *   *Why*: I identify which fields will be used in `WHERE`, `JOIN`, or `ORDER BY` clauses. I create indexes for these fields on day one.
*   **What query will be slow?**
    *   *Why*: I analyze how the table will scale to millions of rows. If I need to join this table with multiple others to render a basic dashboard, I consider storing pre-calculated values (denormalization) to avoid slow queries.
*   **Does this entity require historical audit tracking?**
    *   *Why*: If changes to rows must be tracked (e.g., changes to donations on the NGO Platform), I create a companion audit trail table (`DonationLogs`) or use soft-delete fields instead of modifying the core record directly.

---

## 2. Before Adding a Dependency

When I consider importing an external package (npm package, NuGet library, Python package), I ask:

*   **Can I build this myself in 20 lines of code?**
    *   *Why*: I want to avoid importing heavy libraries for simple utility functions (e.g., pulling in `lodash` for simple array operations, or `left-pad`).
*   **Is the maintenance worth it?**
    *   *Why*: Every dependency is a package I am delegating security and maintenance for. I check the project's GitHub page: Is it maintained? How many open issues are there? When was the last commit?
*   **What happens if this package is abandoned?**
    *   *Why*: If a package is abandoned, I am stuck with its security issues and dependency locks. If I must use a third-party library, I wrap it behind an interface. If the package is abandoned, I can write a replacement wrapper without modifying the rest of my application.
*   **Does this bloat the client bundle?**
    *   *Why*: On the frontend (React), importing large libraries (like moment.js) increases initial load times. I look for lightweight alternatives (like date-fns) or use native browser APIs.

---

## 3. Before Writing a New API Endpoint

When I design a new endpoint, I ask:

*   **What is the change driver for this API?**
    *   *Why*: I ensure the API maps to a single business concern. If an endpoint does multiple things (e.g., updates a user profile *and* triggers an order email), I split it.
*   **Is this endpoint idempotent?**
    *   *Why*: If a network glitch occurs, the client might retry the request. I design write endpoints so that duplicate requests do not create duplicate records. For payments (e.g., Razorpay on the NGO Platform), I verify a unique transactional signature on the backend to guarantee idempotency.
*   **How do I prevent resource abuse?**
    *   *Why*: I establish maximum payload sizes for `POST` requests and enforce rate-limiting on public routes to protect backend services from denial of service.
*   **How will this endpoint be authenticated and authorized?**
    *   *Why*: I specify which roles (`Admin`, `User`, `Volunteer`) have access, and ensure authorization is verified locally using token claims.
