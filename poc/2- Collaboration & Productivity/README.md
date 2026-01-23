# PoC — Collaboration & Productivity (the workspace)

This Proof of Concept describes how the **Collaboration & Productivity** domain is implemented within the True North framework.

This domain represents the **day-to-day digital workspace**: communication, files, meetings and collaborative work.  
It is where users spend most of their time, where clarity, predictability and trust matter most.

The goal of this PoC is not feature completeness, but to demonstrate a **coherent, human-scale workspace** built from well-defined components with clear responsibilities.

---

## Design Principles

### Default does not mean mandatory

True North recommends strong defaults to reduce complexity, but deliberately avoids hard dependencies on any single product or vendor.

The Collaboration & Productivity domain defines **capabilities**, not mandatory implementations.

### Default workspace
A unified workspace platform (e.g. Nextcloud) can provide:
- File storage and sharing
- Calendar and contacts
- Web-based collaboration
- Integrated meetings and chat

This offers a coherent user experience with minimal integration overhead.

### Deconstructed workspace (supported alternatives)
Organizations that require a higher degree of independence can choose to separate components without breaking the overall model:

- **Email clients**  
  Standards-based clients such as Thunderbird (IMAP/SMTP, CalDAV, CardDAV)

- **Mail handling**  
  Self-hosted mail servers or external privacy-focused providers (e.g. Proton Mail)

- **Chat & team collaboration**  
  Self-hosted tools such as Zulip for threaded, asynchronous communication

- **Online meetings**  
  Self-hosted video conferencing tools such as Jitsi Meet

All alternatives must integrate via open protocols and remain replaceable.

### Architectural requirement
Replacing or removing a workspace component must not:
- break identity flows
- force data migration into proprietary formats
- require redesign of other domains

This ensures that defaults remain choices, not traps.


### Collaboration tools do not own identity
All user authentication and lifecycle management is handled by the Identity domain.  
Workspace components consume identity; they never define it.

### Email is a first-class citizen
Email remains a core business communication channel:
- It is not replaced by chat
- It is not embedded into proprietary platforms
- It remains standards-based (SMTP, IMAP, MIME)

Mail clients and servers are interchangeable, and no workspace component assumes ownership of email identity.

### Meetings are tools, not platforms
Audio and video conferencing are treated as **utilities**, not ecosystems:
- No persistent user lock-in
- No hidden data capture
- No dependency for identity or file storage

Meeting tools integrate into the workspace but remain replaceable.

### Editors are stateless and replaceable
Document editing engines:
- Hold no user accounts
- Own no files
- Do not manage permissions

They operate purely as collaborative processors invoked by the workspace.

### Machine trust, not user trust
Integration between workspace components uses machine-to-machine trust (e.g. JWT).  
This is not user authentication and does not replace SSO.

---

## Collaboration is not analytics

Spreadsheets and documents are collaboration and presentation tools.  
They are **not** treated as:
- Sources of truth
- Live database clients
- Analytics engines

Data analysis belongs in a separate domain and feeds results into the workspace in controlled, auditable ways.

---

## Why this domain exists

Many platforms blur the line between:
- communication
- collaboration
- identity
- analytics
- control

This domain enforces separation while preserving a **coherent user experience**.

The result is a workspace that is:
- understandable to users
- explainable to operators
- auditable for security and compliance
- independent of any single vendor

---

## Outcome of this PoC

A validated workspace model where:
- Users experience a unified collaboration environment
- Email, chat and meetings coexist without overlap or lock-in
- Identity remains centralized elsewhere
- Editors and meeting tools remain replaceable
- Growth does not force architectural redesign

This PoC serves as a reference for production deployments and future domain expansions.

## Status
Validated in lab

## Validation
- [X] Lab validation performed
- [X] Runbooks tested end-to-end
- [X] Evidence captured (commands/logs/screens)

---

## Documents

- Goal: [goal.md](./goal.md)
- Scope: [scope.md](./scope.md)
- Architecture: [architecture.md](./architecture.md)
- Decisions: [decisions.md](./decisions.md)
- Pitfalls: [pitfalls.md](./pitfalls.md)
- Self Hosting Mail: [self-hosted-mail](./self-hosted-mail.md)

2026 Nidax / True North
