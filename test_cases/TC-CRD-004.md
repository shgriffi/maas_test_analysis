# TC-CRD-004: Validation webhook rejects quota sum exceeding model capacity

**Priority**: P0
**Objective**: Verify that the validation webhook rejects Subscription CRDs where the sum of guaranteed quotas exceeds the model's total capacity

**Preconditions**:
- Model CRD "granite-3b-code" exists with totalTokensPerMinute: 50000
- Subscription "team-alpha-sub" already consumes 30000 guaranteed tokens/min for this model

**Test Steps**:
1. Create a new Subscription CRD requesting 25000 guaranteed tokens/min for granite-3b-code (would total 55000, exceeding 50000 capacity)
2. Apply the Subscription CRD to the cluster
3. Verify the webhook rejects the configuration with a descriptive error message
4. Create a valid Subscription CRD requesting 15000 guaranteed tokens/min (would total 45000, within capacity)
5. Apply the valid Subscription and verify it is accepted

**Expected Results**:
- The over-capacity Subscription is rejected with an error indicating the sum of guaranteed quotas (55000) exceeds model capacity (50000)
- The error message clearly identifies which model is over-allocated
- The valid Subscription (within capacity) is accepted successfully

**Test Data**:
```yaml
# Should be REJECTED
apiVersion: maas.opendatahub.io/v1alpha1
kind: Subscription
metadata:
  name: team-beta-sub-invalid
  namespace: maas-subscriptions
spec:
  groups:
    - team-beta
  models:
    - modelRef: granite-3b-code
      guaranteedTokensPerMinute: 25000
      burstTokensPerMinute: 30000
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
