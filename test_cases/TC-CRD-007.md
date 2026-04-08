# TC-CRD-007: Validation webhook rejects Subscription CRD with missing required fields

**Priority**: P0
**Objective**: Verify that the validation webhook rejects Subscription CRDs that are missing required fields

**Test Steps**:
1. Create a Subscription CRD YAML without the groups field
2. Apply it and verify the webhook rejects it with a descriptive error
3. Create a Subscription CRD YAML without any model references
4. Apply it and verify the webhook rejects it
5. Create a Subscription CRD YAML with a model reference to a non-existent model
6. Apply it and verify the webhook rejects it
7. Create a valid Subscription CRD with all required fields and verify it is accepted

**Expected Results**:
- Missing groups field produces a clear error message indicating the field is required
- Missing model references produces a clear error message
- Reference to a non-existent model produces a clear error indicating the model was not found
- All error messages are human-readable and actionable

**Test Data**:
```yaml
# Missing groups - should be REJECTED
apiVersion: maas.opendatahub.io/v1alpha1
kind: Subscription
metadata:
  name: invalid-no-groups
  namespace: maas-subscriptions
spec:
  models:
    - modelRef: granite-3b-code
      guaranteedTokensPerMinute: 5000
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
