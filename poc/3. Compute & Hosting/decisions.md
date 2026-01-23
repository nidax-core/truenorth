# Decisions — Compute & Hosting

This document captures decisions made during the Compute & Hosting PoC.

Decisions are recorded to keep the PoC auditable and to make trade-offs explicit.

---

## Decision 1 — Treat the substrate as replaceable

Context:
Infrastructure platforms differ, and platform lock-in often happens implicitly.

Decision:
Define a portability boundary (image + configuration + data) and treat the substrate as an implementation detail.

Consequences:

- Requires discipline in configuration management and data placement
- Reduces the risk of being forced into platform-specific features

---

## Decision 2 — Prefer boring primitives (VMs/containers) over platform magic

Context:
“Convenience” features can become hidden coupling.

Decision:
Prefer primitives that can be reproduced elsewhere (VM templates, container images, simple networking).

Consequences:

- Some automation may be slower to build
- Operational behavior becomes easier to reason about

---

## Decision 3 — Backups require restore tests

Context:
Backups without restores are a false sense of safety.

Decision:
Treat restore tests as part of the PoC definition of “backup”.

Consequences:

- Adds operational work
- Produces measurable confidence and clearer failure modes

---

## Decision 4 — Separate admin access from workload access

Context:
Most incidents and compromises escalate via administrative access paths.

Decision:
Keep a clear separation between control plane access and workload plane access.

Consequences:

- Requires clear documentation of “how to operate” vs “how to use”
- Reduces blast radius when a workload is compromised

