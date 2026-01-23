# Pitfalls — Operations & Support

This document captures real-world pitfalls encountered or anticipated in the Operations & Support domain.

The purpose is to keep the system operable and explainable.

---

## Documentation pitfalls

### Tribal knowledge

Symptom:
Only one person knows how to do a critical operation.

Mitigation:
Document runbooks and validate them by execution, not by intention.

### Runbooks rot

Symptom:
Documentation exists, but it no longer matches reality.

Mitigation:
Treat runbooks as living artifacts; update them after changes and incidents.

---

## Incident pitfalls

### No clear ownership

Symptom:
Incidents stall because no one knows who is responsible.

Mitigation:
Maintain ownership mappings and escalation paths.

### Alert fatigue

Symptom:
Real alerts are ignored because most alerts are noise.

Mitigation:
Tune alerts, require context, and close the loop with outcomes.

---

## Access pitfalls

### Break-glass becomes the default

Symptom:
Emergency access is used for normal work.

Mitigation:
Restrict break-glass, audit it, and fix underlying access issues.

### Privileged changes without evidence

Symptom:
Changes are made but cannot be reconstructed.

Mitigation:
Log privileged actions and require change records for critical operations.

