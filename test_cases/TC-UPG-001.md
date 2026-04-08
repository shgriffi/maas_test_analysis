# TC-UPG-001: RHOAI 3.0 to 3.4 upgrade with ConfigMap to CRD migration

**Priority**: P0
**Objective**: Verify that an in-place upgrade from RHOAI 3.0 to 3.4 completes successfully with ConfigMap-to-CRD migration

**Preconditions**:
- RHOAI 3.0 cluster with active MaaS ConfigMap-based configuration
- Models deployed and subscriptions active under the 3.0 format
- Pre-upgrade baseline metrics captured

**Test Steps**:
1. Document the current 3.0 ConfigMap configuration (models, tiers, quotas, groups)
2. Initiate the RHOAI 3.0 → 3.4 upgrade process
3. Verify the upgrade process completes without errors
4. Verify all RHOAI 3.4 operators are running and healthy
5. Run the ConfigMap → CRD migration script
6. Verify Model, Subscription, and APIKey CRDs are created
7. Compare every field between the original 3.0 ConfigMap and the generated 3.4 CRDs
8. Verify 100% fidelity — no data loss or transformation errors
9. Verify models are accessible through the MaaS gateway post-upgrade

**Expected Results**:
- Upgrade from 3.0 to 3.4 completes without manual intervention
- Migration script converts all ConfigMap data to CRDs with 100% fidelity
- Post-upgrade MaaS gateway serves models correctly
- All pre-existing subscription quotas and group mappings are preserved

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
