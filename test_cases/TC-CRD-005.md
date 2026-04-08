# TC-CRD-005: Backend controller reconciles Subscription CRD to Kuadrant RateLimitPolicy

**Priority**: P0
**Objective**: Verify that the backend controller creates or updates a Kuadrant RateLimitPolicy when a Subscription CRD is created or modified

**Preconditions**:
- Kuadrant operator is installed and running
- Model CRD exists for the referenced model

**Test Steps**:
1. Create a Subscription CRD with per-model quota definitions
2. Wait for the backend controller to reconcile (up to 30 seconds)
3. List RateLimitPolicy resources in the namespace
4. Verify a RateLimitPolicy was created corresponding to the Subscription's quota definitions
5. Update the Subscription CRD to change the guaranteed tokens per minute
6. Wait for reconciliation
7. Verify the RateLimitPolicy was updated to reflect the new quota values

**Expected Results**:
- A RateLimitPolicy resource is automatically created matching the Subscription's quota values
- Updating the Subscription CRD triggers an update to the corresponding RateLimitPolicy
- Rate limit values in the policy match the per-model quotas defined in the Subscription

**Validation**:
- `kubectl get ratelimitpolicy -n maas-subscriptions -o yaml` shows matching quota values

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
