# TrueNorth
This repository is a personal, non-commercial exploration and documentation project.
“Nidax” is used solely as a personal namespace to group ideas and projects, not as a company or commercial entity.
It is not a product, consultancy or active business offering and is maintained alongside regular employment.

The name "True North" is used as a project name and concept. 
Use of the name does not imply permission for branding or commercial use.
It is an open human-scale framework for secure digital collaboration.
It focuses on strong identity, clear boundaries and vendor-neutral architecture.
So organizations can run their core digital services with confidence and autonomy.

This repository is the home of the **True North mission, vision, principles, and proofs of concept**.
It is intentionally lightweight and iterative.

## Mission and principles

- Mission: [docs/mission.md](./docs/mission.md)
- Vision: [docs/vision.md](./docs/vision.md)
- Open standards: [docs/open-standards.md](./docs/open-standards.md)
- Portfolio & evidence index: [docs/portfolio.md](./docs/portfolio.md)

## Runbooks

Operational procedures live in `docs/runbooks/`.

## Why True North

Modern organizations depend on critical digital infrastructure, yet often lose:
- control over identity and access;
- insight into how systems are connected;
- and freedom to evolve without vendor lock-in.

True North explores a different approach:
- strong identity as the foundation;
- security embedded by design;
- collaboration without opacity;
- and technology that remains understandable and auditable.

## Proofs of Concept

The `/poc` directory contains focused Proofs of Concept.
Each PoC is intentionally scoped to validate a single architectural idea not to build a full product.

### PoC overview

Each domain progresses through explicit validation stages.
These stages distinguish technical controls from operational proof.

#### Draft (theory-only)
Architecture, rationale and design assumptions.

#### Lab-validated
Functionality works in a controlled environment and is reproducible.

#### Production-validated
The domain runs in a real environment with real users or workloads.

#### Security-hardened
Security controls are explicitly implemented and reviewed
(identity boundaries, logging, threat model, least privilege).

#### Audit-ready
The system can demonstrate its security and reliability through evidence:
runbooks, restore tests, logs, documented procedures and repeatable outcomes.


| Domain | Name | Status | Link |
|---:|---|---|---|
| 1 | Identity & Access | Validated in lab | [PoC README](./poc/1.%20Identity%20%26%20Access/README.md) |
| 2 | Collaboration & Productivity | Validated in lab | [PoC README](./poc/2.%20Collaboration%20%26%20Productivity/README.md) |
| 3 | Compute & Hosting | Draft (theory-only) | [PoC README](./poc/3.%20Compute%20%26%20Hosting/README.md) |
| 4 | Security, Observability & Compliance | Draft (theory-only) | [PoC README](./poc/4.%20Security,%20Observability%20%26%20Compliance/README.md) |
| 5 | Configuration & Automation | Draft (theory-only) | [PoC README](./poc/5.%20Configuration%20%26%20Automation/README.md) |
| 6 | Integration Layer | Draft (theory-only) | [PoC README](./poc/6.%20Integration%20Layer/README.md) |
| 7 | Operations & Support | Draft (theory-only) | [PoC README](./poc/7.%20Operations%20%26%20Support/README.md) |
| 8 | Analytics & Insight | Draft (theory-only) | [PoC README](./poc/8.%20Analytics%20%26%20Insight/README.md) |

## License and intent

True North is licensed under the **GNU Affero General Public License v3 (AGPL-3.0)**.

This license is a conscious choice.
It ensures that improvements remain shared including when the software is
used to provide services over a network. Commercial use is allowed but
closed and/or extractive use is not the goal of this project.

True North exists to create shared value.
Any commercial activity should respect that spirit.

## Status

This project is under active exploration.
Expect clarity before speed and structure before scale.

2026 Nidax / True North
