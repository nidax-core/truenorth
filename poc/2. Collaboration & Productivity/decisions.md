# Architectural Decisions — Collaboration & Productivity

This document records the key architectural decisions for the Collaboration & Productivity domain within True North.

The purpose is not to justify tooling choices, but to document **why certain patterns are accepted or rejected** to preserve clarity, replaceability and long-term autonomy.

---

## D1 — Workspace is a surface, not a system of record

**Decision**  
The workspace is treated as a user-facing surface that orchestrates collaboration tools.

**Rationale**
- Reduces coupling between tools and core domains
- Allows replacement without redesign
- Keeps identity and data ownership external

**Implication**
- Files, calendars and collaboration are surfaced here
- Identity, analytics and security controls live elsewhere

---

## D2 — Identity is never owned by collaboration tools

**Decision**  
No collaboration component may act as an identity provider.

**Rationale**
- Identity is a critical dependency and must be tool-agnostic
- Prevents lock-in through user databases
- Simplifies audits and migrations

**Implication**
- Workspace integrates with Domain 1 (Identity & Access)
- Editors, chat and meeting tools never authenticate users directly

---

## D3 — Nextcloud is a strong default, not a mandatory dependency

**Decision**  
A unified workspace (e.g. Nextcloud) is recommended as a default, but not required.

**Rationale**
- Reduces complexity for most organizations
- Provides a coherent user experience
- Avoids ideological dependency on a single platform

**Implication**
- All capabilities must remain replaceable
- Alternatives must integrate via open protocols

---

## D4 — Document editors are stateless services

**Decision**  
Document editing engines (e.g. ONLYOFFICE Docs) are treated as stateless processors.

**Rationale**
- Editors should not own files or users
- Prevents duplication of identity and permissions
- Enables future replacement

**Implication**
- Machine-to-machine trust (e.g. JWT) is used
- Editors do not persist user or file state

---

## D5 — Machine trust does not replace user authentication

**Decision**  
JWT or similar mechanisms are used for service integration, not for user login.

**Rationale**
- Separates system trust from human identity
- Avoids misuse of integration tokens
- Preserves a clean security model

**Implication**
- User authentication always flows through Identity domain
- Editors and meeting tools remain identity-agnostic

---

## D6 — Email remains a first-class, standards-based channel

**Decision**  
Email is retained as a core communication mechanism.

**Rationale**
- Universally interoperable
- Legally and operationally critical
- Not replaceable by chat systems

**Implication**
- Clients and mail handling are decoupled
- IMAP/SMTP/CalDAV/CardDAV are mandatory
- Workspace may surface mail, but does not own it

---

## D7 — Meetings are utilities, not platforms

**Decision**  
Audio and video conferencing tools are treated as replaceable utilities.

**Rationale**
- Prevents platform lock-in
- Avoids data gravity around meetings
- Keeps scheduling and identity centralized

**Implication**
- Meetings are scheduled via the workspace calendar
- Tools may be swapped without breaking workflows

---

## D8 — Collaboration tools are not analytics engines

**Decision**  
Spreadsheets and documents are not used for live data access or analytics.

**Rationale**
- Prevents data leakage
- Preserves auditability
- Keeps sources of truth centralized

**Implication**
- Analytics belongs in a separate domain
- Collaboration tools consume outputs, not raw data

---

## D9 — Defaults must always have an exit path

**Decision**  
Every default choice must have a documented and feasible replacement path.

**Rationale**
- Open source alone does not guarantee autonomy
- Organizational needs change over time
- Trust requires a visible exit

**Implication**
- Migration paths are considered at design time
- No component becomes indispensable by accident

---

## D10 — Email delivery is a bounded trust domain with multiple valid models

**Decision**  
Email delivery is treated as an external trust domain with clearly defined boundaries.  
True North does not mandate a single email hosting model, but defines a decision matrix and integration constraints.

**Rationale**
- Email is critical infrastructure with high operational, security and deliverability cost.
- For most organizations, a managed (preferably EU) provider reduces risk and operational burden while still meeting True North goals — provided boundaries, standards and an exit path are explicit.
- Some organizations require full control for threat model, compliance, or sovereignty reasons. In those cases, fully self-hosting is acceptable if the organization has (or is willing to hire) the necessary expertise and accepts the operational responsibility.
- Long-term autonomy is preserved through clear trust boundaries and open protocols, not by mandating a single hosting model.

**Guidance**
- Default: managed EU provider (lowest operational risk for most teams)
- Exceptional: fully self-hosted when control requirements justify the burden

**Accepted delivery models**

| Model | Description | Accepted | Default |
|------|------------|----------|---------|
| Fully self-hosted | Organization runs SMTP/IMAP stack end-to-end | Yes (exceptional) | No |
| Managed EU provider | External provider handles mail delivery & storage | Yes | **Yes** |
| Hybrid | Inbound/outbound filtering external, identity & policy internal | Yes | No |

**Design constraints (mandatory)**
- Identity remains owned by the Identity & Access domain
- Mail infrastructure must integrate with external identity (no local user silos)
- IMAP/SMTP and standard protocols remain available
- Data export and provider exit must be feasible
- Authentication and access events must be auditable

**Explicit non-goals**
- True North does not prescribe a specific mail server stack
- Mail delivery internals are not part of the reference architecture
- Feature parity with hyperscaler mail platforms is not a goal

**Implication**
- Email is interoperable, replaceable, and governed but not reinvented
- The workspace may surface mail functionality without owning mail delivery
- Responsibility boundaries between Identity, Workspace and Mail are explicit
- Reference implementation note available: self-hosted-mail.md (non-normative)


## Summary

These decisions ensure that the Collaboration & Productivity domain remains:
- Understandable
- Replaceable
- Auditable
- Resilient to vendor or ecosystem changes

They are intentionally conservative and prioritize long-term operational clarity over feature density.

