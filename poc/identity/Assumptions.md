# Assumptions

- All users exist only in the central identity service
- All authentication flows go through IdentityGuard
- MFA is required for initial access
- Services fully trust the identity provider
- Local user management on services is disabled
- Identity is a hard dependency (fail-closed)
- Auditability is more important than user experience
- This PoC favors clarity over completeness
