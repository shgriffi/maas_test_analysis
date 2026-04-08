# TC-DASH-003: Filter dashboard by model

**Priority**: P1
**Objective**: Verify that the admin dashboard can be filtered to show usage for a specific model

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- At least 2 models with active usage data across multiple subscriptions

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Select a specific model from the model filter
3. Verify only usage data for the selected model is displayed
4. Verify subscription-level data is scoped to show only usage of the selected model
5. Select a different model and verify the data updates
6. Apply both a model filter and a subscription filter simultaneously
7. Verify the combined filter shows data for the specific model within the specific subscription

**Expected Results**:
- Model filter correctly scopes all dashboard data to the selected model
- Combined model + subscription filtering works correctly
- Data is consistent with individual filter results

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
