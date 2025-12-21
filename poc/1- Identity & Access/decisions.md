# Design Decisions

This document records the key architectural decisions made for the
IdentityGuard Proof of Concept, including their rationale and trade-offs.

The goal is not to be exhaustive, but to make intent explicit.

---

## Why identity first

Identity is treated as the foundational layer of the True North ecosystem.
All other domains (collaboration, security, monitoring) depend on a clear,
centralized, and auditable identity model.

Starting with identity reduces ambiguity and prevents fragmented trust models
later in the stack.

---

## Why IdentityGuard with Keycloak

IdentityGuard is implemented using Keycloak because it provides:

- Native OpenID Connect support
- Centralized policy enforcement (including MFA)
- Strong auditability and transparency
- A mature, open-source ecosystem without vendor lock-in

No custom identity logic is implemented in this PoC.

---

## Why OpenID Connect

OpenID Connect is used as the sole authentication protocol because it:

- Is widely supported by modern open-source services
- Cleanly separates identity from applications
- Allows enforcement of security controls at the identity layer
- Keeps services stateless with respect to authentication

Legacy protocols and direct credential sharing are intentionally avoided.

---

## Why Nextcloud and Zulip as initial clients

Nextcloud and Zulip were selected as representative services because they:

- Are widely used in real-world collaboration environments
- Support OpenID Connect in their open-source editions
- Represent different collaboration patterns and identity usage:
  Nextcloud as a broad, multi-purpose collaboration platform,
  Zulip as a communication-first system with strong identity-driven access control.

They serve as realistic non-trivial clients for validating the identity model.

---

## Why Mattermost was excluded

Mattermost was initially considered as a client.
However, OpenID Connect support requires a paid edition.

This conflicts with the vendor-neutral and open-by-default goals of this PoC,
and Mattermost was therefore excluded.

---

## LDAP is supported as a directory interface not as an identity authority

**Decision**  
LDAP is supported as a read-only directory interface for system-level integrations.

**Rationale**
- Many infrastructure and legacy systems require LDAP
- OIDC/SAML are not universally supported
- Central identity must remain protocol-agnostic

**Implication**
- The IdP remains the single source of truth
- LDAP exposes a scoped projection of identity data
- No lifecycle management or write access is permitted via LDAP
- Systems consuming LDAP must not assume ownership of identity

This prevents legacy-compatible integrations from introducing lock-in or identity drift.


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
- Backup, recovery, and operational hardening

These concerns are valid but addressing them would obscure the core identity
model this PoC aims to validate.
