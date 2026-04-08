# TC-PERF-002: Quota status refresh within 60 seconds

**Priority**: P1
**Objective**: Verify that real-time quota status on the dashboard refreshes within the 60-second target

**Preconditions**:
- Admin dashboard loaded with active subscriptions
- Inference traffic actively generating quota usage

**Test Steps**:
1. Open the admin usage dashboard
2. Note the current quota usage values for a specific subscription
3. Generate a burst of inference requests (e.g., 50 requests) against the subscription
4. Start a timer immediately after the burst
5. Monitor the dashboard for quota usage updates
6. Record the time when the dashboard reflects the new usage data
7. Verify the refresh time is within 60 seconds

**Expected Results**:
- Quota usage data updates within 60 seconds of actual usage occurring
- The dashboard does not require manual refresh to show updated data
- Updated values accurately reflect the new token consumption and request counts

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
