# TC-UPG-003: Service continuity during upgrade

**Priority**: P0
**Objective**: Verify that active inference requests continue being served during the RHOAI upgrade process

**Preconditions**:
- RHOAI 3.0 or 3.3 cluster with active inference traffic
- Load generation tool producing continuous inference requests

**Test Steps**:
1. Start continuous inference traffic against the MaaS gateway (e.g., 1 request/second)
2. Record the baseline success rate and response times
3. Initiate the RHOAI upgrade to 3.4
4. Continue monitoring inference requests throughout the upgrade process
5. Record any failed requests, increased latency, or error responses during the upgrade
6. After the upgrade completes, verify the success rate returns to baseline
7. Calculate the total downtime or error window during the upgrade

**Expected Results**:
- Inference requests continue being served during the upgrade (zero or near-zero downtime)
- Any service disruption is limited to a brief window (documented maintenance window)
- No requests result in data corruption or inconsistent state
- Post-upgrade success rate and latency match pre-upgrade baseline

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
