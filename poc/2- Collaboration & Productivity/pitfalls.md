# Pitfalls — Collaboration & Productivity

This document captures common pitfalls and misunderstandings encountered when implementing the Collaboration & Productivity domain.

The goal is not to document bugs, but to prevent **conceptual mistakes** that lead to fragile or misleading setups.

---

## P1 — Confusing ONLYOFFICE session tokens with the JWT secret

**Symptom**  
After restarting the ONLYOFFICE document server, document editing stops working and error messages reference invalid or malformed tokens.

**Common misconception**  
It appears as if the JWT token has changed and must be updated manually in the workspace configuration.

**What actually happens**  
ONLYOFFICE uses multiple token types:
- A **static JWT secret** shared between the workspace and the document server
- **Ephemeral session tokens** generated at runtime

On restart:
- Session tokens are regenerated automatically
- The shared JWT secret remains unchanged

**Correct understanding**  
The JWT secret configured in the workspace (e.g. Nextcloud) must remain **stable**.  
It should **not** be updated after restarts.

If document editing fails after a restart, the cause is typically:
- Header name mismatch
- Secret mismatch due to configuration drift
- Incomplete container restart

**Why this matters**  
Confusing session tokens with integration secrets leads to unnecessary manual changes, broken trust relationships, and fragile operational procedures.

---

## Guiding principle

> Integration secrets are configuration.  
> Session tokens are runtime artifacts.

Only the former belongs in documentation and configuration management.
