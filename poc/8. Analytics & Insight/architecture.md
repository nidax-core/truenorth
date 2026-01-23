# Architecture — Analytics & Insight

This document describes the conceptual architecture for the Analytics & Insight domain.

The architecture is intentionally vendor-neutral.
Tools may change; definitions and reproducibility must remain.

---

## Architectural overview

Analytics & Insight sits downstream of collection.
Other domains produce and collect telemetry; this domain interprets it.

Conceptual flow:

1. Data sources (logs, metrics, events) produced across domains
2. Collection and storage (owned elsewhere)
3. Analytics access layer (read-only queries, curated views)
4. Semantic layer (definitions, naming, time windows)
5. Dashboards and reports
6. Read-only external consumers (optional)

---

## Typical components

### Query and view layer

- SQL views for relational stores
- OpenSearch (or similar) index patterns and saved queries
- Curated datasets that separate raw data from operational reporting

### BI layer (replaceable)

- Open-source BI tools (e.g. Apache Superset, Metabase)
- Scheduled reporting and dashboards

### External consumers (read-only)

- Export to external analytics consumers (e.g. Power BI) in read-only mode

---

## Reproducibility and definitions

Analytics is only trustworthy if the same inputs produce the same results.

This PoC treats the following as part of the architecture:

- a metric glossary for key KPIs/SLA indicators;
- canonical queries for each key insight;
- explicit time windows and filters (avoid “last 7 days” ambiguity);
- change history for dashboards and definitions.

Dashboards are views.
They must not be the only place where logic lives.

---

## Access and trust boundaries

- Read-only credentials for analytics consumers
- Least privilege: raw data access is restricted; curated views are preferred
- Separation of duties: dashboard builders are not automatically administrators of source systems
- Auditability: who viewed what and who changed what should be traceable (where feasible)

---

## Operational model

- A small number of dashboards is preferred over many inconsistent dashboards.
- Each dashboard has an owner and a review cadence.
- “Key questions” are agreed first; dashboards are built to answer them.

