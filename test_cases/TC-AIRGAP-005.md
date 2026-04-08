# TC-AIRGAP-005: Local model serving with full governance in disconnected cluster

**Priority**: P0
**Objective**: Verify that local model serving with all MaaS governance features works in a fully air-gapped cluster

**Preconditions**:
- Air-gapped OpenShift cluster with RHOAI 3.4 from mirrored registries
- vLLM or llm-d model deployed locally
- All MaaS operators installed from disconnected operator catalog

**Test Steps**:
1. Verify the cluster has no external connectivity
2. Deploy a model using vLLM runtime and enable MaaS checkbox
3. Create a Subscription CRD with rate limiting quotas
4. Create an API key for a test user
5. Send inference requests using the API key — verify they succeed
6. Exceed the rate limit quota and verify HTTP 429 is returned
7. Verify token tracking metrics are recorded in local Prometheus
8. Verify the admin dashboard shows the subscription usage
9. Revoke the API key and verify the gateway rejects subsequent requests

**Expected Results**:
- Full MaaS governance stack works in a disconnected environment
- Rate limiting, API key authentication, and token tracking function without external connectivity
- Dashboard shows accurate local metrics
- The complete user workflow (deploy model → create subscription → create key → inference → monitor) works offline

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
