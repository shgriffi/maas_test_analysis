# TC-UPG-004: Rollback procedure from 3.4 to 3.3

**Priority**: P1
**Objective**: Verify that a documented rollback procedure from RHOAI 3.4 to 3.3 restores the previous working state

**Preconditions**:
- RHOAI 3.4 cluster that was upgraded from 3.3
- Rollback procedure documentation available
- Pre-upgrade backup of 3.3 configuration

**Test Steps**:
1. Document the current 3.4 state (CRDs, policies, gateway configuration)
2. Follow the documented rollback procedure to revert from 3.4 to 3.3
3. Verify the rollback process completes without errors
4. Verify RHOAI 3.3 operators are running and healthy
5. Verify the pre-upgrade ConfigMap-based configuration is restored
6. Verify models are accessible through the MaaS gateway
7. Verify subscription quotas and group mappings match the pre-upgrade state
8. Send inference requests to verify end-to-end functionality

**Expected Results**:
- Rollback procedure is documented and reproducible
- 3.3 configuration is fully restored after rollback
- Model serving resumes normally under 3.3
- No data loss occurs during the rollback process

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
