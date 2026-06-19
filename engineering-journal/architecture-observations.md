# Architecture Observations

This document records my observations of production systems and industry software design patterns. I study how major companies (like Stripe, Vercel, and Linear) build systems to learn from their architectural choices.

---

## 1. The Real Cost of Serverless

Serverless (AWS Lambda, Azure Functions) is marketed as the ultimate cost-saving scaling tool because you "only pay for what you use."

### My Observation
Serverless is cheap for low-traffic apps or highly spike-prone workloads, but it becomes expensive under constant traffic.
*   **Cold Starts**: Serverless functions require a startup delay (cold start) when scale changes. This is unacceptable for interactive REST APIs where page latency must be low.
*   **Calculated Cost**: For constant workloads (such as the repository sync workers in **RepoLens**), running a small container on a dedicated Virtual Private Server (VPS) is cheaper and provides predictable performance compared to paying per-execution fees in a serverless environment.
*   *My Practice*: I use dedicated containers for core processes, reserving serverless tasks for occasional, isolated computations (like generating PDFs or sending webhook alerts).

---

## 2. API Gateway Decoupling (The Stripe Model)

Stripe's APIs are known for their reliability. They decoupled the **API Ingress Gateway** from downstream business microservices.
*   **The Pattern**: The gateway receives the request, validates the authorization signature, registers a copy of the request payload to an audit database, and immediately places the request into an internal message queue (RabbitMQ/Kafka). The client receives a `202 Accepted` status. Downstream workers process the message from the queue.
*   **Why this works**: If a downstream service crashes, the gateway continues to receive payments and volunteers without losing requests. Once the service recovers, the workers process the accumulated queue messages.
*   *My Application*: I used this pattern to design the telemetry ingestion system in **CogniTrace** and the forecast queue in **SmartFood**.

---

## 3. Server-Side Rendering (SSR) Hype

The frontend industry has shifted heavily toward SSR frameworks (Next.js, Remix).

### My Observation
While SSR is optimal for search engine optimization (SEO) and e-commerce index pages, it adds unnecessary hosting complexity to secure, dashboard-centric apps.
*   Dashboards require client-side interaction speed. After the initial bundle download, client-side React SPAs run transitions instantly.
*   Using Next.js for secure, private dashboards increases server resources and introduces state hydration issues between client and server.
*   *My Practice*: Keep landing pages static and build the dashboard interface as a decoupled Vite-powered React SPA.
