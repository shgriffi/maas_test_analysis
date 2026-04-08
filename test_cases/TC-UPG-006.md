# TC-UPG-006: K8s Service Account token backward compatibility during API key rollout

**Priority**: P0
**Objective**: Verify that existing K8s Service Account tokens continue working for gateway authentication during and after the API key feature rollout in 3.4

**Preconditions**:
- Existing service account tokens from pre-3.4 deployments
- API key self-service enabled on the 3.4 cluster

**Test Steps**:
1. Verify a pre-existing service account token authenticates at the MaaS gateway post-upgrade
2. Send an inference request using the service account token — verify it succeeds
3. Create a new API key through the self-service endpoint
4. Send an inference request using the new API key — verify it succeeds
5. Verify both authentication methods (SA token and API key) work simultaneously
6. Verify quota enforcement applies correctly regardless of authentication method
7. Verify users can gradually migrate from SA tokens to API keys without disruption

**Expected Results**:
- Service account tokens from pre-3.4 are not broken by the API key feature
- Both SA tokens and API keys work as valid authentication methods
- Quota enforcement is consistent across authentication methods
- No forced migration from SA tokens — both methods coexist

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
