# Architecture

This Proof of Concept uses a simple hub-and-spoke model.

IdentityGuard (Keycloak) acts as the central authority for authentication
and authorization. All connected services delegate identity and access
decisions to this central layer using OpenID Connect.

Nextcloud and Zulip function as independent service providers (spokes).
They do not manage user identities themselves and do not trust each other;
they only trust IdentityGuard.

The architecture is intentionally minimal to highlight trust boundaries,
dependency direction and audit flows.

## Authentication & Trust Flow

```mermaid
flowchart LR
    User((User))
    IdP["IdentityGuard (Keycloak)"]
    NC["Nextcloud"]
    ZU["Zulip"]

    User -->|Access request| NC
    User -->|Access request| ZU

    NC -->|OIDC redirect| IdP
    ZU -->|OIDC redirect| IdP

    IdP -->|OIDC token\n(MFA enforced)| NC
    IdP -->|OIDC token\n(MFA enforced)| ZU
```

© 2025 Nidax / True North