# True North — Portfolio & Evidence Index

This page is a practical index of what has been validated in the True North repo so far and where to find the evidence.

It is written to help:
- future you (repeatability),
- reviewers (auditability),
- and potential adopters (clarity).

---

## What this repo demonstrates

- A domain-driven framework for a vendor-neutral digital workspace.
- Real integration pitfalls and operational runbooks (not just architecture diagrams).
- A bias towards reproducibility: configuration as a contract, restore-tests as proof.

---

## Evidence (validated items)

### Domain 1 — Identity & Access

**Validated in lab**
- Central identity substrate (LDAP) + broker model (Keycloak)
- Clear identity consumption model (web services via OIDC; non-web via LDAP)

**Operational evidence**
- Domain 1 lab validation runbook:
	- `docs/runbooks/identity-access-lab-validation.md`
- Lifecycle/policy attributes contract (LDAP schema + LDIF examples):
  - `poc/1. Identity & Access/directory-policy-attributes.md`

**Pitfalls captured**
- Time drift causing OIDC token validity failures:
  - `poc/1. Identity & Access/pitfalls.md`

### Domain 2 — Collaboration & Productivity

**Validated in lab**
- Workspace model with replaceable components (Nextcloud, editors, chat, mail as separate capabilities)

**Operational evidence**
- Domain 2 lab validation runbook:
	- `docs/runbooks/collaboration-productivity-lab-validation.md`
- Nextcloud ↔ ONLYOFFICE JWT mismatch fix runbook:
  - `docs/runbooks/nextcloud-onlyoffice-jwt-mismatch.md`

**Pitfalls captured**
- ONLYOFFICE session tokens vs JWT secret confusion:
  - `poc/2. Collaboration & Productivity/pitfalls.md`
- Nextcloud/ONLYOFFICE JWT secret/header mismatch:
  - `poc/2. Collaboration & Productivity/pitfalls.md`

**Integration patterns**
- Mail send authorization driven from LDAP (authorization != authentication):
  - `poc/2. Collaboration & Productivity/self-hosted-mail.md`

### Domain 3 — Compute & Hosting

**In progress**
- Substrate model: control plane vs workload plane, boring primitives, replaceability boundary

**Operational evidence (lab bootstrap runbook)**
- libvirt + Debian 12 + k3s + backup/restore test runbook:
  - `docs/runbooks/compute-hosting-k3s-libvirt-lab.md`

---

## Suggested acceptance criteria (for “Validated in lab”)

These are lightweight, measurable checkpoints you can use to promote a PoC status.

### Domain 1 (Identity)
- 2 web apps authenticate via OIDC (login + logout + MFA where applicable)
- 1 non-web protocol authenticates via LDAP
- A documented and tested time-sync dependency (NTP/chrony)

### Domain 2 (Collaboration)
- A document can be opened/edited/saved by two users
- A known pitfall is reproducibly diagnosed and fixed using a runbook
- Offboarding affects at least one capability (e.g., mail sending) via directory policy

### Domain 3 (Compute/Hosting)
- A workload with persistent data is deployed
- Backup is taken
- Restore recreates workload and data (proof file)
- Evidence is recorded (commands/logs/time-to-restore)

---

## References

- Mission: `docs/mission.md`
- Vision: `docs/vision.md`
- Open standards: `docs/open-standards.md`
