# ADR 0001: Use Kafka for Event Bus

## Status
Accepted

## Context
We need a reliable, scalable event bus for asynchronous communication between services.

## Decision
Use Apache Kafka as the event bus for:
- OrderCreated
- FitnessReward
- TokenMinted
- NotificationQueued
- EmailBatchReady

## Consequences
**Pros:**
- Simplifies async flows

**Cons:**
- Introduces operational overhead (Kafka cluster management)
