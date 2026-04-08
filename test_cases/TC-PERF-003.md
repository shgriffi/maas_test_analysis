# TC-PERF-003: CSV export handles monthly billing cycle data

**Priority**: P1
**Objective**: Verify that CSV export completes successfully for a full monthly billing cycle without timeout or data loss

**Preconditions**:
- Usage data accumulated over at least 1 month
- Multiple subscriptions and models with continuous traffic
- Data volume representative of production billing cycle

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Select "1 month" time range
3. Do not apply any subscription or model filters (export all data)
4. Click CSV export
5. Measure the time to complete the export
6. Verify the download completes without timeout
7. Verify the CSV file size is reasonable (not truncated)
8. Verify the row count matches the expected number of data points for the period
9. Verify the first and last rows span the full monthly date range

**Expected Results**:
- CSV export for a full month completes within a reasonable time (under 30 seconds)
- The file is not truncated or incomplete
- All data points for the period are included
- The export does not cause memory issues or dashboard crashes

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
