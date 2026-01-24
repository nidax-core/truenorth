# PoC — Compute & Hosting (the substrate)

This Proof of Concept describes how the **Compute & Hosting** domain is implemented within the True North framework.

This domain is the **substrate**: the predictable, portable runtime for workloads.
It exists to keep infrastructure replaceable and operations understandable.

The goal of this PoC is not to pick a single “best” platform, but to validate a clean model:

- clear responsibilities and boundaries;
- portable workloads;
- backup and recovery that is tested;
- and infrastructure that can be replaced without rewriting applications.

---

## Design principles

### Infrastructure must be replaceable
Replacing the substrate must not require changes to application code.

### Predictability beats features
Operational predictability, repeatability and auditable changes are more important than platform features.

### Separate control plane and workload plane
Administrative access and runtime access are different trust zones and must be treated as such.

### Backups are only real after restore
Backups without regular restore tests are not a capability.

---

## What this PoC validates

- Workloads can run on a defined “portable boundary” (images + configuration + data).
- Storage and networking responsibilities are explicit and do not leak into applications.
- Backup and recovery is measurable and reproducible.
- Trust boundaries are clear (admin plane vs workload plane).

---

## Status
In progress

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
- Lab runbook (k3s on libvirt): [compute-hosting-k3s-libvirt-lab.md](../../docs/runbooks/compute-hosting-k3s-libvirt-lab.md)

2026 Nidax / True North
