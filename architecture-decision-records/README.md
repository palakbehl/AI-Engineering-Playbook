# My Architecture Decision Records (ADRs)

This directory contains records of the major architectural decisions I made while building my applications. 

Rather than documenting decisions as historical artifacts, each record represents a living choice. They focus on why decisions were made, how they were implemented, and the trade-offs I accepted.

---

## My ADR Structure

To ensure these records remain practical, I have discarded formal, academic structures. Each record contains:

*   **The Decision**: The design choice that was made.
*   **The Context & Trade-offs**: Why the choice was made, and what alternative solutions were evaluated.
*   **What I Would Choose Today**: My current perspective on this decision based on what I have learned since.
*   **What I Would Choose If Starting Again**: What I would do differently if I were to rebuild the system from scratch today.
*   **What Still Makes Me Uncomfortable**: The potential points of failure, scaling issues, or structural risks in this decision.

---

## Index of Decisions

1.  **[ADR-001: PostgreSQL over MongoDB for Analytics](./adr-001-repolens-postgresql-vs-mongodb.md)**
    *   *System*: RepoLens
    *   *Focus*: Relational data complexity and analytical aggregation requirements.
2.  **[ADR-002: JWT-based Stateless Authentication vs. Sessions](./adr-002-cognitrace-jwt-vs-sessions.md)**
    *   *System*: CogniTrace / NGO Platform
    *   *Focus*: Stateless scalability, authentication, and JWT security traps.
3.  **[ADR-003: C# / ASP.NET Core vs. Node.js for Backend Architecture](./adr-003-repolens-csharp-vs-nodejs.md)**
    *   *System*: RepoLens
    *   *Focus*: Structured type safety vs. JavaScript dynamic prototyping.
4.  **[ADR-004: Docker Adoption Framework](./adr-004-docker-adoption.md)**
    *   *System*: Multi-Tier Applications
    *   *Focus*: Environment parity, dependency drift, and containerization trade-offs.
5.  **[ADR-005: React SPA vs. Server-Side Rendering](./adr-005-react-vite-spa-vs-ssr.md)**
    *   *System*: Security dashboards & Portals
    *   *Focus*: Client-side interaction speeds vs. search engine optimization (SEO).
