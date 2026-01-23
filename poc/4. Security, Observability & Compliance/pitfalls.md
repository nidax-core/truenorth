# Pitfalls — Security, Observability & Compliance

This document captures real-world pitfalls encountered or anticipated in the Security, Observability & Compliance domain.

The purpose is to prevent “security theater” and to keep telemetry actionable.

---

## Observability pitfalls

### Logs without stable timestamps

Symptom:
Events cannot be ordered reliably, correlation fails, and investigations stall.

Mitigation:
Ensure consistent time sync and normalize timestamps on ingest.

### Metrics without logs

Symptom:
You can see something is wrong, but not what happened.

Mitigation:
Treat logs as first-class evidence for incidents and outages.

---

## Alerting pitfalls

### Alert fatigue

Symptom:
Alerts are ignored because most are noise.

Mitigation:
Reduce alert volume, increase context, and tune rules based on real outcomes.

### No runbooks, no outcomes

Symptom:
Even good alerts lead to inconsistent responses and no learning.

Mitigation:
Attach minimal response steps and record results (false positive, mitigated, accepted risk).

---

## Security pitfalls

### Telemetry blind spots

Symptom:
Critical systems do not emit logs, or agents are offline without detection.

Mitigation:
Monitor the monitoring: agent health, ingest health, and coverage.

### Evidence tampering

Symptom:
An attacker erases traces on the host.

Mitigation:
Ship logs off-host quickly and apply integrity controls to central storage.

---

## Compliance pitfalls

### Reports without provenance

Symptom:
Audit reports cannot be traced back to source events.

Mitigation:
Every reportable claim must link to evidence in the logging system.

### Retention without intent

Symptom:
Logs are kept “forever” without a reason, creating risk and cost.

Mitigation:
Define PoC-level retention targets and review them as requirements mature.

