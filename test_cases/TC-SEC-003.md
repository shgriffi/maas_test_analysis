# TC-SEC-003: Namespace isolation prevents cross-subscription access

**Priority**: P0
**Objective**: Verify that users in one subscription cannot access models, API keys, or resources belonging to another subscription's namespace

**Preconditions**:
- Two subscriptions (Subscription-A and Subscription-B) in separate namespaces
- User-A belongs to Subscription-A only
- User-B belongs to Subscription-B only

**Test Steps**:
1. Authenticate as User-A and access a model subscribed under Subscription-A — verify success
2. As User-A, attempt to access a model subscribed only under Subscription-B — verify denied
3. As User-A, attempt to list API keys in Subscription-B's namespace — verify denied
4. As User-A, attempt to read Subscription-B's CRD — verify denied
5. Authenticate as User-B and repeat equivalent cross-subscription access attempts
6. Verify all cross-subscription access attempts are denied with HTTP 403

**Expected Results**:
- Users can only access resources within their own subscription's scope
- Cross-subscription model access returns HTTP 403
- Cross-namespace resource listing is denied
- RBAC enforcement prevents any privilege escalation across subscription boundaries

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
