# Risk Analysis

## Release Candidate

**Service:** Checkout API  
**Version:** v2.4

| ID | Risk | Severity | Likelihood | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | Missing `DATABASE_URL` causes volatile SQLite fallback | High | High | Configure and verify production database before deployment | SRE / Operational Lead |
| R2 | Missing `REDIS_URL` causes local in-memory cache fallback | High | High | Configure and verify production Redis before deployment | SRE / Operational Lead |
| R3 | Container image uses mutable `v2.4` tag | Medium | Medium | Pin production image to an immutable digest | Release Owner |
| R4 | Container runs without an explicit non-root user | Medium | Medium | Add and validate a non-root container security context | Platform/SRE |
| R5 | Rollback depends on v2.3 image availability | Medium | Low | Verify the known-good image is available before release | Release Owner |
| R6 | Dependency vulnerabilities may exist despite version pinning | High | Medium | Run `pip-audit` and remediate blocking vulnerabilities | Release Owner |
| R7 | Deployment ownership is not defined in the candidate repository | Medium | Medium | Assign release, SRE, on-call, and escalation roles | Engineering Lead |

## Detailed Risks

### R1 — Missing Database Configuration

**Severity:** High  
**Likelihood:** High

The application falls back to volatile SQLite when `DATABASE_URL` is absent. This is inconsistent with the documented PostgreSQL production architecture.

**Mitigation:** Configure the production database securely and verify `/health` reports the database as connected.

**Owner:** SRE / Operational Lead

### R2 — Missing Redis Configuration

**Severity:** High  
**Likelihood:** High

The application falls back to a local in-memory cache when `REDIS_URL` is absent. This is not distributed across the three replicas.

**Mitigation:** Configure Redis securely and verify `/health` reports Redis as connected.

**Owner:** SRE / Operational Lead

### R3 — Mutable Container Image Tag

**Severity:** Medium  
**Likelihood:** Medium

The deployment references `kalvium/checkout-api:v2.4`, which does not guarantee immutable image content.

**Mitigation:** Use an immutable image digest for production.

**Owner:** Release Owner

### R4 — Container Privileges

**Severity:** Medium  
**Likelihood:** Medium

The Dockerfile does not explicitly create or use a non-root user.

**Mitigation:** Add and validate a non-root runtime configuration.

**Owner:** Platform/SRE

### R5 — Rollback Image Availability

**Severity:** Medium  
**Likelihood:** Low

Rollback depends on availability of the known-good v2.3 image.

**Mitigation:** Verify that the v2.3 image exists and is pullable before production deployment.

**Owner:** Release Owner

### R6 — Dependency Vulnerabilities

**Severity:** High  
**Likelihood:** Medium

Pinning improves reproducibility but does not prove that packages are free of known vulnerabilities.

**Mitigation:** Run `pip-audit` and resolve release-blocking findings.

**Owner:** Release Owner

### R7 — Ownership Gap

**Severity:** Medium  
**Likelihood:** Medium

A deployment without explicit operational ownership can become unowned during an incident.

**Mitigation:** Assign release, operational, on-call, and escalation roles.

**Owner:** Engineering Lead

## Release Gate

The highest-severity unresolved risks are the missing production database and Redis configuration. These must be resolved before a Go decision.
