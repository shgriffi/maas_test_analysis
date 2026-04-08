# TC-DASH-006: Opt-in per-user metrics flag (default off)

**Priority**: P2
**Objective**: Verify that the opt-in per-user metrics flag enables per-user breakdown within subscriptions and is disabled by default

**Preconditions**:
- Admin user logged into the RHOAI Dashboard
- Multiple users generating traffic under the same subscription

**Test Steps**:
1. Navigate to the admin usage dashboard
2. Verify per-user breakdown is NOT displayed by default
3. Verify there is no per-user data visible in subscription-level views
4. Enable the opt-in per-user metrics flag
5. Verify a cardinality warning is displayed when enabling
6. Verify per-user breakdown data appears in the dashboard
7. Verify individual user token consumption and request counts are shown within each subscription
8. Disable the per-user metrics flag and verify the breakdown disappears

**Expected Results**:
- Per-user metrics are off by default (TP feature)
- Enabling the flag displays a cardinality warning
- Per-user breakdown shows individual user consumption within subscriptions
- Disabling the flag removes per-user data from the display

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
