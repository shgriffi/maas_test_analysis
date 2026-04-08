# TC-GITOPS-003: Subscription CRD update via GitOps push

**Priority**: P1
**Objective**: Verify that updating a Subscription CRD via a Git push triggers correct reconciliation of downstream Kuadrant policies

**Preconditions**:
- Existing Subscription CRD deployed via Argo CD
- Backend controller has reconciled the Subscription to RateLimitPolicy/AuthPolicy

**Test Steps**:
1. Verify the current RateLimitPolicy reflects the Subscription's quota values
2. Update the Subscription CRD YAML in Git (increase guaranteed tokens per minute)
3. Trigger or wait for Argo CD auto-sync
4. Verify the Subscription CRD is updated on the cluster
5. Wait for the backend controller to reconcile (up to 30 seconds)
6. Verify the RateLimitPolicy is updated with the new quota values
7. Send inference requests to confirm the new rate limits are enforced

**Expected Results**:
- GitOps push updates the Subscription CRD on the cluster
- Backend controller detects the change and updates the RateLimitPolicy
- New rate limits take effect for inference requests
- End-to-end propagation (Git push → CRD update → policy update → enforcement) completes within 2 minutes

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
