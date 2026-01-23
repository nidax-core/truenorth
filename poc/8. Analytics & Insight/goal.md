# Goal — Analytics & Insight

## Purpose

Turn system data into understanding and informed decisions.

Analytics & Insight focuses on interpretation: trends, KPIs, reporting and decision support.
It does not replace observability.
Observability shows what happened; analytics explains what it means.

---

## What success looks like (verifiable)

- A minimal set of “north star questions” is defined and answered using the analytics layer.
	- Example: capacity trends, security event trends, identity anomalies, collaboration adoption.
- At least one dashboard exists for each audience:
	- operators;
	- security;
	- management.
- KPI and SLA visibility uses explicit definitions (metric glossary) and stable time windows.
- Every insight is reproducible from:
	- underlying data;
	- a documented query;
	- and documented filters/time window.
- Analytics access is read-only by design and in practice.

---

## Non-goals (for this PoC)

- Building a full enterprise data platform or data lake.
- ML/AI pipelines.
- Making dashboards the only place where decisions can be made.
- Pushing actions back into operational systems (“analytics control plane”).

