# Scope — Collaboration & Productivity

This document defines the scope boundaries of the Collaboration & Productivity domain within the True North framework.

Scope is defined explicitly to prevent domain overlap, hidden coupling, and platform creep.

---

## In scope

This domain includes:

### Communication
- Email clients and mail access
- Chat and team communication tools
- Audio and video meetings

### Collaboration
- File storage and sharing
- Real-time document editing
- Comments and lightweight collaboration workflows

### Scheduling and presence
- Calendars and invitations
- Availability and presence indicators
- Meeting entry points

---

## Out of scope

This domain explicitly excludes:

### Identity & access
- Authentication and SSO
- User lifecycle management
- Authorization models and role definitions

(Handled in *Domain 1 — Identity & Access*)

### Analytics & data processing
- Reporting and dashboards
- Live database connectivity
- Business intelligence tooling

(Handled in *Domain 8 — Analytics & Insight*)

### Security enforcement
- Endpoint security and EDR
- Deep content inspection or classic DLP agents
- Network-level controls

(Handled in *Domain 4 Security, Observability & Compliance*)

### Infrastructure
- Compute, networking, storage
- Backup and disaster recovery
- Platform operations

(Handled in *Domain 3 — Compute & Hosting*)

---

## Scope guardrails

- The workspace may **surface** functionality, but must not **own** it
- No component in this domain may become a system of record for identity
- Collaboration tooling must integrate via open standards
- Adding features that blur domain boundaries is considered architectural drift

---

## Scope discipline

Expanding this domain requires:
- An explicit scope change
- A documented rationale
- Review against True North principles

Scope creep is treated as a design failure, not a feature request.
