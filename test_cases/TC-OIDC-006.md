# TC-OIDC-006: Namespace isolation for BYOIDC users across tenants

**Priority**: P1
**Objective**: Verify that BYOIDC users from different customer tenants cannot access each other's resources or namespaces

**Preconditions**:
- Two separate OIDC-authenticated users from different organizations (Tenant A and Tenant B)
- Each tenant has its own subscription and namespace
- Both users have valid OIDC credentials

**Test Steps**:
1. Authenticate as Tenant A user and verify access to Tenant A's subscribed models
2. Attempt to access Tenant B's models or namespace resources as Tenant A user
3. Verify the cross-tenant access is denied
4. Authenticate as Tenant B user and verify access to Tenant B's subscribed models
5. Attempt to list or access Tenant A's API keys, subscriptions, or models as Tenant B user
6. Verify the cross-tenant access is denied
7. Verify neither user can list or modify resources in the other tenant's namespace

**Expected Results**:
- Each tenant user can only access their own subscribed models
- Cross-tenant model access is denied with HTTP 403
- Cross-tenant namespace resource access (API keys, subscriptions) is denied
- Namespace isolation is enforced via K8s RBAC mapped from OIDC claims

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
