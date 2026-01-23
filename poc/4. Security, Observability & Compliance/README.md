# PoC — Security, Observability & Compliance (the nervous system)

This Proof of Concept describes how the **Security, Observability & Compliance** domain is implemented within the True North framework.

This domain is the **nervous system**: it makes security visible, measurable and explainable.
It does not replace good architecture; it provides the telemetry and auditability needed to operate with confidence.

---

## Design principles

### No security without logs
If an event cannot be observed, it cannot be defended or audited.

### No alerts without context
Alerts must link to evidence: raw events, affected assets and why it matters.

### Correlation over noise
Collect broadly, alert selectively.
Prefer a smaller number of high-quality signals over a high-volume alert stream.

### Security is explainable
Operators and auditors must be able to trace decisions back to source events.

---

## What this PoC validates

- Hosts and core services generate consistent telemetry (logs + basic metrics).
- Central aggregation supports search, correlation and evidence-preserving workflows.
- Alerts are delivered externally with enough context to act.
- Audit trails exist for administrative actions and critical changes.

---

## Typical components (context-dependent)

- Endpoint telemetry and detection (e.g., Wazuh)
- Search and correlation backend (e.g., OpenSearch / Elastic-compatible stack)
- Network visibility (e.g., Zeek)
- External alert delivery (email and/or Telegram)

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

2026 Nidax / True North
