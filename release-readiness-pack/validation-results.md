# Configuration Verification

## Target Environment

The documented Checkout API architecture requires Python 3.11, Flask 3.0.2, Gunicorn 22.0.0, PostgreSQL, and Redis.

| Variable | Source | Required | Current Status |
|---|---|---:|---|
| `PORT` | ConfigMap | Yes | Configured |
| `LOG_LEVEL` | ConfigMap | Yes | Configured |
| `DATABASE_URL` | Production Secret/configuration | Yes | Missing from repository manifest |
| `REDIS_URL` | Production Secret/configuration | Yes | Missing from repository manifest |

## Application Configuration

`app/app.py` reads:

```text
PORT
LOG_LEVEL
DATABASE_URL
REDIS_URL
```

When `DATABASE_URL` is absent, the application reports `warning_fallback_sqlite`.

When `REDIS_URL` is absent, the application reports `warning_fallback_local`.

These fallbacks are not suitable as production dependency verification.

## Kubernetes Configuration

`k8s/configmap.yaml` currently contains:

```yaml
LOG_LEVEL: "info"
PORT: "5000"
```

It does not provide `DATABASE_URL` or `REDIS_URL`.

Production credentials must not be committed to Git. They should be supplied through the approved production Secret/configuration mechanism.

## Verification Checklist

- [x] Python runtime matches documented requirement: 3.11
- [x] `PORT` configured
- [x] `LOG_LEVEL` configured
- [ ] Production `DATABASE_URL` supplied securely
- [ ] Production `REDIS_URL` supplied securely
- [ ] Database connectivity verified
- [ ] Redis connectivity verified
- [ ] `/health` confirms both production dependencies are connected

## Required Action Before Production

Before a Go decision:

1. Supply `DATABASE_URL` through the approved production configuration mechanism.
2. Supply `REDIS_URL` through the approved production configuration mechanism.
3. Verify both without exposing secret values.
4. Confirm `/health` reports connected dependencies.
5. Record the verification evidence.
