# Pitfalls — Collaboration & Productivity

This document captures common pitfalls and misunderstandings encountered when implementing the Collaboration & Productivity domain.

The goal is not to document bugs, but to prevent **conceptual mistakes** that lead to fragile or misleading setups.

---

## P1 - Confusing ONLYOFFICE session tokens with the JWT secret

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

---

## P2 - JWT secret/header mismatch between Nextcloud and ONLYOFFICE

**Symptom**  
Users can open the file list, but opening/editing documents fails.
The editor may show errors like:
- "The document security token is not correctly formed"

**Root cause**  
Nextcloud and the ONLYOFFICE Document Server must share the *same* static JWT integration settings.
If either side is configured with a different value (or different header name), the Document Server rejects requests.

This typically happens due to:
- configuration drift (only one side was updated);
- environment variables changed on the Document Server but not in Nextcloud;
- header name mismatch (e.g. `Authorization` vs `JWT`), depending on the chosen deployment defaults.

**Fix**  
Align the JWT settings on both sides:
- Nextcloud ONLYOFFICE app: JWT secret (and header name if configurable)
- ONLYOFFICE Document Server: JWT secret and header name

Then restart the Document Server (and, if applicable, the reverse proxy) and re-test by opening a document.

**Prevention**  
Treat the JWT secret as managed configuration:
- generate a strong secret and keep it stable;
- store it in configuration management / secrets management;
- document rotation steps explicitly (avoid “change it in the UI and hope”).

See: `docs/runbooks/nextcloud-onlyoffice-jwt-mismatch.md`
