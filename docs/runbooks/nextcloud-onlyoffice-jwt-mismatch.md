# Runbook — Fix Nextcloud + ONLYOFFICE JWT mismatch

## Status
Validated in lab

## Owner
Collaboration & Productivity

## Last reviewed
2026-01-24

---

## Purpose
Restore document editing when the Nextcloud ↔ ONLYOFFICE integration fails due to mismatched JWT configuration.

## When to use
- Users report that opening/editing office documents fails.
- The ONLYOFFICE editor shows an error such as: "The document security token is not correctly formed".
- Nextcloud logs show ONLYOFFICE requests failing (often 401/403 style failures).

## Preconditions
- Admin access to Nextcloud (to view/update the ONLYOFFICE app settings)
- Admin access to the ONLYOFFICE Document Server deployment (container/env/config)
- Ability to restart the Document Server safely

## Risks
- Changing the JWT secret breaks integration immediately until both sides match.
- Secret rotation can cause temporary user impact.
- Restarting the Document Server interrupts active editing sessions.

---

## Inputs
- Environment: lab / production
- Nextcloud URL: 
- ONLYOFFICE Document Server URL: 
- Incident ID / ticket: 

## Evidence to collect (before changes)
- Screenshot / exact error text from the user
- Nextcloud logs around the failure time
- Document Server logs around the failure time
- Current integration settings (record what they are before changing)

---

## Procedure

1. Confirm the symptom is JWT-related.
	- Expected outcome: the error matches a token/security issue, not a general connectivity failure.
	- Verify: error text includes “security token” / “token not correctly formed”, or logs show auth failure between Nextcloud and Document Server.

2. Check the Nextcloud ONLYOFFICE app integration settings.
	- Expected outcome: you can view the configured JWT secret (or secret key) and any related token/header settings.
	- Verify: Nextcloud Admin settings → ONLYOFFICE (exact menu may differ by version).
	- Record: the configured JWT secret and header name (if shown/configurable).

3. Check the ONLYOFFICE Document Server JWT configuration.
	- Expected outcome: you can determine the active JWT secret and header name.
	- Verify (common patterns):
		- Docker / compose: environment variables such as `JWT_ENABLED`, `JWT_SECRET`, `JWT_HEADER`
		- Config files: check the Document Server configuration for JWT/token settings
	- Record: the configured JWT secret and header name.

### Deployment-specific examples (optional)

These snippets are **examples** to speed up troubleshooting. They are intentionally optional because deployments vary.
Use placeholders like `<service>`, `<namespace>`, `<container>` and adapt to your environment.

#### Docker Compose

- Inspect runtime JWT-related environment (example):
	- `docker compose exec -it <onlyoffice-service> printenv | grep -E '^JWT_'`
	- Expected outcome: you see `JWT_ENABLED`, `JWT_SECRET` and optionally `JWT_HEADER`.

- Restart the Document Server (example):
	- `docker compose restart <onlyoffice-service>`

- Check logs around the failure window (example):
	- `docker compose logs --since=30m <onlyoffice-service>`

#### Kubernetes

- Identify where the JWT secret is injected (ConfigMap/Secret) (example):
	- `kubectl -n <namespace> describe deploy/<onlyoffice-deployment> | sed -n '/Environment:/,/Mounts:/p'`

- Restart to pick up new config (example):
	- `kubectl -n <namespace> rollout restart deploy/<onlyoffice-deployment>`
	- `kubectl -n <namespace> rollout status deploy/<onlyoffice-deployment>`

#### systemd / bare metal

- Restart service (example):
	- `sudo systemctl restart onlyoffice-documentserver`
	- `sudo systemctl status onlyoffice-documentserver --no-pager`

- Locate configuration (varies by distro/version):
	- Check the vendor documentation for where JWT settings are stored.
	- Prefer not to edit packaged defaults without tracking changes.

4. Align settings (choose one source of truth).
	- Expected outcome: both Nextcloud and Document Server are configured with the same JWT secret and same header name.
	- Guidance:
		- Prefer treating the secret as “infrastructure config” (secrets manager / config management), not a UI-only value.
		- Use a strong random secret; avoid short or guessable strings.
		- Never paste the secret into tickets or chat; store it in an approved secret store.

5. Restart the ONLYOFFICE Document Server.
	- Expected outcome: the Document Server picks up the updated configuration.
	- Verify: service is healthy, logs show normal startup.

6. Validate end-to-end.
	- Expected outcome: opening and editing a document works for at least two users.
	- Verify:
		- Open a document in Nextcloud → editor loads
		- Edit + save works
		- Optional: confirm no new auth errors in logs

---

## Validation (after changes)
- Document open/edit/save confirmed working
- No recurring auth/token errors in logs during a short observation window

## Rollback
- If validation fails and you changed values: restore the previous JWT secret/header values on both sides (from your recorded evidence) and restart the Document Server.

## Communication
- Notify impacted users once validated.
- Record the final secret location (where it is stored/managed) and any follow-up tasks.

## Follow-ups
- Add configuration drift protection (e.g. single source of truth for the JWT secret).
- Document the approved rotation procedure (planned downtime window, step order, verification).
- If using containers: consider making the JWT settings explicit in IaC (compose/helm) to avoid UI-only changes.

## References
- PoC pitfalls: `poc/2. Collaboration & Productivity/pitfalls.md`
