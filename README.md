# System Architecture

This repository documents the architecture of the **XYZ Platform**.

## 📌 Overview
The system is a microservices-based platform that supports authentication, e-commerce, fitness tracking, notifications, analytics, and blockchain integration.

- **Communication**: REST, gRPC, Kafka Event Bus
- **Data Stores**: PostgreSQL, MongoDB, Redis, IPFS
- **Observability**: Prometheus, Grafana, Loki, Jaeger
- **Infrastructure**: Kubernetes, ArgoCD, Service Mesh, Chaos Testing

## 📊 Architecture Diagram
![System Architecture](diagrams/system-architecture.png)

## 📂 Documentation
- [Overview](docs/overview.md)
- [Component Docs](docs/components/)
- [Infrastructure](docs/infrastructure.md)
- [Event Flows](docs/events.md)

## 🔖 ADRs
See [Architecture Decision Records](docs/adr/).
