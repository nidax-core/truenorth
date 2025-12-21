# Scope

## In scope
- Central identity service (IdentityGuard)
- Mandatory MFA at initial authentication
- Single Sign-On to:
  - Nextcloud
  - Zulip
- Centralized role-based access control
- No local user accounts on services
- Authentication and authorization audit logs
- Fail-closed behavior when identity is unavailable
- Clear documentation of choices and trade-offs
- LDAP directory access as a read-only projection of centrally managed identities
- Authentication and lookup support for system-level integrations

## Explicitly excluded
- Mattermost as a client  
  Reason: OpenID Connect support requires a paid edition, which conflicts
  with the vendor-neutral and open-by-default goals of this PoC

## Out of scope
- High availability or clustering
- Performance or load testing
- External identity providers
- Biometric or hardware-backed authentication
- Device posture or network trust evaluation
- Automated user provisioning or lifecycle management
- UX polish or theming
- Production hardening
- LDAP as a primary identity store
- User or group lifecycle management via LDAP
- Write access from consuming systems


© 2025 Nidax / True North