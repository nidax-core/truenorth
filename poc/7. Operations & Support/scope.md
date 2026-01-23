# Scope — Operations & Support

This document defines what is included in the Operations & Support PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- Onboarding and migration
- Documentation and runbooks
- Incident handling
- User and admin support

---

## Out of scope

- Building monitoring/SIEM tooling (Domain 4 owns the platform; this domain defines how it is used)
- Designing the hosting substrate (Domain 3)
- Implementing the automation toolchain (Domain 5)
- Organizational HR/legal processes (beyond operational offboarding steps)

---

## Interfaces to other domains

### Domain 1 — Identity & Access

- Joiner/mover/leaver operational steps must be documented and repeatable.
- Break-glass access must be defined and auditable.

### Domain 2 — Collaboration & Productivity

- User-facing support is anchored in the workspace domain (clients, access, common issues).
- Migration and onboarding often start with workspace tooling.

### Domain 3 — Compute & Hosting

- Day-2 procedures include host/service lifecycle operations (restarts, failover, recovery).
- Backup/restore runbooks must be executable.

### Domain 4 — Security, Observability & Compliance

- Alerts map to incident procedures and owners.
- Evidence and audit trails support incident reconstruction and reporting.

### Domain 5 — Configuration & Automation

- Change management procedures align with the automation workflow.
- Drift and failed applies are handled via documented response paths.

---

## Design rule

If operators cannot explain the system, the system is incomplete.

In this PoC, “explainable” includes:

- ownership (who is responsible);
- interfaces (how domains depend on each other);
- procedures (how to operate and recover);
- and evidence (how to prove what happened).

