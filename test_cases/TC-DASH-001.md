# TC-DASH-001: Admin dashboard displays subscription-level usage

**Priority**: P0
**Objective**: Verify that the admin usage dashboard displays token consumption, request counts, and rate limit violations at the subscription level

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- At least one subscription with active model usage generating metrics
- Thanos Querier configured for metrics aggregation

**Test Steps**:
1. Log in to the RHOAI Dashboard as an admin user
2. Navigate to the MaaS admin usage dashboard page
3. Verify the dashboard loads and displays subscription-level data
4. Verify token consumption metrics are shown per subscription
5. Verify request count metrics are shown per subscription
6. Verify rate limit violation counts are shown per subscription
7. Generate some inference traffic and rate limit violations
8. Refresh the dashboard and verify updated metrics appear within 60 seconds

**Expected Results**:
- Dashboard is accessible to admin users from the RHOAI Dashboard page
- Token consumption is displayed per subscription with accurate values
- Request counts match the actual number of inference requests
- Rate limit violations are tracked and displayed
- Data refreshes within the 60-second SLA

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
