# Scope — Integration Layer

This document defines what is included in the Integration Layer PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- Service-to-service authentication
- Event and webhook handling
- Lightweight APIs
- Cross-domain workflows

### Typical components (context-dependent)

- OIDC integrations
- Webhooks and message queues (where needed)
- Small, explicit glue services

---

## Out of scope

- End-user authentication UX and identity lifecycle management (Domain 1)
- Hosting substrate operation (Domain 3) beyond providing runtime and secrets injection
- Central logging/SIEM ownership (Domain 4), beyond producing telemetry
- Large data pipelines and analytics
- Shared databases as an integration mechanism

---

## Interfaces to other domains

### Domain 1 — Identity & Access

- Token issuance/validation patterns (OIDC)
- Service accounts and machine identities
- Claim and audience design for service-to-service authorization

### Domain 2 — Collaboration & Productivity

- Workflow triggers (e.g., provisioning notifications, mailbox/workspace events)
- Webhook consumers and producers

### Domain 3 — Compute & Hosting

- Runtime for glue services
- Secrets injection and network boundaries for integrations

### Domain 4 — Security, Observability & Compliance

- Correlation IDs and structured logs for cross-domain workflows
- Alerting hooks (webhook/email/chat) for security events

---

## Design rule

Glue code is allowed. Hidden coupling is not.

In this PoC, “hidden coupling” includes:

- shared databases between services;
- undocumented API dependencies;
- integration logic embedded in an unrelated service without an explicit contract;
- relying on UI-only configuration that cannot be reproduced.

