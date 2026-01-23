# Architecture — Security, Observability & Compliance

This document describes the architectural structure of the Security, Observability & Compliance domain within True North.

It focuses on responsibilities, trust boundaries and data flows, not on specific products.

---

## Architectural overview

This domain collects telemetry from hosts and services, normalizes it, stores it centrally, and produces:

- searchable evidence (logs with provenance);
- detections (rules and correlation);
- and actionable alerts with context.

Conceptually, the flow is:

1. Producers (hosts, services, network sensors)
2. Collection and normalization
3. Central storage and search
4. Correlation and detection
5. Alert delivery and audit reporting

---

## Core components and roles

### Telemetry producers

Examples:

- OS logs (auth, sudo, systemd)
- Application logs (mail, web apps, directory services)
- Network sensors (optional)

Requirement:

- Logs must include timestamps and stable identifiers (host, service, user where applicable).

### Endpoint telemetry and integrity

Responsibilities:

- Collect host signals
- File integrity monitoring (FIM) for critical paths
- Basic intrusion-relevant detections

### Log pipeline (ingest and normalization)

Responsibilities:

- Ingest logs from multiple producers
- Parse and normalize to a consistent schema where possible
- Attach context: asset tags, environment, service identifiers

### Storage and search

Responsibilities:

- Store logs and detections with integrity guarantees
- Support search, correlation and evidence retrieval
- Apply retention and access controls

### Correlation and detection

Responsibilities:

- Define detections as rules (and keep them auditable)
- Correlate signals across hosts/services to reduce noise
- Produce alerts only when action is reasonable

### Alerting and response interface

Responsibilities:

- Deliver alerts to external channels (email/chat/webhook)
- Include context and evidence links
- Support a minimal response workflow (acknowledge, triage, record outcome)

### Reporting and audit

Responsibilities:

- Provide audit trails for key actions and events
- Support reporting that is traceable to source events

---

## Trust boundaries

- **Production plane vs logging plane**: telemetry systems must not become a hidden dependency for production runtime.
- **Access to logs**: logs may contain sensitive data; access is role-scoped and audited.
- **Tamper resistance**: evidence systems must assume an attacker may try to erase traces.

---

## Data flow examples

### Privilege escalation detection

1. A user performs a privileged action (sudo/admin)
2. Host logs are ingested and normalized
3. A correlation rule enriches the event with asset and identity context
4. An alert is emitted with:
	- affected host/service
	- identity details
	- raw log evidence reference
	- suggested response steps

### File integrity change

1. A critical file changes (FIM)
2. The change event is ingested
3. Baseline and allow-list logic reduces noise
4. An alert is emitted with:
	- file path
	- before/after hash (where available)
	- process/user context (where available)
	- evidence link

---

## Operational guarantees

- Alerts are explainable and traceable to source events.
- Observability data supports both debugging and security investigations.
- The system prioritizes clarity and provenance over alert volume.

