# TC-CRD-006: Backend controller reconciles Subscription CRD to Kuadrant AuthPolicy

**Priority**: P0
**Objective**: Verify that the backend controller creates or updates a Kuadrant AuthPolicy when a Subscription CRD is created

**Preconditions**:
- Kuadrant operator and Authorino are installed and running
- Model CRD exists for the referenced model

**Test Steps**:
1. Create a Subscription CRD with group references and model access
2. Wait for the backend controller to reconcile (up to 30 seconds)
3. List AuthPolicy resources in the namespace
4. Verify an AuthPolicy was created that enforces authentication for the subscription's group-to-model access
5. Delete the Subscription CRD
6. Verify the corresponding AuthPolicy is also cleaned up

**Expected Results**:
- An AuthPolicy resource is automatically created corresponding to the Subscription
- The AuthPolicy enforces that only members of the specified groups can access the subscribed models
- Deleting the Subscription CRD removes the associated AuthPolicy

**Validation**:
- `kubectl get authpolicy -n maas-subscriptions -o yaml` shows correct group references

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
