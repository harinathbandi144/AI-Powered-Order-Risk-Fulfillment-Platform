# AI-Powered Order Risk & Fulfillment Platform

An event-driven e-commerce microservices platform built with **Java, Spring Boot, Apache Kafka, Redis, Spring Cloud, and Groq AI**. The platform processes customer orders through inventory, payment, and notification services while using AI-powered risk assessment to approve, hold, or reject potentially fraudulent or anomalous orders.

The project focuses on real-world distributed systems engineering: asynchronous communication, idempotent event processing, service resilience, security, observability, and fault-tolerant order workflows.

> **Project status:** 🚧 In Development

## 🎯 Project Overview

Traditional applications often connect services through synchronous REST calls, creating tightly coupled workflows that can fail when a downstream service becomes unavailable.

This project explores an event-driven alternative. Services communicate through Apache Kafka events, duplicate message delivery is handled using Redis-backed idempotency, and a Groq-powered AI risk scorer evaluates orders before they proceed through the fulfillment workflow.

The platform is designed to demonstrate how production-oriented microservices handle both normal order processing and failure scenarios.

## 🏗️ Architecture

The platform consists of five business microservices, an API Gateway, and a service discovery server.

### Business Microservices

| Service | Responsibility |
|---|---|
| Order Service | Creates orders, manages order states, and coordinates the order workflow through events. |
| Inventory Service | Checks and reserves stock and publishes inventory outcomes. |
| Payment Service | Processes simulated payments and publishes payment outcomes. |
| Notification Service | Consumes relevant events and sends order-status notifications. |
| AI Risk Scorer | Uses Groq AI to evaluate order risk and return a structured decision with an auditable rationale. |

### Infrastructure Components

- **Spring Cloud Gateway:** Central entry point for client requests and routing.
- **Netflix Eureka:** Service registration and discovery.
- **Apache Kafka:** Asynchronous event-driven communication between services.
- **Redis:** Idempotency keys and duplicate-processing protection.
- **JWT Security:** Authentication and authorization across protected APIs.
- **OpenTelemetry + Zipkin:** Distributed tracing across service boundaries.
- **Resilience4j:** Circuit breaker, bulkhead, and resilience controls around AI calls.
- **PostgreSQL:** Persistent storage for service-owned business data.

## 🔄 Order Processing Workflow

The intended workflow is:

1. A customer submits an order through the API Gateway.
2. The Order Service validates the request and creates an order with an initial state.
3. The order workflow requests an AI risk assessment.
4. The AI Risk Scorer evaluates the order and returns a structured decision: `APPROVE`, `HOLD`, or `REJECT`.
5. Approved orders proceed through inventory reservation and payment processing.
6. Services publish business events to Kafka, allowing downstream services to react asynchronously.
7. The Order Service updates the order state based on workflow outcomes.
8. The Notification Service consumes relevant events and sends the appropriate customer notification.
9. Distributed tracing helps follow a single order across the participating services.

The precise event sequence and failure-recovery rules will be implemented and documented as the project evolves.

## ✨ Key Features

### Event-Driven Microservices
- Independent Spring Boot services with clearly defined responsibilities.
- Kafka producers and consumers for asynchronous communication.
- An `order-events` topic for order-related events.
- Reduced reliance on synchronous service-to-service request chains.

### AI-Powered Risk Assessment
- Groq AI integration for structured order risk classification.
- Risk decisions: `APPROVE`, `HOLD`, and `REJECT`.
- Auditable decision records containing the outcome and a concise rationale.
- Explicit handling of AI timeouts, malformed responses, and service unavailability.

The rationale records the factors supporting the decision; it is not intended to store hidden model chain-of-thought.

### Reliable Event Processing
- Redis-backed idempotency keys.
- Protection against duplicate event delivery and repeated processing.
- Defined order states and controlled state transitions.
- Failure handling for inventory, payment, and AI dependencies.

### Security
- JWT-based authentication.
- Protected API endpoints.
- Authorization rules for sensitive operations.
- Secure management of API keys and environment-specific configuration.

### Resilience and Fault Tolerance
- Resilience4j circuit breaker and bulkhead around AI requests.
- Configurable timeouts and failure handling.
- Kafka-based asynchronous processing to avoid unnecessarily blocking the order workflow.
- A chaos-testing scenario that stops the AI Risk Scorer and verifies that orders can remain pending or queued for recovery instead of failing unpredictably.

### Observability
- OpenTelemetry instrumentation.
- Zipkin distributed trace visualization.
- Correlation of requests and events across services.
- Health checks and structured logging for troubleshooting.

## 🛠️ Technology Stack

| Category | Technology |
|---|---|
| Language | Java |
| Framework | Spring Boot |
| Microservices | Spring Cloud |
| API Gateway | Spring Cloud Gateway |
| Service Discovery | Netflix Eureka |
| Messaging | Apache Kafka |
| Idempotency / Cache | Redis |
| Database | PostgreSQL |
| AI Integration | Groq API |
| Security | Spring Security, JWT |
| Resilience | Resilience4j |
| Distributed Tracing | OpenTelemetry, Zipkin |
| Build Tool | Maven |
| Testing | JUnit 5, Mockito, Spring Boot Test, Testcontainers |
| Containerization | Docker, Docker Compose |
| Version Control | Git, GitHub |

## 📁 Planned Repository Structure

```text
ai-order-risk-fulfillment-platform/
├── api-gateway/
├── service-discovery/
├── order-service/
├── inventory-service/
├── payment-service/
├── notification-service/
├── ai-risk-scorer/
├── infrastructure/
│   ├── docker-compose.yml
│   └── kafka/
├── docs/
│   ├── architecture.md
│   └── event-catalog.md
├── .env.example
├── .gitignore
└── README.md
```

Each service will have its own application configuration, business logic, tests, and persistence configuration where required. The final structure may evolve as implementation decisions are made.

## 🧪 Reliability and Testing Scenarios

The project will include tests and demonstrations for scenarios such as:

- A valid order being approved and processed successfully.
- A high-risk order being held or rejected.
- An inventory shortage preventing fulfillment.
- A failed payment moving an order into an appropriate failure state.
- A Kafka event being delivered more than once without repeating its business effect.
- The AI Risk Scorer becoming unavailable during order processing.
- Circuit breaker and bulkhead behavior under AI-service failures.
- An order being traced across multiple services using Zipkin.
- Unauthorized requests being rejected by JWT security.

## 🚀 Development Roadmap

- [ ] Phase 1 — Create the repository, define the architecture, and configure the development environment.
- [ ] Phase 2 — Build the Order Service and establish the core order lifecycle.
- [ ] Phase 3 — Implement Inventory and Payment services.
- [ ] Phase 4 — Introduce Kafka producers, consumers, and event contracts.
- [ ] Phase 5 — Implement the Groq-powered AI Risk Scorer.
- [ ] Phase 6 — Add Redis-backed idempotency and reliable event handling.
- [ ] Phase 7 — Configure the API Gateway, Eureka, and JWT security.
- [ ] Phase 8 — Implement Resilience4j policies and AI-service failure recovery.
- [ ] Phase 9 — Add OpenTelemetry, Zipkin, integration tests, and chaos testing.
- [ ] Phase 10 — Containerize the platform, document setup, and prepare a complete end-to-end demonstration.

## 📚 Engineering Concepts Demonstrated

This project is intended to provide practical experience with:

- Event-driven architecture and asynchronous messaging.
- Microservice boundaries and independent service responsibilities.
- Eventual consistency and distributed workflow coordination.
- Idempotency and duplicate-message handling.
- Failure recovery and resilience patterns.
- AI integration into business decision workflows.
- Authentication, authorization, and secrets management.
- Distributed tracing and production-oriented debugging.
- Integration testing and fault injection.

## ⚙️ Getting Started

The setup instructions will be added as the services and infrastructure become available.

Planned prerequisites:

- Java LTS-compatible JDK.
- Maven.
- Docker and Docker Compose.
- PostgreSQL, Kafka, and Redis.
- A Groq API key for AI risk assessment.

Secrets must be supplied through environment variables or an appropriate secrets manager. Never commit real API keys, passwords, JWT secrets, or production credentials.

## 👨‍💻 Project Goal

Build a production-oriented learning project that demonstrates how to design, implement, test, and troubleshoot an event-driven e-commerce platform with AI-assisted risk decisions.

The primary goal is not simply to connect multiple services. It is to understand how those services behave under duplicate events, downstream failures, asynchronous processing, and partial system outages.

---

**Built with Java, Spring Boot, Kafka, Redis, and Groq AI.**
