# TC-EGRESS-002: API key injection from labeled K8s secret

**Priority**: P0
**Objective**: Verify that the API key injection plugin automatically injects provider credentials from labeled K8s secrets into egress requests

**Preconditions**:
- External model endpoint configured via ServiceEntry
- K8s secret containing the external provider's API key, labeled for injection
- API key injection BBR plugin configured and active

**Test Steps**:
1. Create a K8s secret containing the external provider API key with the appropriate injection label
2. Verify the secret is created with correct labels
3. Send an inference request through the gateway targeting the external model (without manually adding the provider API key)
4. Verify the BBR plugin injects the provider API key from the labeled secret into the egress request
5. Verify the external provider accepts the request and returns a valid response
6. Delete the K8s secret
7. Send another inference request and verify it fails due to missing provider credentials

**Expected Results**:
- The plugin automatically injects the API key from the labeled K8s secret
- The client does not need to include the external provider's credentials
- Requests succeed when the secret exists and fail when it is removed
- The injected API key is not visible in the client response

**Test Data**:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-openai-credentials
  namespace: maas-egress
  labels:
    maas.opendatahub.io/provider-key: "true"
    maas.opendatahub.io/model: "gpt-4-azure"
type: Opaque
stringData:
  api-key: "sk-azure-proj-abc123def456ghi789"
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
