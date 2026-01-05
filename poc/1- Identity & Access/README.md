# Identity Proof of Concept

This Proof of Concept explores identity as the foundational layer of True North.

It demonstrates how a single, centralized and auditable identity domain can
provide secure access to multiple services while remaining understandable,
transparent and vendor-neutral.

---

## Scope and focus

The PoC is centered around **IdentityGuard**, which is implemented as a layered system:

- A directory service acting as the authoritative identity substrate
- An identity broker (Keycloak) responsible for authentication flows, MFA and policy enforcement

This separation allows identity to be consumed consistently across both modern
web-based services and non-web or system-level integrations.

---

## Demonstrated integrations

- **Nextcloud** and **Zulip** authenticate via OpenID Connect against the identity broker
- **Non-web protocols** (e.g. mail, VPN, PAM) authenticate directly against the directory

No connected service manages its own user lifecycle or credentials.

---

## Architectural intent

This Proof of Concept demonstrates that:

- Identity is centralized but protocol-aware
- Authentication mechanisms are chosen based on protocol capabilities, not ideology
- User lifecycle is enforced consistently across all services
- Vendor lock-in is avoided by design, not by abstraction

---

## Non-goals

This PoC is intentionally minimal and tightly scoped.

It does not attempt to address:
- High availability or clustering
- Automated provisioning or deprovisioning
- Backup, recovery or operational hardening
- Performance or scalability guarantees

The focus is on architectural correctness, not production readiness.

---

## Known pitfalls

See [pitfalls.md](./pitfalls.md) for real-world issues encountered and lessons learned during this PoC.

© 2026 Nidax / True North
