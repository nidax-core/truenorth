# Pitfalls — Integration Layer

This document captures real-world pitfalls encountered or anticipated in the Integration Layer domain.

The purpose is to avoid hidden coupling and fragile workflows.

---

## Coupling pitfalls

### Shared databases

Symptom:
Changing one service schema breaks another service.

Mitigation:
Integrate via APIs/events; keep ownership clear.

### Undocumented dependencies

Symptom:
An integration “just stops working” after an update.

Mitigation:
Document contracts, version interfaces, and test integration paths.

---

## Reliability pitfalls

### No idempotency

Symptom:
Webhook retries create duplicated side effects (double provisioning, double notifications).

Mitigation:
Require event IDs, deduplication and retry-safe handlers.

### Queue as a hidden dependency

Symptom:
The queue becomes the central coupling point and failure domain.

Mitigation:
Use queues only with explicit ownership and contracts; keep workflows small.

---

## Security pitfalls

### Missing webhook validation

Symptom:
Anyone can trigger workflows by calling the endpoint.

Mitigation:
Validate signatures/secrets and apply least-privilege authorization.

### Over-scoped machine identities

Symptom:
A compromised glue service can access everything.

Mitigation:
Minimize scopes/claims and separate workflows.

---

## Operability pitfalls

### No correlation IDs

Symptom:
You cannot trace a workflow across services.

Mitigation:
Require correlation IDs and structured logs end-to-end.

