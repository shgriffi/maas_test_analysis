# TC-OIDC-004: BYOIDC user creates API key

**Priority**: P1
**Objective**: Verify that a user authenticated via external OIDC can create and use MaaS API keys

**Preconditions**:
- External OIDC provider configured with the cluster
- BYOIDC user has an active subscription
- API key self-service is enabled

**Test Steps**:
1. Authenticate as the BYOIDC user using OIDC credentials
2. Create a new API key through the API key self-service endpoint
3. Verify the plaintext key is returned in the creation response (show-once)
4. Store the plaintext key
5. Send an inference request using the newly created API key as a Bearer token
6. Verify the request succeeds and is authorized under the correct subscription
7. Verify the API key inherits the BYOIDC user's subscription and quota limits

**Expected Results**:
- BYOIDC users can create API keys without requiring OpenShift accounts
- The created API key authenticates successfully at the gateway
- Inference requests using the API key are subject to the same subscription quotas as the OIDC user
- The API key appears in the user's key list

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
