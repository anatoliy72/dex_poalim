# Design Patterns for AML (Anti-Money Laundering) Microservice Architecture

## 1. **Microservice Patterns**

### API Gateway Pattern
- **Description**: An API Gateway serves as a single entry point for all client requests, routing them to the appropriate microservices. It handles cross-cutting concerns like authentication, authorization, rate limiting, and request transformations.
- **Why**: In an AML system, sensitive operations like user data retrieval and transaction monitoring require centralized security management. The API Gateway simplifies this and reduces complexity at the microservice level.

### Circuit Breaker Pattern
- **Description**: The Circuit Breaker pattern monitors calls to external services and prevents further attempts when a service is deemed unavailable. When a failure threshold is reached, the circuit opens, blocking additional requests until the system recovers.
- **Why**: AML systems interact with databases, third-party services, and external systems. If one service fails, the Circuit Breaker prevents cascading failures, improving system resilience and stability.

## 2. **Data Patterns**

### Database per Service Pattern
- **Description**: This pattern ensures each microservice manages its own database independently. This avoids cross-service data access and enforces clear service boundaries.
- **Why**: In an AML system, different services (e.g., user data, transaction logs) need isolated databases to prevent tight coupling, ensuring changes in one service don't negatively impact others. This leads to better fault tolerance and scalability.

### CQRS (Command Query Responsibility Segregation) Pattern
- **Description**: CQRS splits the responsibility of reading and writing data. Commands modify the data, while queries fetch the data. Often, separate data stores are used for queries and commands to optimize performance and scalability.
- **Why**: AML systems often need to process high volumes of transactions and run complex queries. By separating reads and writes, CQRS allows better performance tuning for both operations, leading to higher scalability.

## 3. **Observability Patterns**

### Log Aggregation Pattern
- **Description**: This pattern collects logs from all microservices in a single location for centralized analysis. Tools like Fluentd, Logstash, or Loki forward logs to a central storage (e.g., Elasticsearch) for easier searching and reporting.
- **Why**: In an AML system, it is essential to analyze user behavior, detect fraud, and audit system actions. Log aggregation helps centralize logs from all services, making it easier to trace issues, audit, and detect patterns in real-time.

### Health Check API Pattern
- **Description**: This pattern involves each microservice exposing a health-check endpoint that returns the current status (e.g., up or down) of the service. External monitoring tools like Prometheus can check these endpoints regularly.
- **Why**: AML systems must operate continuously with minimal downtime. Health checks provide early detection of service issues, allowing for proactive mitigation before critical failures occur.

### Distributed Tracing Pattern
- **Description**: Distributed tracing tools allow you to track a request as it flows through different microservices. This is critical for identifying bottlenecks or failures in a microservice architecture.
- **Why**: AML workflows can span multiple services, so tracing requests is essential for monitoring performance and detecting issues. Distributed tracing enables better visibility and helps in quickly diagnosing where problems occur.

## 4. **Intervention Patterns**

### Saga Pattern
- **Description**: The Saga pattern is used to manage distributed transactions across multiple microservices. Each service in the transaction completes a step, and if one step fails, the Saga coordinates compensating transactions to roll back changes.
- **Why**: AML processes like fraud detection require multiple microservices to work together. If a step in the transaction fails, the Saga pattern ensures that previous steps are rolled back, maintaining consistency and reliability across the system.
