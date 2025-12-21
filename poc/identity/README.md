# Identity Proof of Concept

This Proof of Concept explores identity as the foundational layer of True North.

It demonstrates how a single central identity can securely and transparently
provide access to multiple services while remaining understandable, auditable and vendor-neutral.

In this PoC, Keycloak acts as the identity provider, with Nextcloud and Zulip
configured as OpenID Connect clients.

This PoC is intentionally minimal and tightly scoped.
It is not a production setup and makes no claims about scalability, availability or long-term operations.
It is based on a working lab setup but intentionally omits implementation details to keep the focus on architecture and design decisions. 

## Known Pitfalls

See [Pitfalls.md](./Pitfalls.md) for real-world issues encountered during this PoC.


© 2025 Nidax / True North