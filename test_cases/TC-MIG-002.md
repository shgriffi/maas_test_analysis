# TC-MIG-002: Migrate complex multi-model multi-tenant ConfigMap to CRD

**Priority**: P0
**Objective**: Verify that the migration script handles complex multi-model, multi-tenant ConfigMap configurations with full fidelity

**Preconditions**:
- Legacy ConfigMap JSON with multiple models, multiple tiers, and overlapping group assignments
- Migration script installed and accessible

**Test Steps**:
1. Create a ConfigMap with 3 models, 4 tiers, and groups that appear in multiple tiers
2. Run the migration script targeting the ConfigMap
3. Verify the script completes without errors
4. Verify 3 Model CRDs were created with correct capacity values
5. Verify 4 Subscription CRDs were created with correct group-to-model mappings
6. Verify per-model quotas in each Subscription match the original tier definitions
7. Verify that quota sums across subscriptions do not exceed model capacity

**Expected Results**:
- All 3 Model CRDs are created with matching capacity quotas
- All 4 Subscription CRDs are created with matching group mappings and quotas
- Groups appearing in multiple tiers are correctly represented in multiple Subscriptions
- 100% fidelity confirmed — no values altered, dropped, or duplicated

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
