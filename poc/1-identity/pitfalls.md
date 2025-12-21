# Identity PoC – Pitfalls & Lessons Learned

This document captures real-world issues encountered during the Identity PoC.
These are not theoretical concerns, but failures observed during implementation.

---

## OIDC Pitfall: Time Drift between IdP and Clients

### Symptom
- Login flow redirects correctly
- Authentication succeeds at the IdP
- User is returned to the client but not logged in
- No visible error in the browser

### Observed Error
Zulip error log:
WARN [zulip.auth.oidc] Token error: The token is not yet valid (iat)

### Root Cause
- IdentityGuard (Keycloak) host had no active NTP service
- Issued tokens contained `iat` timestamps in the future
- Zulip strictly validates token timestamps

### Why this was confusing
- Nextcloud tolerated the time drift and worked correctly
- Zulip failed silently after redirect
- Initial instinct was to reset containers instead of inspecting client logs

### Resolution
- Installed and enabled NTP (chrony) on the IdentityGuard host
- Restarted Keycloak container to pick up corrected system time

### Lesson
Time synchronization is a hard security dependency in OIDC setups.
Always verify system clocks before debugging authentication logic.