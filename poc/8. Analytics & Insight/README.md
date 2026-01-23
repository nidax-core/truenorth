# PoC — Analytics & Insight

This Proof of Concept describes how the **Analytics & Insight** domain is implemented within the True North framework.

This domain is the sense-making layer.
It turns system data into understanding and informed decisions.

True North deliberately separates **data collection** from **data interpretation**.
Logs, metrics and events are produced by other domains; this domain exists to make sense of them.

---

## Design rules

- Analytics consumes data; it never controls systems.
- No business logic lives exclusively in dashboards.
- No vendor-specific analytics tool is a hard dependency.
- All insights must be reproducible from the underlying data.

---

## What this PoC validates

- Operators, security teams and management can answer a small set of agreed questions from the same underlying data.
- KPI/SLA visibility is consistent, versioned and explainable.
- Dashboards and reports are reproducible: same query + same time window + same filters → same result.
- Analytics access is read-only and least-privilege.
- The BI layer is replaceable without losing definitions.

---

## Documents

- Goal: [goal.md](./goal.md)
- Scope: [scope.md](./scope.md)
- Architecture: [architecture.md](./architecture.md)
- Decisions: [decisions.md](./decisions.md)
- Pitfalls: [pitfalls.md](./pitfalls.md)

