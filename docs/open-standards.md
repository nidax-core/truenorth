# Open Standards — True North

This document defines the open standards that form the interoperability backbone of the True North framework.

Open standards are not a preference; they are a **hard architectural requirement** to ensure replaceability, auditability, and long-term autonomy.

---

## Definition

Within True North, an open standard is a specification that:
- Is publicly documented
- Can be implemented without vendor permission
- Is not legally or technically restricted
- Has multiple independent implementations
- Does not require proprietary extensions to function

---

## Identity & Access

**Required**
- OIDC (OpenID Connect)
- OAuth 2.0
- SAML 2.0
- LDAP v3

**Explicitly excluded**
- Proprietary identity APIs
- Vendor-specific directory extensions

---

## Collaboration & Productivity

### File access
**Required**
- WebDAV

### Email
**Required**
- SMTP
- IMAP
- MIME

### Calendar & contacts
**Required**
- CalDAV
- CardDAV

### Document formats
**Required**
- Office Open XML (DOCX, XLSX, PPTX)
- OpenDocument Format (ODF)

---

## Communication

### Chat & messaging
**Recommended**
- Open APIs with documented export formats
- Message history export without proprietary tooling

(No single protocol is mandated due to ecosystem fragmentation.)

### Audio & video
**Recommended**
- WebRTC

---

## Integration & automation

**Required**
- HTTP(S)
- REST-style APIs
- JSON

**Recommended**
- Webhooks

---

## Observability & security

**Required**
- Syslog-compatible logging
- Structured log formats (JSON)

---

## What open standards do not mean

- Open source licensing is **not sufficient** on its own
- “Open core” models are treated with caution
- Open standards do not guarantee ethical behavior or stability
- Standards may evolve; lock-in comes from ignoring exit paths

---

## Architectural rule

> Any component that cannot integrate via the standards listed above is considered incompatible with True North, regardless of feature richness.

Exceptions require explicit documentation and justification.
