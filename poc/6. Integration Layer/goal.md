# Goal — Integration Layer

## Purpose

Tie everything together without tight coupling.

The Integration Layer exists to support cross-domain workflows (Identity, Workspace, Hosting, Security) without turning any single domain into a hidden dependency.

---

## What success looks like (verifiable)

- Cross-domain workflows are implemented via explicit interfaces (APIs/events/webhooks), not shared databases.
- Service-to-service authentication is consistent and enforceable (issuer/audience/claims are defined).
- Integrations are observable end-to-end (correlation ID, evidence logs, clear outcomes).
- Webhook and event handling is reliable (retries, idempotency, deduplication).
- Replacing a service does not require redesigning other domains; only the explicit contract changes.

---

## Non-goals (for this PoC)

- Building an ESB or large orchestration platform.
- Hiding business logic inside integration glue.
- Creating a universal “event bus” for everything.
- Replacing Domain 1 identity authority with per-service identity logic.

---

## Why this matters to True North

Integrations are where lock-in often happens.
This PoC keeps integration understandable and replaceable by making coupling explicit.

