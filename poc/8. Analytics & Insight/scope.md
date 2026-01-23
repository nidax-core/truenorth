# Scope — Analytics & Insight

This document defines what is included in the Analytics & Insight PoC and what is explicitly excluded.

---

## In scope

### Responsibilities

- Aggregation and analysis of operational data
- Trend analysis and reporting
- Dashboards for operators, security teams and management
- KPI and SLA visibility
- Supporting audits and strategic decision-making

### Outputs

- Dashboards and scheduled reports
- A small metric glossary (definitions for key KPIs)
- Reproducible queries for each key insight

---

## Out of scope

- Telemetry collection agents and pipelines (owned by other domains)
- Operational automation triggered by analytics
- Business logic that exists only inside BI dashboards
- A vendor-specific analytics tool as a hard dependency

---

## Typical data sources (interfaces)

Analytics & Insight consumes data from other domains.

- Security logs and alerts (Security, Observability & Compliance)
- Usage and activity data (Collaboration & Productivity)
- Infrastructure metrics (Compute & Hosting)
- Identity and access events (Identity & Access)

---

## Data governance boundaries

Analytics increases the blast radius of data.
This PoC must explicitly define:

- what is collected and what is not;
- retention periods;
- how PII is handled (minimize, pseudonymize, restrict);
- who can access raw vs curated views;
- and how “audit evidence” is preserved.

---

## Design rule

Analytics consumes data; it never controls systems.

If analytics becomes a control plane (even indirectly), it breaks separation of concerns and makes failures harder to reason about.

