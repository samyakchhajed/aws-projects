# AWS Projects Portfolio

This directory contains case studies detailing the deliberate selection, refactoring, and evolution of AWS compute models. The projects examine how different workload contracts lead to different compute models across virtual servers, serverless execution, and containerized tasks.

---

## Compute Choice Evolution Matrix

The table below outlines how the workload contracts and architecture evolved across the three projects:

| Aspect | Project 1: EC2 by Necessity | Project 2: Lambda Daily Computation | Project 3: ECS/Fargate Tasks |
| :--- | :--- | :--- | :--- |
| **Compute Choice** | Amazon EC2 (t3.micro) | AWS Lambda (Python 3.11) | Amazon ECS on AWS Fargate |
| **Workload Type** | Persistent stream ingestion | Bounded daily computation | Mixed batch & time-bound streaming |
| **State Model** | Stateful (in-memory rolling state) | Stateless (state externalized to DynamoDB) | Workload-local runtime state; artifacts persisted to S3 |
| **Execution Window** | Long-running (persistent WebSocket) | Bounded (< 30 seconds) | Bounded (exits after unit of work) |
| **Retry Semantics** | Unsafe | Safe (idempotent executions) | No automatic retry or restart |
| **Control Plane** | AWS Systems Manager (SSM) | Amazon EventBridge Scheduler | Native ECS Task Invocation |

---

## Project Summaries

### 1. [Project 1: EC2 by Necessity](./Project-1-EC2-by-necessity/)
A case study on compute selection under strict non-negotiable constraints.
* **Core Problem**: Managing a persistent WebSocket connection that processes in-memory rolling state over execution windows exceeding serverless timeouts.
* **Decision**: Selected EC2 to preserve execution continuity for a long-running workload with non-resettable in-memory state and a persistent WebSocket connection. Lambda did not satisfy the runtime and state constraints, while ECS added orchestration that the single-process workload did not require.
* **Control plane**: Managed via AWS Systems Manager (SSM) Run Command to avoid open SSH access.

### 2. [Project 2: Lambda Daily Computation](./Project-2-Lambda-daily-computation/)
A continuation of Project 1, showing how redesigning the execution contract makes serverless the correct choice.
* **Core Problem**: Refactoring the previous stateful, long-running system into a stateless scheduler-driven model.
* **Decision**: Redesigned the system to externalize execution state to Amazon DynamoDB and output immutable JSON files to Amazon S3. 
* **Result**: After refactoring the workload into short, stateless, and idempotent executions, it could run as scheduled Lambda invocations triggered by EventBridge.

### 3. [Project 3: Orchestrated System (ECS / Fargate)](./Project-3-Orchestrated-System-ECS-Fargate/)
Managing multiple batch and streaming workloads that exceed Lambda limits but do not require persistent EC2 hosts.
* **Core Problem**: Running time-bound WebSocket streaming tasks and data analytics tasks that run to completion and exit.
* **Decision**: Deployed containers as independent Amazon ECS Tasks on AWS Fargate.
* **Architecture**: A single multi-role Docker image configuration-driven via environment variables, writing final data outputs to S3, with execution validated via CloudWatch logs.
