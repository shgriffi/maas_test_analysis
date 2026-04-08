# TC-OIDC-001: OIDC token translation to in-memory K8s identity

**Priority**: P0
**Objective**: Verify that a valid OIDC token from an external identity provider is translated into an in-memory Kubernetes identity allowing MaaS access

**Preconditions**:
- External OIDC provider (Azure AD or Okta) configured with the cluster
- Test user account exists in the external OIDC provider

**Test Steps**:
1. Obtain a valid JWT token from the external OIDC provider for the test user
2. Send an inference request to the MaaS gateway using the JWT as a Bearer token
3. Verify the request is authenticated and the inference response is returned
4. Check the gateway logs to confirm the OIDC token was translated to a K8s identity
5. Verify the translated K8s identity does not persist after the request completes (in-memory only)

**Expected Results**:
- OIDC JWT token is accepted by the MaaS gateway as a valid authentication credential
- The token is translated to a transient in-memory K8s identity
- The inference request succeeds with the translated identity
- No persistent K8s user or service account is created for the OIDC user

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
