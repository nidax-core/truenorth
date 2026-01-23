# Scope — Security, Observability & Compliance

This document defines what is included in the Security, Observability & Compliance PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- Host and application monitoring
- Log aggregation and correlation
- File integrity and intrusion detection
- Alerting and response
- Audit trails and reporting

### Typical components (context-dependent)

- Wazuh
- OpenSearch / Elastic-compatible stack
- Network visibility tooling (e.g., Zeek)
- External alerting (email, chat, webhook)

---

## Out of scope

- Identity lifecycle ownership (Domain 1 remains authoritative)
- Hosting substrate design (Domain 3) beyond emitting telemetry
- Full data governance and legal retention policy (beyond PoC-level defaults)
- “Compliance by documentation” without provenance

---

## Interfaces to other domains

### Domain 1 — Identity & Access

- Consume identity-related events (authentication, authorization decisions, admin actions).
- Provide audit trails that support offboarding and access reviews.

### Domain 2 — Collaboration & Productivity

- Consume service logs (mail, groupware, file sharing, chat) as evidence.
- Provide alerts and audit output relevant to user-facing services.

### Domain 3 — Compute & Hosting

- Consume host-level telemetry (system logs, resource health, integrity signals).
- Provide a feedback loop for hardening and operational decisions.

---

## Design rule

No security without logs. No alerts without context.

This PoC treats observability as a dependency for security and compliance not as an optional add-on.

