# TC-CRD-003: Create APIKey CRD

**Priority**: P0
**Objective**: Verify that an APIKey CRD can be created with proper validation and metadata fields

**Test Steps**:
1. Ensure a Subscription CRD exists in the cluster
2. Create an APIKey CRD YAML associated with a user and subscription
3. Apply the APIKey CRD to the cluster
4. Verify the CRD is accepted and the resource is created
5. Retrieve the APIKey resource and confirm all metadata fields are persisted
6. Verify the APIKey resource appears in `kubectl get apikeys`

**Expected Results**:
- APIKey CRD is accepted with valid references to user and subscription
- API key metadata (name, creation timestamp, expiration if set) is persisted correctly
- The key hash field is populated (plaintext key is not stored in the CRD)

**Test Data**:
```yaml
apiVersion: maas.opendatahub.io/v1alpha1
kind: APIKey
metadata:
  name: jsmith-analytics-key-01
  namespace: maas-keys
spec:
  owner: jsmith
  subscriptionRef: analytics-team-sub
  displayName: "Dev environment key"
  expiresAt: "2027-01-01T00:00:00Z"
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
