# Architecture — Operations & Support

This document describes the operational model of True North.

It focuses on people, processes, and the boundaries required to operate the system predictably.

---

## Architectural overview

Operations & Support is the human interface to the system.
It defines how day-2 work happens:

- onboarding and migrations;
- incident response;
- change handling;
- and user/admin support.

This domain is intentionally process-oriented.
It is still architecture: it defines responsibilities and trust boundaries.

---

## Roles and responsibilities

### Operator (day-2 operations)

Responsibilities:

- Execute runbooks
- Triage incidents and perform mitigation steps
- Validate system health after changes

### Administrator (privileged changes)

Responsibilities:

- Perform changes that require elevated access
- Maintain break-glass procedures
- Ensure privileged actions are auditable

### Support (user-facing)

Responsibilities:

- Single intake path for user issues
- Routing to owners and known-problem playbooks
- Communication during incidents affecting users

### Auditor/compliance viewer (read-only)

Responsibilities:

- Review evidence and audit trails
- Verify that procedures and controls are followed

---

## Operational flows

### Onboarding and migration

1. Intake: what is being onboarded/migrated and why
2. Preconditions: identity, access, data locations, constraints
3. Execution: documented steps and checkpoints
4. Validation: functional checks and rollback criteria
5. Handover: ownership and support path

### Incident handling

1. Detect: alert, user report, or operator observation
2. Triage: determine scope, impact, and owner
3. Mitigate: execute runbooks or safe containment steps
4. Recover: restore service to known-good state
5. Learn: document root cause, update runbooks, track follow-ups

### Change handling

1. Propose: change request with intent and risk
2. Review: peer review for critical changes
3. Apply: controlled execution (aligned with Domain 5)
4. Verify: health checks and evidence
5. Record: what changed, when, by whom

---

## Trust boundaries

- **Break-glass access**: defined, time-bounded where possible and always auditable.
- **Separation of duties**: changes that affect security boundaries should have review.
- **Support vs admin**: support can route and communicate; admin performs privileged steps.

---

## Definition of done

The system is operationally “done” for this PoC when:

- critical paths have runbooks;
- alerts map to owners and procedures;
- incident handling produces outcomes and improves documentation;
- and operators can explain system boundaries and dependencies.

