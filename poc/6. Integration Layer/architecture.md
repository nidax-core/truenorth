# Architecture — Integration Layer

This document describes the architectural structure of the Integration Layer domain within True North.

It focuses on responsibilities, trust boundaries and data flows, not on specific products.

---

## Architectural overview

The Integration Layer connects services using explicit, minimal contracts:

- synchronous APIs for request/response interactions;
- webhooks for external triggers;
- and (where justified) queues for buffering, retries and decoupling.

The integration layer is not a platform in itself.
It is a set of small adapters and workflows that make coupling visible.

---

## Core components and roles

### Service-to-service authentication

Responsibilities:

- Validate tokens (issuer, audience, expiry)
- Enforce authorization via explicit claims/scopes
- Support key rotation without breaking services

### Webhook receiver

Responsibilities:

- Validate authenticity (signatures/shared secrets)
- Enforce idempotency and deduplication
- Provide replay protection where relevant
- Normalize inbound events to an internal contract

### Message queue (optional)

Used when:

- producers cannot wait for consumers;
- delivery must be retried reliably;
- bursts must be buffered.

Constraint:

- a queue must not become a hidden dependency; contracts and ownership remain explicit.

### Glue services

Responsibilities:

- Implement one workflow or a small set of closely related workflows
- Keep logic explicit and documented
- Produce structured logs and outcomes

---

## Trust boundaries

- **External inbound vs internal calls**: webhooks are untrusted until validated.
- **Secrets boundary**: signing secrets and client secrets are protected and audited.
- **Blast radius control**: glue services should not have broad privileges “just in case”.

---

## Data flow examples

### Identity-driven offboarding signal

1. Domain 1 emits an “account disabled” event (or triggers a webhook)
2. Integration layer validates the message and normalizes it
3. Glue service calls downstream services with least-privilege machine identity
4. Outcomes are logged with correlation ID and evidence references

### Security alert to workspace notification

1. Domain 4 generates an alert
2. Integration layer forwards a context-rich notification to a Domain 2 channel
3. Notification includes evidence link references and correlation ID

### Provisioning workflow

1. A provisioning trigger is received (API/webhook)
2. Glue service calls the necessary domain endpoints in a defined order
3. Workflow is idempotent (retry safe) and produces a clear final state

---

## Replaceability model

Replaceability is achieved through explicit contracts:

- APIs are versioned and documented
- Event schemas are explicit and stable
- Workflows are small and isolated

Replacing a service changes the contract not the entire environment.

