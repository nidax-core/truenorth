# Architecture — Collaboration & Productivity

This document describes the architectural structure of the Collaboration & Productivity domain within True North.

It focuses on **responsibilities, trust boundaries, and data flows**, not on specific products.

---

## Architectural overview

The Collaboration & Productivity domain is centered around a **workspace layer** that orchestrates communication and collaboration tools, while delegating identity and security to other domains.

High-level responsibilities:
- Provide a coherent user workspace
- Integrate collaboration tools without tight coupling
- Enforce policies at the workspace boundary
- Remain replaceable as a whole or in parts

---

## Core components and roles

### Workspace layer
The workspace layer acts as the primary interaction surface for users.

Responsibilities:
- File storage and sharing
- Calendar and contacts
- Entry point for meetings and collaboration
- Policy enforcement (sharing, access scopes)

Example implementation:
- Nextcloud

The workspace layer does **not** own identity or authentication logic.

---

### Document editing service
A stateless document editing service provides real-time collaboration capabilities.

Responsibilities:
- Render and edit documents
- Handle concurrent editing sessions
- Return results to the workspace

Constraints:
- No user accounts
- No file ownership
- No identity awareness

Trust model:
- Machine-to-machine trust (e.g. JWT) with the workspace

Example implementation:
- ONLYOFFICE Docs

---

### Communication services

#### Email
Email consists of two independent components:

- **Mail clients**  
  Used by end users to read and compose messages  
  (e.g. Thunderbird, webmail)

- **Mail handling**  
  Responsible for SMTP/IMAP, filtering, storage, and delivery  
  (self-hosted or external provider)

The workspace may surface email functionality but does not replace mail infrastructure.

---

#### Chat and team collaboration
Chat tools provide asynchronous communication and team coordination.

Characteristics:
- Threaded or channel-based communication
- No ownership of files or identity
- Optional integration into the workspace UI

Example implementation:
- Zulip

---

#### Online meetings
Meeting tools provide real-time audio and video communication.

Characteristics:
- Invoked from calendar events
- No persistent ownership of user identity
- Replaceable without breaking other components

Example implementation:
- Nextcloud Talk, Jitsi Meet

---

## Identity and trust boundaries

- User identity is managed exclusively in **Domain 1 — Identity & Access**
- Workspace and collaboration tools consume identity via standard protocols
- Editors and meeting tools never authenticate users directly

This separation ensures that collaboration tools remain replaceable and auditable.

---

## Data flow examples

### Document collaboration flow
1. User authenticates to the workspace via Identity domain
2. User opens a document in the workspace
3. Workspace invokes the document editor using machine trust
4. Editor processes edits and returns results
5. Workspace stores and controls the file

---

### Meeting scheduling flow
1. User creates a calendar event in the workspace
2. Workspace generates a meeting link
3. Participants join via the meeting service
4. No meeting service owns calendar or identity data

---

## Architectural guarantees

This domain guarantees that:
- No collaboration tool becomes a system of record for identity
- Components can be replaced independently
- Open protocols define integration boundaries
- Growth does not require architectural redesign

The Collaboration & Productivity domain remains human-scale, understandable and resilient to vendor or tooling changes.
