# Goal — Compute & Hosting

## Purpose

Run workloads predictably and portably.

This domain provides the substrate on which other domains run.
It should remain understandable, auditable and replaceable.

---

## What success looks like (verifiable)

- Workloads are deployable from a defined source of truth (repeatable process).
- Workloads can be moved to an alternative substrate with minimal operational work and no application rewrites.
- Backup and restore is tested and produces consistent results.
- Resource isolation is enforceable (compute, network, and storage boundaries).

---

## Non-goals (for this PoC)

- Enterprise HA across multiple regions.
- Full multi-tenant hosting platform for arbitrary external customers.
- Perfect platform standardization (Kubernetes everywhere, etc.).
- Replacing Domain 1 (Identity & Access) with platform-local accounts.

---

## Why this matters to True North

If the substrate becomes a hard dependency, every service becomes a hostage.
This PoC aims to keep the substrate a tool: replaceable, understandable, and operationally boring.

