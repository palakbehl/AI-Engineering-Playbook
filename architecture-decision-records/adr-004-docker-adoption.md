# ADR-004: Docker Adoption Framework

## The Decision
I containerized local development and production database services using **Docker** and **Docker Compose**.

---

## Context & Trade-offs

During early development across my projects, I ran into environmental inconsistencies: Node versions differed between my machine and production, local PostgreSQL installations had differing configuration files, and Python ML dependencies (for SmartFood) clashed with local development runtimes.

### Why Docker
*   **Environment Parity**: Packaging databases (PostgreSQL, MongoDB, Redis) and backend services inside containers guarantees they run identically on Windows, macOS, and Linux servers.
*   **Onboarding & Tear Down**: Setting up a development environment with multiple databases and servers is reduced to a single command: `docker compose up -d`.
*   **Dependency Isolation**: Python ML libraries (Facebook Prophet, Pandas) run in their own container context, avoiding collisions with other Python versions installed on the host OS.

### The Trade-off
*   Running Docker (especially inside WSL2 on Windows) consumes significant system memory (RAM). I chose to accept this resource overhead to eliminate the time spent debugging environment-specific database and library errors.

---

## What I Would Choose Today
I would still use **Docker**. It is a mandatory tool for maintaining clean development environments.

---

## What I Would Choose If Starting Again
If starting again, I would use **Docker Multi-stage Builds** earlier in my configurations. In early iterations, my runtime images contained compiler dependencies, testing libraries, and development tools, resulting in image sizes of over 1.2 GB. Using multi-stage builds reduces image sizes to under 150 MB by copying only compiled files to the final runtime image.

---

## What Still Makes Me Uncomfortable
**Local Volume Permissions**: Sharing files between host operating systems and Linux containers (bind mounts) can cause permission issues and file sync delays (especially on Windows). In addition, if developers forget to map database directories to Docker volumes, destroying a container can cause data loss.
