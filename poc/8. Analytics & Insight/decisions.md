# Decisions — Analytics & Insight

This document captures decisions made during the Analytics & Insight PoC.

Decisions are recorded to keep analytics explainable, reproducible, and vendor-neutral.

---

## Decision 1 — Separate collection from interpretation

Context:
Collection is a reliability and security concern. Interpretation is a decision-support concern.
Mixing them creates unclear ownership and makes changes risky.

Decision:
Analytics consumes data produced by other domains.
It does not own collection agents or pipelines.

Consequences:

- Clear interfaces and responsibilities
- Analytics can evolve without destabilizing collection

---

## Decision 2 — Analytics is read-only by design

Context:
If analytics can modify systems, it becomes a control plane and increases failure coupling.

Decision:
Analytics access is read-only and least-privilege.

Consequences:

- Safer separation of concerns
- Some workflows require explicit handoff to operational domains

---

## Decision 3 — No business logic lives exclusively in dashboards

Context:
Dashboards change frequently and are often edited without the same rigor as code.

Decision:
Key logic must be reproducible outside the BI layer (queries + definitions).

Consequences:

- Better auditability and explainability
- Requires discipline in definition management

---

## Decision 4 — Vendor neutrality is a requirement

Context:
Analytics tools change.
Vendor lock-in turns dashboards into a hard dependency and blocks replacement.

Decision:
Treat the BI layer as replaceable; keep metric definitions and queries portable.

Consequences:

- Reduces long-term risk
- May limit tool-specific features

