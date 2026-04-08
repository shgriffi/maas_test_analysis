# TC-PERF-001: Dashboard loads under 3 seconds with 200 models and 500 subscriptions

**Priority**: P1
**Objective**: Verify that the admin usage dashboard loads within the 3-second performance target with the maximum expected data scale

**Preconditions**:
- OpenShift cluster with 200 Model CRDs and 500 Subscription CRDs deployed
- Metrics data generated for all models and subscriptions
- Thanos Querier configured with data retention

**Test Steps**:
1. Deploy 200 Model CRDs and 500 Subscription CRDs on the cluster
2. Generate metrics data simulating usage across all subscriptions and models
3. Log into the RHOAI Dashboard as an admin
4. Navigate to the admin usage dashboard
5. Measure the time from navigation click to full dashboard render (all charts, tables, and stat cards visible)
6. Repeat the measurement 5 times and calculate the average
7. Test with all filters (subscription, model, time range) applied

**Expected Results**:
- Average dashboard load time is under 3 seconds
- No load attempt exceeds 5 seconds
- Filtering operations complete within 1 second
- Dashboard remains responsive during data loading (no UI freezes)

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
