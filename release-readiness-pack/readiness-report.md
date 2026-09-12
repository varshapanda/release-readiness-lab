# Deployment Ownership

## Release Candidate

**Service:** Checkout API  
**Version:** v2.4

| Role | Owner | Responsibility |
|---|---|---|
| Release Owner | Release Engineering | Release coordination and readiness decision |
| SRE / Operational Lead | SRE / Platform Team | Production health, configuration, and rollback |
| On-Call Support | Production On-Call Engineer | Monitor deployment and respond to incidents |
| Application Owner | Checkout API Development Team | Application correctness and defects |
| Escalation | Engineering Lead | Decision support for unresolved production risks |

## Release Owner

Responsibilities:

- Confirm readiness evidence is complete.
- Confirm release gates are satisfied.
- Coordinate deployment.
- Confirm rollback target.
- Record the Go/No-Go decision.

## SRE / Operational Lead

Responsibilities:

- Verify Kubernetes configuration.
- Verify database and Redis connectivity.
- Monitor rollout.
- Validate health checks.
- Execute rollback if required.

## On-Call Support

Responsibilities:

- Monitor alerts and application health.
- Investigate production incidents.
- Escalate customer-impacting failures.
- Support rollback.

## Application Owner

Responsibilities:

- Investigate application failures.
- Validate expected API behaviour.
- Resolve application defects.

## Escalation Path

```text
On-Call Engineer
       |
       v
SRE / Operational Lead
       |
       v
Release Owner
       |
       v
Engineering Lead
```

## Deployment Window

Record these details before production release:

```text
Deployment Window: __________________
Release Owner: ______________________
On-Call Engineer: ____________________
SRE Lead: ____________________________
```
