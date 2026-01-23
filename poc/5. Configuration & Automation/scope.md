# Scope — Configuration & Automation

This document defines what is included in the Configuration & Automation PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- System configuration
- Service deployment
- Updates and drift control
- Environment documentation as code

### Typical components (context-dependent)

- Ansible
- Version-controlled configuration
- Secrets management

---

## Out of scope

- User lifecycle ownership and authentication policy (Domain 1 remains authoritative)
- Hosting substrate selection (Domain 3 provides targets; this domain drives configuration)
- Security detection and alerting logic (Domain 4), except emitting change/audit signals
- Application-level authorization models

---

## Interfaces to other domains

### Domain 1 — Identity & Access

- Automation runners and operators authenticate via the identity domain.
- Authorization to apply changes is controlled and auditable.
- Offboarding must revoke automation access cleanly.

### Domain 3 — Compute & Hosting

- Substrate provides the execution targets (hosts, VMs, clusters).
- This domain applies configuration and deploys services onto those targets.

### Domain 4 — Security, Observability & Compliance

- Change events and runner actions are logged centrally.
- Drift and failed applies can emit alerts with context.

---

## Design rule

If it cannot be reproduced, it is considered broken.

In this PoC, “reproducible” means:

- The desired state is version-controlled.
- The execution path is documented and repeatable.
- Inputs are explicit (inventory, environment parameters).
- Secrets are injected at runtime and are not committed.

