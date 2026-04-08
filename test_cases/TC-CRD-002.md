# TC-CRD-002: Create Subscription CRD with group-to-model relationships

**Priority**: P0
**Objective**: Verify that a Subscription CRD can be created defining group-to-model relationships with per-model quotas

**Test Steps**:
1. Ensure a Model CRD exists in the cluster (e.g., granite-3b-code)
2. Create a Subscription CRD YAML defining a group mapping to one or more models with per-model quotas
3. Apply the Subscription CRD to the cluster
4. Verify the CRD is accepted and the resource is created
5. Retrieve the Subscription resource and confirm group mappings and per-model quotas are correct
6. Verify the Subscription resource appears in `kubectl get subscriptions`

**Expected Results**:
- Subscription CRD is accepted with valid group-to-model relationships
- Per-model quotas (guaranteed and burst) are persisted correctly
- Group references resolve to valid groups in the cluster

**Test Data**:
```yaml
apiVersion: maas.opendatahub.io/v1alpha1
kind: Subscription
metadata:
  name: analytics-team-sub
  namespace: maas-subscriptions
spec:
  groups:
    - analytics-team
  models:
    - modelRef: granite-3b-code
      guaranteedTokensPerMinute: 10000
      burstTokensPerMinute: 15000
      guaranteedRequestsPerMinute: 20
      burstRequestsPerMinute: 30
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
