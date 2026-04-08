# TC-UPG-002: RHOAI 3.3 to 3.4 upgrade with ConfigMap to CRD migration

**Priority**: P0
**Objective**: Verify that an in-place upgrade from RHOAI 3.3 to 3.4 completes successfully with ConfigMap-to-CRD migration

**Preconditions**:
- RHOAI 3.3 cluster with active MaaS ConfigMap-based configuration
- More complex configuration than 3.0 (multiple models, tiers, API keys via service accounts)

**Test Steps**:
1. Document the current 3.3 ConfigMap configuration including any 3.3-specific features
2. Initiate the RHOAI 3.3 → 3.4 upgrade process
3. Verify the upgrade process completes without errors
4. Run the ConfigMap → CRD migration script
5. Verify all CRDs are created with correct values
6. Verify that 3.3-specific configuration elements are correctly mapped to 3.4 CRD fields
7. Verify the backend controller reconciles the new CRDs to Kuadrant policies
8. Send inference requests through the gateway and verify functionality

**Expected Results**:
- Upgrade from 3.3 to 3.4 completes without errors
- Migration handles 3.3-specific configuration correctly
- All subscription quotas, group mappings, and model configurations are preserved
- Post-upgrade functionality is equivalent to or better than pre-upgrade

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
