# Runbook — Validate Domain 1 lab (Identity & Access)

## Status
Validated in lab

## Owner
Identity & Access

## Last reviewed
2026-01-24

---

## Purpose
Validate that the Identity domain works end-to-end as a reusable substrate:
- a directory as the authoritative source of identities;
- an identity broker for modern web authentication flows (OIDC);
- consistent lifecycle/policy enforcement across consumers.

This runbook is intentionally tool-agnostic; it focuses on observable behavior and evidence.

## When to use
- After initial lab build of Domain 1.
- After any change to directory schema, user lifecycle attributes, broker config, or TLS.
- When downstream domains report login problems (OIDC failures, token errors, group mapping issues).

## Preconditions
- Admin access to the directory service.
- Admin access to the identity broker (realm/tenant admin).
- At least:
	- 2 OIDC-relying services (e.g. “Nextcloud” + “Zulip”), and
	- 1 non-web consumer that authenticates against LDAP (e.g. IMAP/SMTP, VPN or PAM).
- Two test users (User Alice, User Bob) and a test group/role you can map.

## Risks
- Password resets and policy changes can lock out real users.
- Changing time sync, TLS or realm settings can break all relying parties.
- Testing with production-like accounts may create audit noise; prefer dedicated test identities.

---

## Inputs
- Environment: lab
- Directory type/version:
- Broker type/version:
- Relying parties tested:
- Ticket/incident (optional):

## Evidence to collect (before changes)
- Current broker realm export or configuration snapshot (if available).
- Current directory schema/LDIF references for policy attributes.
- Current NTP/chrony status from the broker host and one relying party.

---

## Procedure

1. Confirm time synchronization (critical dependency).
	- Expected outcome: systems agree on time and are actively synchronized.
	- Verify (examples):
		- `timedatectl status`
		- `chronyc tracking`
	- If fails: fix NTP/chrony first; do not troubleshoot OIDC tokens until time is correct.

2. Verify directory reachability and search/bind.
	- Expected outcome: directory accepts binds and returns expected entries.
	- Verify (examples):
		- `ldapsearch -x -H ldap(s)://<host> -D '<binddn>' -W -b '<base>' '(uid=<userA>)' dn`
	- If fails: confirm network/DNS/TLS and bind credentials; capture directory logs.

3. Verify broker ↔ directory integration.
	- Expected outcome: broker can authenticate a directory user and resolve groups/claims.
	- Verify:
		- Use the broker admin UI/CLI to test LDAP user lookup.
		- Attempt a broker login for User A.
	- Evidence:
		- broker logs around the login attempt.

4. Validate OIDC login flow for two relying parties.
	- Expected outcome: both services redirect to the broker, login succeeds, and sessions are established.
	- Verify:
		- User Alice login works in Service 1.
		- User Alice login works in Service 2.
		- Logout works (service session ends; broker session ends if applicable).
	- Evidence:
		- screenshots of successful login in both services;
		- broker event log entries for successful auth.

5. Validate authorization mapping (groups/roles).
	- Expected outcome: a directory group/attribute maps to service permissions (role/claim).
	- Verify:
		- Add User A to a known group in the directory.
		- Re-login (or refresh token) and confirm the expected permission appears in at least one relying party.
	- Evidence:
		- group membership record (directory); claim/role view in broker/service.

6. Validate non-web LDAP authentication for at least one consumer.
	- Expected outcome: the consumer authenticates directly against LDAP for User A.
	- Verify (examples):
		- IMAP/SMTP AUTH works; or
		- VPN login works; or
		- PAM login works on a test host.
	- Evidence:
		- consumer auth logs for successful login.

7. Validate an offboarding signal is enforceable (policy, not just auth).
	- Expected outcome: changing a documented policy attribute changes a capability outcome.
	- Verify (choose one):
		- disable access in broker and confirm OIDC login is blocked, or
		- adjust a service authorization attribute (e.g. “may send mail”) and confirm only that capability is denied.
	- Guidance:
		- Keep the contract in Identity docs; services should only consume it.
	- Reference:
		- `poc/1. Identity & Access/directory-policy-attributes.md`

---

## Validation (after checks)
- Time sync healthy on broker + at least one relying party.
- Two OIDC relying parties tested: login + logout.
- One non-web LDAP consumer tested.
- One authorization mapping verified (group/claim/role).
- Evidence captured and linked in the PoC notes.

## Rollback
This runbook is primarily validation. If you changed configuration while troubleshooting:
- revert to the last known good broker configuration/realm export;
- revert any directory schema/attribute changes;
- re-run steps 1–4 to confirm recovery.

---

## Communication
- If this validation was triggered by an incident: post a short summary of which step failed and attach the captured evidence.

## Follow-ups
- Add drift protection for broker config (export/backup + review cadence).
- Maintain a small set of dedicated test identities/groups.

## References
- PoC docs: `poc/1. Identity & Access/README.md`
- Policy attributes contract: `poc/1. Identity & Access/directory-policy-attributes.md`
- Pitfalls: `poc/1. Identity & Access/pitfalls.md`
