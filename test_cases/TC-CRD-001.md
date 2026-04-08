# TC-CRD-001: Create Model CRD with capacity quotas

**Priority**: P0
**Objective**: Verify that a Model CRD can be created with capacity quota definitions and is accepted by the cluster

**Test Steps**:
1. Create a Model CRD YAML defining a model with capacity quotas (e.g., 1000 tokens/min total capacity)
2. Apply the Model CRD to the cluster using `kubectl apply`
3. Verify the CRD is accepted and the resource is created
4. Retrieve the Model resource and confirm all fields are persisted correctly
5. Verify the Model resource appears in `kubectl get models`

**Expected Results**:
- Model CRD is accepted by the cluster without validation errors
- All capacity quota fields are persisted correctly
- Model resource is retrievable via kubectl with correct field values

**Test Data**:
```yaml
apiVersion: maas.opendatahub.io/v1alpha1
kind: Model
metadata:
  name: granite-3b-code
  namespace: maas-models
spec:
  displayName: "Granite 3B Code"
  capacity:
    totalTokensPerMinute: 50000
    totalRequestsPerMinute: 100
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
