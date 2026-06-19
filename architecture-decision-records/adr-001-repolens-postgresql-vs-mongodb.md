# ADR-001: PostgreSQL over MongoDB for RepoLens Analytics

## The Decision
I chose **PostgreSQL** as the primary storage and analytical database engine for **RepoLens**, rejecting MongoDB.

---

## Context & Trade-offs

RepoLens acts as a repository auditor. To compute technical debt metrics, the database needs to store structured relational data (Repositories -> Branches -> Commits -> Authors -> Code Changes). The system must run queries (e.g., calculating code churn trends, determining code complexity by directory tree, and correlating contributor activity with repository debt indices).

### Why PostgreSQL
*   **Complex Analytical Joins**: Calculating directory-level technical debt requires hierarchical recursive queries. PostgreSQL handles this using Common Table Expressions (CTEs). Performing the same query in MongoDB requires complex, memory-intensive aggregation pipelines (`$lookup` and `$unwind`) that are difficult to write and optimize.
*   **Referential Integrity**: Commits, branches, and repositories are tightly coupled. PostgreSQL guarantees that child records (e.g., commit details) are automatically updated or deleted when their parent entities change.
*   **Hybrid JSONB Storage**: I can store unstructured raw API data from the GitHub API directly into PostgreSQL `JSONB` columns, while maintaining structured relational integrity in other columns.

### Why not MongoDB
*   To avoid complex queries, MongoDB requires duplicating data (denormalization) across documents. This is problematic for Git histories because a single commit might be linked to multiple branches or pull requests. Duplicating commit data across document collections creates data consistency challenges during repository sync updates.

---

## What I Would Choose Today
I would still choose **PostgreSQL**. Its support for relational modeling and SQL analysis is superior for analytics dashboards.

---

## What I Would Choose If Starting Again
If starting again, I would use **TimescaleDB** (a PostgreSQL extension for time-series analytics) alongside the base database. This would allow me to query and visualize repository health over time (such as commit velocity and debt spikes) using specialized time-bucket queries.

---

## What Still Makes Me Uncomfortable
**Write Lock Contentions**: During initial repository imports, the sync worker writes thousands of commits, branches, and code changes in bulk. If database write locks are not managed carefully, it can block read requests from the web API, leading to slow dashboard load times. To mitigate this, I have to batch writes and manage PostgreSQL connection limits.
