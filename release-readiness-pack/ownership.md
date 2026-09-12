# Rollback Plan

## Release Candidate

Current candidate:

```text
kalvium/checkout-api:v2.4
```

## Known-Good Version

According to `docs/architecture.md`, v2.3 is the current stable production release.

**Rollback target: `v2.3`**

## Rollback Triggers

Rollback should be considered if:

- Health checks fail.
- Database connectivity is unavailable.
- Redis connectivity is unavailable.
- Checkout errors increase unexpectedly.
- Latency becomes unacceptable.
- Pods repeatedly restart.
- The release causes a customer-impacting regression.

## Rollback Procedure

### 1. Check the current deployment

```bash
kubectl get deployment checkout-api -n checkout-system
kubectl get pods -n checkout-system
```

### 2. Change the deployment to v2.3

```bash
kubectl set image deployment/checkout-api checkout-api=kalvium/checkout-api:v2.3 -n checkout-system
```

### 3. Monitor the rollout

```bash
kubectl rollout status deployment/checkout-api -n checkout-system
```

### 4. Verify pods

```bash
kubectl get pods -n checkout-system
```

### 5. Verify the service

```bash
kubectl get service checkout-api-service -n checkout-system
```

### 6. Verify application health

Use the appropriate in-cluster or externally exposed endpoint:

```bash
curl http://<SERVICE-ENDPOINT>/health
```

## Alternative Kubernetes Rollback

Review deployment history:

```bash
kubectl rollout history deployment/checkout-api -n checkout-system
```

If the desired previous revision is confirmed:

```bash
kubectl rollout undo deployment/checkout-api -n checkout-system
```

Then verify:

```bash
kubectl rollout status deployment/checkout-api -n checkout-system
```

## Post-Rollback Verification

- [ ] Pods are Running
- [ ] Deployment rollout completes
- [ ] `/health` returns HTTP 200
- [ ] Database reports connected
- [ ] Redis reports connected
- [ ] Error rate returns to baseline
- [ ] No restart loop occurs

## Rollback Test Evidence

**Environment:** PASTE ACTUAL ENVIRONMENT USED

**Commands/output:** PASTE ACTUAL TEST EVIDENCE HERE

**Result:** PASS / NOT EXECUTED

Do not claim rollback was tested unless it was actually executed in a Kubernetes environment.
