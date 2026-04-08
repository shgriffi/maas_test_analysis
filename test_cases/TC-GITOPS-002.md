# TC-GITOPS-002: CRD validation webhook works during GitOps reconciliation

**Priority**: P1
**Objective**: Verify that CRD validation webhooks correctly reject invalid configurations during GitOps reconciliation cycles

**Preconditions**:
- Argo CD installed and connected to a Git repository
- CRD validation webhooks are active on the cluster

**Test Steps**:
1. Push a valid Subscription CRD YAML to the Git repository
2. Sync via Argo CD and verify it is applied successfully
3. Push an invalid Subscription CRD YAML (quota sum exceeds model capacity) to the Git repository
4. Sync via Argo CD
5. Verify the sync reports an error from the validation webhook
6. Verify the Argo CD Application enters a "Degraded" or "OutOfSync" state
7. Verify the error message from the webhook is visible in Argo CD's sync status
8. Fix the YAML in Git and re-sync to verify recovery

**Expected Results**:
- Validation webhooks fire during Argo CD sync operations
- Invalid configurations are rejected with descriptive error messages
- Argo CD correctly surfaces the webhook error in its UI
- Fixing the configuration and re-syncing resolves the error

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
