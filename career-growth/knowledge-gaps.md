# My Knowledge Gaps

This document is a collection of the technologies and architectural patterns where I do not yet feel confident.

My goal is not to memorize terms. My goal is to build enough systems that I naturally encounter the performance and coordination problems these tools were built to solve.

---

## Current Areas of Ignorance

### 1. Distributed Systems Coordination
*   **The Gap**: I understand the theory of Paxos and Raft consensus protocols, but I have never configured a system where multiple database nodes must agree on system state. 
*   **How I Plan to Bridge It**: I want to build a small distributed key-value store in Go or C# from scratch. Writing the consensus and node-to-node replication logic manually will help me understand distributed state coordination.

### 2. Enterprise Message Queues (RabbitMQ / Kafka)
*   **The Gap**: I have built basic in-memory job queues (using BullMQ and Redis) in my project **SmartFood**. However, I have not configured enterprise brokers like RabbitMQ or Kafka, which handle message partition routing, dead-letter exchanges, and clustering.
*   **How I Plan to Bridge It**: I will refactor SmartFood's Python/Node.js sidecar connection to use RabbitMQ instead of Redis, testing how the system handles worker crashes and message retries.

### 3. Advanced PostgreSQL Tuning
*   **The Gap**: I can write indexes and read `EXPLAIN` query plans. However, I do not understand how to tune PostgreSQL server parameters (like `shared_buffers`, `work_mem`, or WAL write limits) to match specific server hardware configurations.
*   **How I Plan to Bridge It**: I plan to run load testing scripts (using tools like `pgbench`) on local PostgreSQL containers under different memory and pool limit settings to measure the performance impact.

### 4. Production Observability at Scale
*   **The Gap**: I have used basic logging frameworks (Serilog, Winston). I do not have experience setting up distributed tracing tools (Jaeger, OpenTelemetry) or monitoring dashboards (Grafana, Prometheus) across multiple microservices.
*   **How I Plan to Bridge It**: I will configure OpenTelemetry and Grafana on my Docker Compose files to trace requests as they travel between services.
