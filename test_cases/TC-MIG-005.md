# TC-MIG-005: Migration rollback on failure reverts partial changes

**Priority**: P1
**Objective**: Verify that the migration script automatically rolls back any partially created CRDs when it encounters an error mid-migration

**Preconditions**:
- Legacy ConfigMap JSON with multiple models and tiers
- Ability to simulate a failure during migration (e.g., by creating a conflicting resource)

**Test Steps**:
1. Create a ConfigMap with 3 models and 3 tiers
2. Pre-create a conflicting Model CRD with the same name as the second model in the ConfigMap to trigger a failure
3. Run the migration script
4. Verify the script encounters an error when trying to create the conflicting Model CRD
5. Verify the script rolls back the first Model CRD that was successfully created
6. Verify no Subscription CRDs from this migration run remain in the cluster
7. Verify the original ConfigMap is unchanged

**Expected Results**:
- Migration script detects the conflict and halts
- All CRDs created before the failure are rolled back (deleted)
- The cluster state is restored to pre-migration condition
- Error output clearly identifies what failed and what was rolled back
- Original ConfigMap remains intact and unmodified

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
