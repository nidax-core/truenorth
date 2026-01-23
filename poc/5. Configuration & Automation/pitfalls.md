# Pitfalls — Configuration & Automation

This document captures real-world pitfalls encountered or anticipated in the Configuration & Automation domain.

The purpose is to prevent fragile automation and hidden coupling.

---

## Operational pitfalls

### Snowflake servers

Symptom:
Servers diverge because of manual changes and “one-off fixes”.

Mitigation:
Treat manual changes as incidents; codify the fix and re-apply.

### Works on my laptop

Symptom:
Automation behaves differently depending on where it is executed.

Mitigation:
Use a controlled runner environment and pin tool versions.

---

## Security pitfalls

### Secrets in configuration or logs

Symptom:
Secrets leak into Git history or CI logs.

Mitigation:
Strict secrets boundary, secret masking, and review discipline.

### Over-permissive automation

Symptom:
Runners can do everything everywhere, making compromise catastrophic.

Mitigation:
Least privilege, environment separation, and auditable access.

---

## Maintainability pitfalls

### Copy-paste environments

Symptom:
Every environment becomes a fork with inconsistent behavior.

Mitigation:
Shared roles/modules with environment-specific parameters.

### Undocumented manual steps

Symptom:
A deploy “usually works” but depends on tribal knowledge.

Mitigation:
Document as code and make the documented path the only supported path.

