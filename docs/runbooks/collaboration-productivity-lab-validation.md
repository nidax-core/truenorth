# Runbook — Validate Domain 2 lab (Collaboration & Productivity)

## Status
Validated in lab

## Owner
Collaboration & Productivity

## Last reviewed
2026-01-24

---

## Purpose
Validate that the “workspace” domain works end-to-end as a coherent, replaceable set of capabilities:
- users authenticate via the Identity domain (SSO where applicable);
- files and collaboration work across at least two users;
- integrations are machine-to-machine (editors are stateless);
- lifecycle/offboarding affects at least one capability via directory policy.

## When to use
- After initial Domain 2 lab build.
- After upgrades of workspace components (Nextcloud, editor engine, chat, mail).
- After Identity-domain changes impacting OIDC claims/groups.
- When users report document editing failures, sharing problems, or mail authorization anomalies.

## Preconditions
- Admin access to the workspace platform(s) and any integrated editor.
- Identity integration is available (OIDC/SAML/LDAP depending on component).
- Two test users (User Alice, User Bob) exist and can access the workspace.
- One shared test document exists (or can be created).

## Risks
- Testing sharing/permissions with real groups can accidentally broaden access.
- Restarting editors/mail components interrupts sessions.
- “Fixing” JWT/secrets without recording the previous state risks longer outages.

---

## Inputs
- Environment: lab
- Workspace platform(s):
- Online editor engine (if used):
- Chat/meetings tooling (if used):
- Mail stack (if used):

## Evidence to collect (before changes)
- Current component versions.
- Current integration endpoints (workspace → editor URL, broker URL).
- Relevant logs around failure windows (if troubleshooting).

---

## Procedure

1. Validate authentication is delegated to Identity.
	- Expected outcome: users authenticate via the broker/directory; the workspace does not own credentials.
	- Verify:
		- Login for User A succeeds.
		- Login for User B succeeds.
		- Group/role mapping appears as expected in the workspace (if used).
	- Evidence:
		- screenshot of successful login;
		- broker/workspace auth events.

2. Validate basic file operations.
	- Expected outcome: the workspace can create, upload, and download files.
	- Verify:
		- User A uploads a test file.
		- User A downloads it back and confirms contents.
	- Evidence:
		- file metadata (timestamp/size) and a short note of success.

3. Validate sharing/permissions between two users.
	- Expected outcome: User A shares a folder or file to User B with minimal permissions.
	- Verify:
		- User B can access the shared item.
		- Permission boundaries match the share setting (read vs edit).
	- Evidence:
		- screenshot of share settings and User B access.

4. Validate collaborative editing (two-user open/edit/save).
	- Expected outcome: both users can open the same document, edits appear, and save persists.
	- Verify:
		- User A opens a document in the editor.
		- User B opens the same document.
		- Both make a small edit; both see updates; save completes.
		- Re-open confirms persistence.
	- Evidence:
		- short screen recording or screenshots + a note of timestamps.

5. Validate editor trust model (machine-to-machine, stateless editor).
	- Expected outcome: the editor does not manage user lifecycle and relies on workspace tokens.
	- Verify:
		- editor has no local user management requirement;
		- integration uses a documented trust mechanism (often JWT).
	- If you see JWT/token errors:
		- use the dedicated runbook: `docs/runbooks/nextcloud-onlyoffice-jwt-mismatch.md`

6. Validate one offboarding-related capability changes via directory policy.
	- Expected outcome: updating a documented directory attribute changes a specific capability.
	- Verify (example):
		- set “mail send allowed” to false and confirm SMTP submission is rejected, while other access remains (as designed).
	- References:
		- Identity contract: `poc/1. Identity & Access/directory-policy-attributes.md`
		- Mail integration pattern: `poc/2. Collaboration & Productivity/self-hosted-mail.md`

---

## Validation (after checks)
- Two users can login.
- Two-user sharing works.
- Two-user document open/edit/save works.
- A known integration failure mode is covered by a runbook (JWT mismatch).
- One offboarding/policy attribute has a measurable effect on a capability.

## Rollback
This runbook is primarily validation. If changes were made during troubleshooting:
- revert to the recorded pre-change configuration (URLs, secrets, headers);
- restart only the component that needs to pick up config;
- repeat steps 1 and 4.

---

## Communication
- Record which step failed and attach evidence.
- If this was user-impacting: notify users once validation passes.

## Follow-ups
- Keep JWT/integration secrets in a single source of truth (avoid UI-only drift).
- Add a regular “two-user edit test” cadence after upgrades.

## References
- PoC docs: `poc/2. Collaboration & Productivity/README.md`
- Pitfalls: `poc/2. Collaboration & Productivity/pitfalls.md`
- JWT mismatch runbook: `docs/runbooks/nextcloud-onlyoffice-jwt-mismatch.md`
