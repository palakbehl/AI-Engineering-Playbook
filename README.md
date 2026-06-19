# Palak Behl | Engineering Operating System

Welcome to my second brain. 

This repository documents how I personally think, learn, build, debug, review, design, and maintain software. It is not an educational reference or a compilation of textbook definitions. Instead, it is a highly opinionated reflection of my decision-making processes, heuristics, preferences, and the hard lessons I have learned while building real-world applications.

If I joined your engineering team tomorrow, this repository serves as the blueprint for how I would approach your systems, code, and trade-offs.

---

## What This Repository Is Not

To set clear expectations, **this repository is NOT**:
*   **Software engineering notes**: You will not find explanations of what OOP, Docker, or PostgreSQL are.
*   **Interview preparation material**: This is not a repository of memorized definitions or LeetCode patterns.
*   **AI prompt collections**: I do not document prompt templates or generate AI hype.
*   **Technology tutorials**: I am not teaching how to spin up a web server or build an API from scratch.

This repository exists solely to codify my growth, documenting how my thinking evolves as I design and build software.

---

## About Me

I am a B.Tech Computer Engineering student, a Full Stack Developer, an **Amazon Future Engineer Scholar**, and a multiple **Odoo Hackathon Finalist**. I construct full-stack web applications, background processing services, security telemetry pipelines, and database designs.

My engineering perspectives have been forged through the development of four primary projects:
1.  **RepoLens**: A C# and ASP.NET Core system that analyzes repository intelligence and scans codebases for technical debt using PostgreSQL and the Gemini API.
2.  **CogniTrace**: A high-security React and Node.js security dashboard analyzing user mouse and keyboard telemetry using client-side machine learning (TensorFlow.js) and Role-Based Access Control (RBAC).
3.  **SmartFood**: A MERN-stack inventory optimization application that leverages Python machine learning models (Facebook Prophet & Random Forest) to forecast food demands.
4.  **Ajaysinh Foundation NGO Platform**: A production-grade web portal running on React, Node.js, Express, and MongoDB, integrating Razorpay to handle fundraising and donation security safely.

---

## Operating System Structure & Navigation

### 1. [Engineering Philosophy](./engineering-philosophy/)
*   **[My Rules](./engineering-philosophy/my-rules.md)**: The strict coding guidelines I actually follow, from maximum controller sizes to database query paranoia.
*   **[Mistakes I Keep Making](./engineering-philosophy/mistakes-i-keep-making.md)**: The anti-patterns I find myself repeating (and how I actively guard against them).
*   **[System Design Thinking](./engineering-philosophy/system-design-thinking.md)**: How I model systems, map domain boundaries, and evaluate technical trade-offs.

### 2. [AI Collaboration](./ai-collaboration/my-ai-framework.md)
*   My rules for AI-generated code, mistakes AI has helped me catch vs. the logic bugs it introduced, and my exact validation and testing pipelines.

### 3. [Debugging Mechanics](./debugging-mechanics/my-debugging-flow.md)
*   My mental model for tracing runtime issues across the stack: React render loop leaks, ASP.NET Core dependency injection lifetimes, database connection starvation, and authentication failures.

### 4. [Operating Frameworks](./operating-frameworks/decision-lists.md)
*   Actionable questions I ask myself *before* creating a database table, adding an external dependency, or writing an API contract.

### 5. [Architecture Decision Records (ADRs)](./architecture-decision-records/)
*   Decisions from my projects detailing what I chose, why, what I would do differently today, and what still makes me uncomfortable:
    *   [ADR-001: PostgreSQL vs. MongoDB for Analytics](./architecture-decision-records/adr-001-repolens-postgresql-vs-mongodb.md)
    *   [ADR-002: JWT vs. Stateful Sessions](./architecture-decision-records/adr-002-cognitrace-jwt-vs-sessions.md)
    *   [ADR-003: C# / ASP.NET Core vs. Node.js for Backend Architecture](./architecture-decision-records/adr-003-repolens-csharp-vs-nodejs.md)
    *   [ADR-004: Docker Adoption Framework](./architecture-decision-records/adr-004-docker-adoption.md)
    *   [ADR-005: React SPA vs. Server-Side Rendering](./architecture-decision-records/adr-005-react-vite-spa-vs-ssr.md)

### 6. [Project Retrospectives](./project-retrospectives/)
*   Founder-style letters describing what I built, initial vs. wrong assumptions, and design regrets for [RepoLens](./project-retrospectives/repolens.md), [CogniTrace](./project-retrospectives/cognitrace.md), [SmartFood](./project-retrospectives/smartfood.md), and [Ajaysinh NGO Platform](./project-retrospectives/ajaysinh-foundation.md).
*   **[How Projects Changed My Thinking](./project-retrospectives/how-projects-changed-my-thinking.md)**: A summary of my growth as an engineer across these applications.

### 7. [Technical Debt](./technical-debt/my-technical-debt-framework.md)
*   How I identify code smells, calculate debt metrics using Git churn, and refactor code without introducing production regressions.

### 8. [Code Review](./code-review/how-i-review-code.md)
*   How I read and audit pull requests, focusing on security loops, query execution paths, and clean boundaries.

### 9. [Learning Journal](./learning-journal/)
*   Personal logs documenting how I learn technologies:
    *   [Learning ASP.NET Core](./learning-journal/learning-aspnet-core.md)
    *   [Learning PostgreSQL](./learning-journal/learning-postgresql.md)
    *   [Learning Docker](./learning-journal/learning-docker.md)
    *   [Learning System Design](./learning-journal/learning-system-design.md)

### 10. [Engineering Journal](./engineering-journal/)
*   **[Things That Clicked](./engineering-journal/things-that-clicked.md)**: Critical engineering insights that finally made sense after months of study.
*   **[Questions I Am Exploring](./engineering-journal/questions-i-am-exploring.md)**: Open-ended architecture and systems design questions I am currently analyzing.
*   **[Architecture Observations](./engineering-journal/architecture-observations.md)**: My notes on industry systems and design patterns.

### 11. [Career Growth](./career-growth/)
*   My roadmap, things I am learning, and **[Knowledge Gaps](./career-growth/knowledge-gaps.md)**—the areas of distributed computing where I still need to build systems to gain confidence.
