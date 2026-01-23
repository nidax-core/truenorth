# Decisions — Integration Layer

This document captures decisions made during the Integration Layer PoC.

Decisions are recorded to keep the PoC auditable and to make coupling explicit.

---

## Decision 1 — Prefer explicit contracts over shared state

Context:
Shared databases and implicit behaviors create hidden coupling.

Decision:
Integrate via explicit APIs/events with documented schemas.

Consequences:

- Requires contract discipline and versioning
- Enables replaceability and clearer ownership

---

## Decision 2 — Small glue services over a monolithic integration platform

Context:
Integration layers tend to become “the place where everything lives”.

Decision:
Use small, single-responsibility glue services that own a narrow workflow.

Consequences:

- More services, but each stays understandable
- Reduced blast radius and simpler changes

---

## Decision 3 — Require idempotency for event/webhook handling

Context:
Retries happen. Without idempotency, retries cause duplicate side effects.

Decision:
Webhook/event handlers must be dedupe- and retry-safe.

Consequences:

- Requires event IDs and state tracking where needed
- Improves reliability under failure

---

## Decision 4 — Observability is part of the contract

Context:
Cross-domain workflows without traceability are not operable.

Decision:
Require correlation IDs and structured logs for all integration workflows.

Consequences:

- Slightly more implementation work
- Faster debugging and better auditability

