# TC-UPG-005: Data integrity verification post-upgrade

**Priority**: P0
**Objective**: Verify that subscription quotas, API keys, and telemetry data maintain integrity after upgrade from 3.0/3.3 to 3.4

**Preconditions**:
- Completed upgrade from 3.0 or 3.3 to 3.4
- Pre-upgrade snapshot of all subscription quotas, API keys (service account tokens), and recent telemetry metrics

**Test Steps**:
1. Compare pre-upgrade subscription quota values with post-upgrade Subscription CRD quota values
2. Verify all quota types (guaranteed, burst, tokens/min, requests/min) match exactly
3. Verify all group-to-model mappings are preserved
4. Verify existing service account tokens still authenticate at the gateway (backward compatibility)
5. Verify telemetry metrics continuity — no gap in metrics timeline around the upgrade window
6. Verify rate limit violation counts are preserved or clearly reset at upgrade boundary
7. Run the full test suite against the upgraded cluster to verify no regression

**Expected Results**:
- All subscription quotas match pre-upgrade values with 100% fidelity
- Group mappings are identical pre and post upgrade
- Existing authentication tokens continue working
- Telemetry metrics show continuity without unexplained gaps
- No data corruption or silent data loss

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
