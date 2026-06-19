# Learning PostgreSQL: My Journal

This journal documents my experience modeling, querying, and optimizing relational databases using **PostgreSQL** while building **RepoLens**.

---

## How I Think About Relational Data

I used to view relational databases as simple spreadsheet stores. After building applications with complex data relations, I realized that relational modeling is about **guaranteeing the structural integrity of your system state**. If a commit cannot exist without a branch, and a branch cannot exist without a repository, database constraints are the rules that prevent data corruption.

---

## Why RepoLens Uses PostgreSQL

When analyzing codebase histories:
*   I need to calculate hierarchical metrics (e.g., aggregating technical debt scores across nested folder trees).
*   I need to analyze churn trends (e.g., rolling commit averages and code additions over time).
*   I need referential constraints to prevent orphan commit records.

PostgreSQL excels at these analytical requirements through **Window Functions** and **Common Table Expressions (CTEs)**.

---

## Queries That Changed How I Think

### 1. Hierarchical Folder Traversal using CTEs
To calculate technical debt scores for nested directories (e.g., finding the debt score of `/src/controllers` by scanning all files inside it), I had to learn how to write **Recursive Common Table Expressions**:

```sql
WITH RECURSIVE directory_tree AS (
    -- Anchor member: Select root files
    SELECT id, path, parent_directory_id, debt_score
    FROM files
    WHERE parent_directory_id = @RootDirectoryId
    
    UNION ALL
    
    -- Recursive member: Join child directories/files
    SELECT f.id, f.path, f.parent_directory_id, f.debt_score
    FROM files f
    INNER JOIN directory_tree dt ON f.parent_directory_id = dt.id
)
SELECT SUM(debt_score) FROM directory_tree;
```
Writing this query opened my eyes to SQL's power—it can traverse complex trees in a single database execution block, avoiding multiple round-trip API calls.

### 2. Rolling Metrics using Window Functions
To calculate a 7-day rolling average of commit additions per repository, I learned to write window functions:

```sql
SELECT 
    commit_date,
    additions,
    AVG(additions) OVER (
        ORDER BY commit_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) as rolling_additions
FROM commit_history;
```
This taught me how to run analytics calculations in the database engine instead of loading thousands of rows into server memory.

---

## Indexing & Performance Lessons

*   **Foreign Keys do not create indexes**: I assumed that configuring a foreign key constraint automatically indexed the column. After observing slow join queries, I learned that PostgreSQL requires you to create indexes on foreign keys manually.
*   **The Leftmost Prefix Rule**: I wrote a composite index on `(repository_id, author_id)`. I then ran queries filtering only by `author_id` and observed they were slow. I learned that composite indexes are order-dependent—queries must filter by the first column in the index key for the database planner to use it.

---

## Mistakes I Made

*   **Exhausting Connections**: I did not configure connection pooling in early deployments. Every time an API requested repository metrics, it opened a new PostgreSQL connection process. Under load, this led to connection starvation, crashing both the database and the API. (I resolved this by implementing connection limits and setting up PgBouncer).
*   **Sequential Scans on Large Tables**: I ran queries filtering by unindexed search strings, triggering table scans on millions of rows. This pinned the database server CPU at 100%. I now prefix queries with `EXPLAIN` to verify the execution path.
