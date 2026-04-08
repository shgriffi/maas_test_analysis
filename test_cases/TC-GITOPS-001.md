# TC-GITOPS-001: Declarative YAML MaaS configuration via Argo CD

**Priority**: P1
**Objective**: Verify that all MaaS CRD-based configuration can be managed declaratively via GitOps workflow using Argo CD

**Preconditions**:
- Argo CD installed and connected to a Git repository
- Git repository contains MaaS CRD YAML files (Model, Subscription, APIKey)
- Target namespace exists on the cluster

**Test Steps**:
1. Push Model, Subscription, and APIKey CRD YAML files to the Git repository
2. Create an Argo CD Application pointing to the Git repository path
3. Sync the Argo CD Application
4. Verify the CRD resources are created on the cluster matching the YAML definitions
5. Verify the backend controller reconciles the Subscription to RateLimitPolicy/AuthPolicy
6. Modify a Subscription CRD in Git (change quota values)
7. Sync again and verify the cluster resources are updated

**Expected Results**:
- Argo CD successfully syncs all MaaS CRD resources from Git
- CRD validation webhooks accept the declarative configurations
- Backend controller reconciles CRDs applied via GitOps the same as manual `kubectl apply`
- Updates in Git are reflected on the cluster after sync

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
