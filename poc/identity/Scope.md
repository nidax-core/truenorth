# Scope

## In scope
- Central identity service (IdentityGuard)
- Mandatory MFA at initial authentication
- Single Sign-On to:
  - Nextcloud
  - Mattermost
- Centralized role-based access control
- No local user accounts on services
- Authentication and authorization audit logs
- Fail-closed behavior when identity is unavailable
- Clear documentation of choices and trade-offs

## Out of scope
- High availability or clustering
- Performance or load testing
- External identity providers
- Biometric or hardware-backed authentication
- Device posture or network trust evaluation
- Automated user provisioning or lifecycle management
- UX polish or theming
- Production hardening
