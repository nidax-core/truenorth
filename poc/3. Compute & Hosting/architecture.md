# Architecture — Compute & Hosting

This document describes the architectural structure of the Compute & Hosting domain within True North.

It focuses on responsibilities, boundaries and operational guarantees not on specific products.

---

## Architectural overview

Compute & Hosting provides the substrate that runs workloads for other domains.
It defines how compute, networking, storage and recovery are delivered in a way that remains replaceable.

The substrate is split into two planes:

- **Control plane**: administration, provisioning, policy and observability.
- **Workload plane**: the runtime where services execute.

---

## Core components and roles

### Compute layer

Responsibilities:

- Run VMs and/or containers
- Enforce resource isolation (CPU, memory)
- Provide predictable scheduling and placement

### Network layer

Responsibilities:

- Segmentation between workloads (east/west)
- Ingress and egress control (north/south)
- Clear boundary between internal services and exposed entry points

### Storage layer

Responsibilities:

- Block storage for stateful services
- Object storage where appropriate
- Snapshots and lifecycle operations

Constraint:

- Storage choices must not leak vendor-specific assumptions into applications.

### Backup and recovery

Responsibilities:

- Automated backups for data and configuration
- Restore procedures that are documented and tested
- Measurable recovery targets (PoC-level RPO/RTO)

### Observability boundary

Responsibilities:

- Basic health and logging to diagnose failures
- Separation between platform logging and application logging

---

## Trust boundaries

- **Admin plane vs workload plane**: admin access is high risk and tightly controlled.
- **Network segmentation**: internal services are not implicitly reachable.
- **Secrets boundary**: credentials and tokens are never treated as configuration.

---

## Data flow examples

### Workload deployment flow

1. Workload image is built (VM template or container image)
2. Configuration is applied from a versioned source
3. Workload is scheduled onto the substrate
4. Service becomes reachable through defined ingress paths

### Backup and restore flow

1. Backups run on a defined schedule
2. Backups are stored with integrity guarantees
3. Restore is tested using a documented procedure
4. Recovery results are recorded (time, completeness, issues)

---

## Replaceability model

Infrastructure replacement is possible when the portable boundary is respected:

- Images are reproducible and platform-agnostic where feasible
- Configuration is not trapped in a UI-only workflow
- Data locations and backup procedures are explicit

The goal is not “no migration work”, but “no application rewrite”.

