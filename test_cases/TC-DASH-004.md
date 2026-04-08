# TC-DASH-004: Filter dashboard by time range

**Priority**: P1
**Objective**: Verify that the admin dashboard supports time range filtering with all documented presets

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- Usage data spanning at least 1 month

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Select "1h" time range and verify data is scoped to the last hour
3. Select "24h" time range and verify data is scoped to the last 24 hours
4. Select "3d" time range and verify data is scoped to the last 3 days
5. Select "7d" time range and verify data is scoped to the last 7 days
6. Select "1 month" time range and verify data is scoped to the last month
7. For each time range, verify that token consumption and request count totals are consistent (shorter ranges show subsets of longer ranges)

**Expected Results**:
- All 5 time range presets (1h, 24h, 3d, 7d, 1 month) are available
- Data is correctly scoped to the selected time range
- Totals decrease as the time range narrows (shorter windows show less data)
- Time range filter combines correctly with subscription and model filters

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
