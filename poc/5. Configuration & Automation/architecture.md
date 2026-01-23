# Architecture — Configuration & Automation

This document describes the architectural structure of the Configuration & Automation domain within True North.

It focuses on responsibilities, trust boundaries and operational flows not on a single tool.

---

## Architectural overview

This domain defines how changes are introduced into the environment:

1. Version-controlled intent (configuration and documentation)
2. Controlled execution (runner)
3. Change application (targets)
4. Verification and drift detection
5. Audit logging and accountability

The control plane must remain understandable and reproducible.

---

## Core components and roles

### Configuration repository

Responsibilities:

- Single source of truth for desired state
- Reviewable changes (PRs)
- Version history for audits and rollback reasoning

### Inventory and environment model

Responsibilities:

- Define targets (hosts, groups, environments)
- Express environment-specific parameters without duplicating logic

### Execution environment (runner)

Responsibilities:

- Execute automation with controlled credentials
- Produce logs that can be correlated to changes
- Enforce a consistent toolchain version (reduce “works on my laptop”)

### Secrets boundary

Responsibilities:

- Store and deliver secrets to automation at runtime
- Prevent secrets from entering Git, logs, or documentation

### Verification and drift control

Responsibilities:

- Verify that desired state was applied
- Detect drift and classify it (unexpected vs approved)
- Support remediation workflows

---

## Trust boundaries

- **Humans vs runners**: operators propose changes; runners execute them.
- **Prod vs non-prod**: environment separation is explicit.
- **Secrets**: access is least-privilege, time-bounded where possible, and audited.

---

## Flow examples

### Bootstrap a new host

1. Target is added to inventory
2. Baseline configuration is applied
3. Host emits telemetry (Domain 4) and becomes a managed asset

### Deploy a service change

1. Change is proposed and reviewed
2. Runner applies change to targets
3. Verification confirms intended state
4. Logs and outcomes are recorded

### Drift detection and response

1. Drift is detected
2. Evidence is collected (what changed, where, when)
3. Decision is made: remediate or accept and record

