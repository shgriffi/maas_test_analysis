# TC-DASH-005: CSV export for finance team cost attribution

**Priority**: P1
**Objective**: Verify that the admin dashboard supports CSV export of usage data for finance team cost attribution

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- Usage data available for at least one full billing period
- Subscription and model filters applied

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Apply filters (subscription, model, time range) to scope the export
3. Click the CSV export button
4. Verify the CSV file is downloaded successfully
5. Open the CSV and verify it contains columns for: subscription, model, token consumption, request counts, rate limit violations, time period
6. Verify the data in the CSV matches the dashboard display
7. Export a full monthly billing cycle and verify the file is complete (not truncated)

**Expected Results**:
- CSV export produces a valid CSV file
- CSV columns include all relevant usage metrics
- Data matches the dashboard display for the selected filters
- Monthly billing cycle export completes without timeout or truncation
- File encoding is UTF-8 and compatible with standard spreadsheet applications

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
