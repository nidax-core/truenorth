# PoC — Integration Layer (the glue)

This Proof of Concept describes how the **Integration Layer** domain is implemented within the True North framework.

This domain is the **glue** between domains and services.
Its purpose is to enable cross-domain workflows without tight coupling.

The goal of this PoC is not to build a large integration platform.
It is to validate a clean model where integrations are explicit, auditable and replaceable.

---

## Design principles

### Glue code is allowed. Hidden coupling is not.
Integrations may require small services and adapters, but dependencies must be explicit and documented.

### Contracts over convenience
Prefer small, stable interfaces (APIs/events) over shared databases or undocumented behavior.

### Identity-aware, least-privilege
Service-to-service authentication is mandatory and scoped to the minimum required capability.

### Observable by default
Cross-domain workflows must be traceable end-to-end (correlation IDs, logs, outcomes).

---

## What this PoC validates

- Service-to-service authentication works across domains without embedding identity logic into every service.
- Webhooks and event-driven triggers can be handled reliably and idempotently.
- Lightweight glue services remain small and replaceable.
- Cross-domain workflows are auditable and explainable.

---

## Typical components (context-dependent)

- OIDC integrations
- Webhooks (and optionally message queues where needed)
- Small, explicit glue services

---

## Status
Draft (theory-only). This PoC is currently documentation-only and has not been validated in practice yet.

## Validation
- [ ] Lab validation performed
- [ ] Runbooks tested end-to-end
- [ ] Evidence captured (commands/logs/screens)

---


## Documents

- Goal: [goal.md](./goal.md)
- Scope: [scope.md](./scope.md)
- Architecture: [architecture.md](./architecture.md)
- Decisions: [decisions.md](./decisions.md)
- Pitfalls: [pitfalls.md](./pitfalls.md)

2026 Nidax / True North
