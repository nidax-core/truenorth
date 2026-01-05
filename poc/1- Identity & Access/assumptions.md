# Assumptions

- All user identities are managed centrally within the Identity & Access domain
- A directory service acts as the authoritative identity substrate
- An identity broker enforces authentication flows and access policies where supported
- Authentication paths are protocol-dependent:
  - Web-based services authenticate via the identity broker (OIDC/SAML)
  - Non-web services authenticate directly against the directory
- Multi-factor authentication is enforced by the identity broker where applicable
- Connected services do not manage local user lifecycles
- Services trust identity interfaces, not each other
- Identity is treated as a hard dependency (fail-closed)
- Auditability and transparency are prioritized over user experience
- This PoC favors clarity and explicit design choices over completeness

© 2026 Nidax / True North
