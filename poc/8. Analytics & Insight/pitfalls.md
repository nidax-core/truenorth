# Pitfalls — Analytics & Insight

This document captures real-world pitfalls encountered or anticipated in the Analytics & Insight domain.

The goal is to keep analytics trustworthy, reproducible and safe.

---

## Meaning and incentives

### Vanity metrics

Symptom:
Dashboards show numbers that look good but do not measure outcomes.

Mitigation:
Define key questions first, then build dashboards to answer them.

### Metrics without context

Symptom:
Numbers are interpreted without understanding the inputs, time windows or filters.

Mitigation:
Use explicit definitions and stable time windows; document filters and assumptions.

---

## Reproducibility pitfalls

### Dashboard-only logic

Symptom:
Critical logic only exists inside a dashboard or visualization tool.

Mitigation:
Keep canonical queries and a metric glossary outside the BI layer.

### Definition drift

Symptom:
Two dashboards use the same term (e.g. “active users”) but compute it differently.

Mitigation:
Maintain shared definitions and review changes.

---

## Data governance pitfalls

### Accidental PII expansion

Symptom:
Combining datasets reveals more personal information than intended.

Mitigation:
Minimize and restrict; prefer curated views; define privacy boundaries early.

### Overbroad access to raw data

Symptom:
Too many users have raw access “because they need dashboards”.

Mitigation:
Prefer curated views and role-based access.

---

## Architecture pitfalls

### Analytics becomes a control plane

Symptom:
Dashboards trigger actions or become the source of truth for operational state.

Mitigation:
Keep analytics read-only and keep operational logic in operational domains.

### Vendor lock-in

Symptom:
Dashboards and definitions cannot be moved without a full rebuild.

Mitigation:
Keep definitions portable and treat the BI tool as replaceable.

