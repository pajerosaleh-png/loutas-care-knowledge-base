# RFC-004-Event-Driven-Architecture.md

**Document ID:** RFC-004  
**Document Classification:** Request for Comments  
**Owner:** Architecture Review Board  
**Status:** Draft  
**Version:** 1.0  
**Created:** 2026-07-26  
**Last Updated:** 2026-07-26  
**Next Review:** 2026-08-26  
**Approval Authority:** Chief Software Architect

---

# RFC-004 — Event-Driven Architecture

---

# Status

**Draft**

This RFC proposes introducing an Event-Driven Architecture (EDA) within the **LOUTAS Care Platform** to improve scalability, modularity, reliability, and integration between internal services and future external systems.

---

# Authors

| Name | Role |
|------|------|
| Chief Software Architect | Architecture |

---

# Executive Summary

As the LOUTAS Care platform expands, many business processes require asynchronous communication.

Examples include:

- Appointment notifications
- AI processing
- Billing workflows
- Inventory updates
- Audit logging
- Analytics
- Future third-party integrations

This RFC proposes introducing an event-driven architecture alongside the existing REST API architecture.

REST APIs will remain the primary synchronous communication mechanism, while business events will handle asynchronous workflows.

---

# Background

Current modules include:

- Authentication
- Patient Management
- Reception
- Appointment Management
- EMR
- Billing
- Pharmacy
- Laboratory
- Radiology
- Inventory
- AI
- Reporting

Several workflows already generate events that are consumed by multiple modules.

An event-driven model will reduce coupling and improve extensibility.

---

# Problem Statement

The platform currently relies primarily on synchronous request-response communication.

As the system grows, this approach may result in:

- Tight coupling
- Reduced scalability
- Longer response times
- Duplicate business logic
- Difficult integrations
- Limited extensibility

A standardized event architecture is required.

---

# Goals

The proposed architecture aims to:

- Reduce coupling
- Improve scalability
- Enable asynchronous workflows
- Simplify integrations
- Improve system resilience
- Support future microservices
- Improve maintainability

---

# Non-Goals

This RFC does not propose:

- Replacing REST APIs
- Database redesign
- Full microservices migration
- Event sourcing implementation
- CQRS implementation

These topics may be considered in future RFCs.

---

# Proposed Solution

Introduce an Event Bus responsible for publishing and delivering business events.

Business modules publish events whenever significant domain actions occur.

Interested services subscribe to those events without creating direct dependencies.

---

# Proposed Architecture

```text
Business Module
        │
        ▼
 Publish Event
        │
        ▼
   Event Bus
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
AI     Audit   Notification
Service Service Service
        │
        ▼
 Other Subscribers
```

---

# Event Principles

Business events shall be:

- Immutable
- Timestamped
- Versioned
- Tenant-aware
- Traceable
- Auditable
- Independently consumable

Events shall describe facts that have already occurred.

---

# Example Events

Examples include:

- PatientRegistered
- PatientUpdated
- AppointmentBooked
- AppointmentConfirmed
- AppointmentCancelled
- VisitStarted
- VisitCompleted
- InvoiceCreated
- InvoicePaid
- PaymentRefunded
- InventoryAdjusted
- MedicationDispensed
- LaboratoryOrderCreated
- LaboratoryResultReceived
- RadiologyOrderCreated
- AIClinicalSummaryGenerated

---

# Event Structure

Every event should contain:

- Event ID
- Event Type
- Event Version
- Timestamp
- Tenant ID
- Branch ID
- User ID
- Correlation ID
- Payload

Example:

```json
{
  "eventId": "uuid",
  "eventType": "AppointmentBooked",
  "version": 1,
  "tenantId": "tenant-id",
  "branchId": "branch-id",
  "timestamp": "2026-07-26T10:00:00Z",
  "payload": {}
}
```

---

# Event Consumers

Examples of subscribers include:

- Notification Service
- AI Service
- Billing Service
- Analytics Service
- Audit Service
- Reporting Service
- Integration Service

Subscribers shall process events independently.

---

# Event Delivery

The event platform shall support:

- At-least-once delivery
- Retry policies
- Dead-letter queues
- Failure monitoring
- Event replay (future capability)

Consumers shall implement idempotent processing where applicable.

---

# Security Considerations

Events shall enforce:

- Tenant isolation
- Authentication
- Authorization
- Audit logging
- Encryption in transit
- Encryption at rest

Sensitive healthcare information shall only be included when necessary.

---

# Monitoring

The event platform shall provide:

- Event throughput
- Consumer health
- Queue depth
- Processing latency
- Retry statistics
- Failure metrics
- Delivery success rate

Operational dashboards shall expose platform health.

---

# Alternatives Considered

## Option 1 — REST Only

### Advantages

- Simpler implementation
- Familiar architecture

### Disadvantages

- Tight coupling
- Poor scalability
- Limited asynchronous processing

**Decision:** Rejected.

---

## Option 2 — Hybrid REST + Event Architecture

### Advantages

- Scalable
- Flexible
- Decoupled
- Future-proof
- Supports integrations

### Disadvantages

- Additional infrastructure
- Operational complexity

**Decision:** Approved.

---

## Option 3 — Full Event-Driven Microservices

### Advantages

- Maximum scalability
- Strong service isolation

### Disadvantages

- High implementation complexity
- Increased operational overhead

**Decision:** Deferred for future platform maturity.

---

# Risks

Potential risks include:

- Event duplication
- Lost messages
- Consumer failures
- Ordering issues
- Operational complexity

Mitigation strategies include retries, monitoring, idempotent consumers, and governance.

---

# Performance Considerations

The proposed architecture supports:

- Asynchronous processing
- Horizontal scalability
- Independent service scaling
- Reduced response latency
- Improved resilience

---

# Migration Strategy

Implementation shall occur incrementally:

1. Introduce Event Bus.
2. Publish events from selected modules.
3. Add subscribers gradually.
4. Expand to additional business domains.
5. Introduce external integrations.

---

# Rollout Plan

Phase 1

- Event framework

Phase 2

- Appointment events

Phase 3

- Billing events

Phase 4

- AI events

Phase 5

- Inventory and reporting events

---

# Testing Strategy

Testing shall include:

- Unit Testing
- Integration Testing
- Event Contract Testing
- Performance Testing
- Failure Recovery Testing
- User Acceptance Testing (UAT)

---

# Operational Impact

Operations shall maintain:

- Event monitoring
- Queue management
- Failure alerts
- Consumer health dashboards
- Event retention policies
- Documentation

---

# Open Questions

- Which messaging platform should be adopted?
- What should be the default event retention period?
- Should event replay be supported in the initial implementation?
- Should external partners consume selected events directly?

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Product Owner | Pending | Pending |
| Chief Architect | Pending | Pending |
| Engineering Lead | Pending | Pending |
| Security Lead | Pending | Pending |

---

# Related Documents

- ADR-005-API-Architecture.md
- ADR-009-Deployment-Architecture.md
- ADR-010-Integration-Architecture.md
- RFC-003-AI-Agent-Orchestration.md
- Security Standards
- Architecture Roadmap

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-26 | Initial draft |

---

**End of Document**
