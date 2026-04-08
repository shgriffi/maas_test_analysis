# TC-MIG-004: Migration dry-run mode previews changes without applying

**Priority**: P1
**Objective**: Verify that the migration script's dry-run mode shows what CRDs would be created without actually applying them to the cluster

**Preconditions**:
- Legacy ConfigMap JSON exists in the cluster
- Migration script supports a dry-run flag

**Test Steps**:
1. Run the migration script with the dry-run flag enabled
2. Verify the script outputs a preview of the CRDs that would be created
3. Verify the preview includes Model, Subscription, and APIKey CRDs with populated field values
4. Verify no new CRD resources were actually created in the cluster
5. Run `kubectl get models,subscriptions,apikeys` to confirm no resources exist
6. Run the migration without dry-run and verify resources are created matching the preview

**Expected Results**:
- Dry-run mode produces human-readable YAML output of all CRDs that would be created
- No resources are created in the cluster during dry-run
- Actual migration output matches the dry-run preview exactly

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
