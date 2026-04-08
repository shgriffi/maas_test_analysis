# TC-OIDC-002: Subscription assignment via OIDC group claims

**Priority**: P0
**Objective**: Verify that OIDC group claims from external identity providers are used to assign users to the correct MaaS subscriptions

**Preconditions**:
- External OIDC provider configured with group claims (e.g., Azure AD groups)
- Subscription CRDs exist referencing OIDC group names
- Test user belongs to "data-science-team" group in the OIDC provider

**Test Steps**:
1. Create a Subscription CRD mapping group "data-science-team" to model "granite-3b-code" with specific quotas
2. Obtain a JWT token for the test user whose group claims include "data-science-team"
3. Send an inference request to granite-3b-code using the JWT token
4. Verify the request succeeds and the user is authorized under the "data-science-team" subscription
5. Obtain a JWT token for a different user NOT in "data-science-team"
6. Send an inference request to granite-3b-code using this token
7. Verify the request is denied due to no matching subscription

**Expected Results**:
- User with matching OIDC group claim is authorized and gets access under the correct subscription
- User without matching OIDC group claim is denied access
- Quota enforcement applies based on the matched subscription's limits

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
