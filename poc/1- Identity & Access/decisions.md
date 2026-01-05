# Design Decisions

This document records the key architectural decisions made for the
IdentityGuard Proof of Concept including their rationale and trade-offs.

The goal is not to be exhaustive but to make intent explicit.

---

## Why identity first

Identity is treated as the foundational layer of the True North ecosystem.
All other domains (collaboration, security, monitoring) depend on a clear,
centralized and auditable identity model.

Starting with identity reduces ambiguity and prevents fragmented trust models
later in the stack.

---

## Why a layered IdentityGuard model

IdentityGuard is designed as a layered system:

- A directory service acting as the authoritative identity substrate
- An identity broker responsible for authentication flows and policy enforcement

This separation ensures that identity can be consumed consistently across both
modern web protocols and non-web or legacy systems.

---

## Why Keycloak as identity broker

Keycloak is used as the identity broker because it provides:

- Native OpenID Connect and SAML support
- Centralized authentication flows and MFA enforcement
- Policy-driven access control
- Strong auditability and transparency
- A mature, open-source ecosystem without vendor lock-in

Keycloak does not act as an identity store in this model.
It derives all identity data from the underlying directory.

---

## Why OpenID Connect for web-based services

OpenID Connect is used for web-based applications because it:

- Is widely supported by modern open-source services
- Cleanly separates identity from applications
- Enables centralized policy enforcement (including MFA)
- Keeps services stateless with respect to authentication

OIDC is used where protocol capabilities allow it, but is not treated as
a universal authentication mechanism.

---

## Why a directory (LDAP) as identity substrate

**Decision**  
A directory service (LDAP) acts as the authoritative identity substrate.

**Rationale**
- Many system-level and non-web protocols cannot consume OIDC or SAML
- Mail, VPN and PAM authentication require direct credential verification
- Identity lifecycle must remain centralized and protocol-agnostic

**Implication**
- User, group and credential data live in the directory
- Authentication may occur either:
  - via the identity broker (for web-based services), or
  - directly against the directory (for non-web services)
- No consuming system manages user lifecycle or credentials
- No write-back from consuming systems is permitted

This model preserves identity consistency without forcing protocol workarounds.

---

## Why Nextcloud and Zulip as initial clients

Nextcloud and Zulip were selected as representative services because they:

- Are widely used in real-world collaboration environments
- Support OpenID Connect in their open-source editions
- Represent different collaboration patterns and identity usage

They serve as realistic non-trivial clients for validating the identity model.

---

## Why Mattermost was excluded

Mattermost was initially considered as a client.
However, OpenID Connect support requires a paid edition.

This conflicts with the vendor-neutral and open-by-default goals of this PoC,
and Mattermost was therefore excluded.

---

## Why a single realm and minimal roles

The PoC uses a single realm and minimal role definitions to:

- Reduce cognitive overhead
- Make trust boundaries explicit
- Avoid premature complexity

Fine-grained authorization and multi-tenant setups are intentionally deferred.

---

## Why this PoC is not production-ready

This Proof of Concept deliberately excludes:

- High availability and clustering
- Automated provisioning and lifecycle management
- Backup, recovery and operational hardening

These concerns are valid but addressing them would obscure the core identity
model this PoC aims to validate.

© 2026 Nidax / True North