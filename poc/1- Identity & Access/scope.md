# Scope

## In scope
- Central identity domain (IdentityGuard)
- Separation of identity substrate and identity broker
- Directory service (LDAP) as the authoritative identity substrate
- Identity broker enforcing authentication flows and access policies
- Mandatory MFA where supported by protocol and client
- Single Sign-On to:
  - Nextcloud
  - Zulip
- Centralized role and group definitions
- No local user accounts on consuming services
- Authentication and authorization audit logs
- Fail-closed behavior when identity interfaces are unavailable
- Authentication and lookup support for system-level integrations
- Clear documentation of architectural choices and trade-offs

---

## Explicitly excluded
- Mattermost as a client  
  Reason: OpenID Connect support requires a paid edition, which conflicts
  with the vendor-neutral and open-by-default goals of this PoC

---

## Out of scope
- High availability or clustering
- Performance or load testing
- External identity providers
- Biometric or hardware-backed authentication
- Device posture or network trust evaluation
- Automated user provisioning or lifecycle management
- UX polish or theming
- Production hardening
- Policy definition or authentication flows implemented in LDAP
- User or group lifecycle management initiated from consuming systems
- Write access to the directory from consuming services

