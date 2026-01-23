# PoC — Configuration & Automation (the control plane)

This Proof of Concept describes how the **Configuration & Automation** domain is implemented within the True North framework.

This domain is the **control plane**: it keeps systems consistent, repeatable and operable as the environment grows.
It turns “how things are run” into something that is reviewable, reproducible and auditable.

The goal of this PoC is not to showcase tools, but to validate the operating model:

- configuration is versioned;
- deployments are repeatable;
- drift is detected and handled;
- and environments can be rebuilt from a defined source of truth.

---

## Design principles

### Reproducibility is the baseline
If it cannot be reproduced, it is considered broken.

### Declarative intent, verifiable outcomes
Automation should express intent, apply changes predictably, and verify results.

### Drift is a signal
Drift is not “normal”; it is either a bug or an intentional change that must be recorded.

### Secrets are boundaries
Secrets are never configuration and never documentation.

---

## What this PoC validates

- A new host can be bootstrapped reproducibly.
- A service can be deployed repeatedly from versioned configuration.
- Updates can be applied with a controlled process.
- Drift can be detected and either remediated or consciously accepted.

---

## Typical components (context-dependent)

- Ansible
- Version-controlled configuration (Git)
- Secrets management (vault/agent/Ansible Vault)

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

