# Scope — Compute & Hosting

This document defines what is included in the Compute & Hosting PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- Virtual machines and containers
- Storage and networking
- Resource isolation
- Backup and recovery

### Typical components (context-dependent)

- Virtualization or orchestration platform (e.g., Proxmox, OpenStack, Kubernetes)
- Block and object storage
- Automated backups

---

## Out of scope

- User lifecycle and authentication policies (Domain 1)
- Workspace product decisions (Domain 2)
- Application-level authorization models
- Data governance, classification and retention policy (beyond backup/restore mechanics)
- External mail deliverability, spam handling and MX/DNS operations (belongs in mail-specific work)

---

## Interfaces to other domains

### Domain 1 — Identity & Access

- Administrative access to the substrate is driven by the identity domain.
- Substrate-local user management is treated as an implementation detail, not an authority.
- Secrets, API tokens and break-glass access must be auditable.

### Domain 2 — Collaboration & Productivity

- Domain 2 services run as workloads on this substrate.
- Domain 2 defines service requirements (ports, storage, performance, backup needs).
- This domain provides stable runtime, isolation and recovery.

---

## Design rule

Infrastructure must be replaceable without rewriting applications.

This PoC enforces a portability boundary:

- Workload identity: an image (VM template or container image)
- Workload configuration: versioned configuration and runtime parameters
- Data: stored in clearly defined locations with a backup/restore path

