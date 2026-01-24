# Directory policy attributes (LDAP) — lifecycle and service authorization

This document defines how the Identity substrate (LDAP) expresses **user lifecycle state** and **service-specific authorization flags**.

It exists because many critical services (mail, VPN, PAM) cannot consume OIDC/SAML claims directly.
They must make authorization decisions using directory attributes.

The key is to keep this **owned by Identity**, while allowing other domains to **consume** it.

---

## Principles

- The directory is the **source of truth** for user state and lifecycle.
- Consuming systems **read** attributes; they do not invent lifecycle models.
- Separate **authentication** from **authorization**.
	- A user may be able to authenticate but still be blocked from a specific action (e.g. send mail).
- Prefer **explicit flags** over implicit heuristics.
- Custom attributes should be **namespaced** (e.g. `tn*`) to avoid collisions with vendor schemas.

---

## Attribute naming and mapping

The attribute names in this document (`tnAccountStatus`, `tnMailSendAllowed`) are **recommended examples**, not a hard requirement.

If your lab already uses different names (e.g. `mailAccountStatus` and `mailSendAllowed`), that is fine.
What matters is that:
- the meaning is documented;
- the names stay stable once chosen (treat them as a contract);
- consuming systems (like Postfix) use the same contract.

Example mapping (choose one set and standardize):

| Concept | Recommended name | Common alternative |
|---|---|---|
| Lifecycle state | `tnAccountStatus` | `mailAccountStatus` |
| May send mail | `tnMailSendAllowed` | `mailSendAllowed` |

---

## Minimal attribute model

The following model supports clean offboarding and “receive-only” style accounts:

### `tnAccountStatus`
High-level lifecycle state.

- Suggested values: `active`, `disabled`, `offboarded`
- Owner: Identity domain
- Consumers: any system that needs a coarse lifecycle signal

### `tnMailSendAllowed`
Service-specific authorization flag used by mail infrastructure.

- Type: boolean (or a controlled string like `TRUE`/`FALSE` depending on directory)
- Owner: Identity domain
- Consumers: mail domain (e.g., Postfix sender restrictions)

Design intent:
- `tnAccountStatus` can remain `active` while `tnMailSendAllowed=FALSE` (receive-only or temporarily restricted).
- Offboarding typically sets both:
	- `tnAccountStatus=offboarded`
	- `tnMailSendAllowed=FALSE`

---

## Implementing attributes in LDAP (schema)

How you add attributes depends on the directory implementation.

Common options:
- **OpenLDAP**: add a custom schema (cn=config) and attach via an **AUXILIARY** objectClass
- **389 Directory Server / FreeIPA / AD**: use their schema extension mechanisms (different tooling)

This PoC uses the OpenLDAP-style approach as a reference pattern.

### Naming and OIDs

LDAP schema attributes and objectClasses require OIDs.

- In production, use an organization-owned OID arc (often via a Private Enterprise Number).
- In PoC/lab, you can use a placeholder arc, but do not reuse it across organizations.

In the examples below we use:
- Base OID (example): `1.3.6.1.4.1.55555.1`

### Example: OpenLDAP schema as LDIF (cn=config)

This is an **example** schema definition. Adjust names, OIDs, and syntax to your directory conventions.

```ldif
# Add as cn=tnPolicy,cn=schema,cn=config

dn: cn=tnPolicy,cn=schema,cn=config
objectClass: olcSchemaConfig
cn: tnPolicy

# Attribute: tnAccountStatus
olcAttributeTypes: ( 1.3.6.1.4.1.55555.1.1
  NAME 'tnAccountStatus'
  DESC 'True North account lifecycle state'
  EQUALITY caseIgnoreMatch
  SYNTAX 1.3.6.1.4.1.1466.115.121.1.15
  SINGLE-VALUE )

# Attribute: tnMailSendAllowed
olcAttributeTypes: ( 1.3.6.1.4.1.55555.1.2
  NAME 'tnMailSendAllowed'
  DESC 'True North mail send authorization flag'
  EQUALITY booleanMatch
  SYNTAX 1.3.6.1.4.1.1466.115.121.1.7
  SINGLE-VALUE )

# AUX objectClass to attach attributes to inetOrgPerson entries
olcObjectClasses: ( 1.3.6.1.4.1.55555.1.10
  NAME 'tnPolicyAccount'
  DESC 'True North policy attributes'
  SUP top
  AUXILIARY
  MAY ( tnAccountStatus $ tnMailSendAllowed ) )
```

Operational note:
- The exact `dn:` and required permissions depend on how you manage OpenLDAP.
- Store schema definitions in configuration management, not as ad-hoc manual edits.

---

## Applying policy attributes to a user (LDIF)

Example: attach the auxiliary objectClass and set policy attributes for a user entry.

```ldif
# Example user DN; adapt to your DIT

dn: uid=alice,ou=users,dc=truenorth,dc=local
changetype: modify
add: objectClass
objectClass: tnPolicyAccount
-
replace: tnAccountStatus
tnAccountStatus: active
-
replace: tnMailSendAllowed
tnMailSendAllowed: TRUE
```

Example: offboard user but keep mailbox readable/receivable (authorization-level block).

```ldif
dn: uid=alice,ou=users,dc=truenorth,dc=local
changetype: modify
replace: tnAccountStatus
tnAccountStatus: offboarded
-
replace: tnMailSendAllowed
tnMailSendAllowed: FALSE
```

Apply the LDIF (example):

```bash
ldapmodify -x -H ldap://identityguard \
  -D "cn=admin,dc=truenorth,dc=local" -W \
  -f /tmp/disable-alice.ldif
```

---

## How consuming systems should use this

Consumers (e.g. Postfix) should:
- read these attributes;
- make a narrow decision at the boundary (SMTP transaction);
- avoid encoding lifecycle logic in multiple places.

For the mail domain implementation pattern, see:
- `poc/2. Collaboration & Productivity/self-hosted-mail.md`

