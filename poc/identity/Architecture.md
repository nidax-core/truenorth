# Architecture

This Proof of Concept uses a simple hub-and-spoke model.

IdentityGuard acts as the central authority for authentication
and authorization. All services delegate identity and access
decisions to this central layer.

The architecture is intentionally minimal to highlight trust
boundaries, dependency direction and audit flows.

A visual diagram will be added once the PoC implementation begins.
