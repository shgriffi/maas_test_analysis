# TC-DASH-002: Filter dashboard by subscription

**Priority**: P1
**Objective**: Verify that the admin dashboard can be filtered to show usage for a specific subscription

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- At least 3 subscriptions with active usage data

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Verify all subscriptions are shown by default
3. Select a specific subscription from the subscription filter
4. Verify only data for the selected subscription is displayed
5. Verify token consumption, request counts, and rate limit violations are scoped to the selected subscription
6. Clear the filter and verify all subscriptions reappear
7. Select a different subscription and verify the data updates correctly

**Expected Results**:
- Subscription filter correctly scopes all dashboard data
- Filtering is responsive and updates the display without full page reload
- Clearing the filter restores the full view
- Data accuracy is maintained when switching between filters

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
