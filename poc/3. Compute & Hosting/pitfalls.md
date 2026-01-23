# Pitfalls — Compute & Hosting

This document captures real-world pitfalls encountered or anticipated in the Compute & Hosting domain.

The purpose is to prevent avoidable complexity and to keep the substrate understandable.

---

## Operational pitfalls

### Backups that are never restored

Symptom:
“We have backups” but recovery fails under pressure.

Mitigation:
Schedule restore tests and record results.

### Configuration drift

Symptom:
The running platform differs from the documented or versioned configuration.

Mitigation:
Prefer declarative configuration and treat manual changes as incidents.

---

## Security pitfalls

### Flat networks

Symptom:
Any compromised service can reach everything.

Mitigation:
Default-deny segmentation and explicit ingress/egress.

### Shared admin credentials

Symptom:
No accountability and no clean offboarding.

Mitigation:
Admin access is identity-driven, audited, and revocable.

---

## Portability traps

### Vendor-specific storage dependencies

Symptom:
Stateful services depend on a storage feature that cannot be reproduced elsewhere.

Mitigation:
Keep data placement explicit and validate restore/migration paths.

### UI-only configuration

Symptom:
Critical configuration lives only in a platform UI and is hard to reproduce.

Mitigation:
Export configuration or manage it as code.

