# ADR-002: JWT-based Stateless Authentication vs. Sessions

## The Decision
I chose stateless **JSON Web Tokens (JWT)** for authentication and authorization in **CogniTrace** and the **Ajaysinh NGO Platform**, rather than traditional stateful sessions stored in Redis or PostgreSQL.

---

## Context & Trade-offs

In both applications, backend APIs are separated from static frontend clients. In CogniTrace, users continuously stream high-frequency keyboard and mouse telemetry packets.

### Why JWT
*   **Low Latency**: The backend API verifies the user's role and identity directly in memory using the public cryptographic key. It does not need to execute database lookups on every single telemetry packet, keeping response times low.
*   **Horizontal Scalability**: Stateless servers do not save session variables in memory, meaning I can run multiple server instances behind a load balancer without needing a shared session cache (like Redis).

### Why not Stateful Sessions
*   Stateful sessions require the database or Redis to validate session keys on every inbound API request. Under a high stream of telemetry data in CogniTrace, this would cause connection pool starvation and database latency.

---

## What I Would Choose Today
I would still use **JWTs**, but I would enforce a stricter separation of scopes. I would separate public volunteer routes from security auditor dashboards using decoupled token scopes to prevent role collision.

---

## What I Would Choose If Starting Again
If I rebuilt these systems today, I would implement **BFF (Backend-For-Frontend) Architecture**. Instead of storing the JWT directly on the client (React browser), the frontend would communicate with a secure gateway using cookies. The gateway would swap the cookie for the JWT before routing the request to downstream services, protecting the token from browser-based Cross-Site Scripting (XSS) leaks.

---

## What Still Makes Me Uncomfortable
**Token Revocation**: If a user's account is compromised, the backend cannot revoke a stateless JWT until the token naturally expires. To address this risk, I set short token lifespans (15 minutes) and require clients to use secure HttpOnly refresh cookies to get new tokens.
