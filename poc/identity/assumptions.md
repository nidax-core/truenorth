# Assumptions

- All user identities are managed centrally in IdentityGuard
- All authentication flows are delegated to the central identity provider
- Multi-factor authentication is enforced by IdentityGuard for user access
- Connected services fully trust IdentityGuard and do not trust each other
- Local user management on services is disabled or not used
- Identity is treated as a hard dependency (fail-closed)
- Auditability and transparency are prioritized over user experience
- This PoC favors clarity and explicit design choices over completeness

© 2025 Nidax / True North