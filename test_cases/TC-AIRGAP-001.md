# TC-AIRGAP-001: API key lifecycle in air-gapped cluster

**Priority**: P0
**Objective**: Verify that the full API key lifecycle (create, list, get, revoke) works in a fully disconnected/air-gapped cluster without external dependencies

**Preconditions**:
- Air-gapped OpenShift cluster with RHOAI 3.4 installed from mirrored registries
- No external network connectivity
- MaaS gateway and API key self-service deployed

**Test Steps**:
1. Verify the cluster has no external network connectivity (ping external hosts fails)
2. Create a new API key through the self-service endpoint
3. Verify the plaintext key is returned and the key is stored (show-once pattern)
4. List the user's API keys and verify the new key appears
5. Use the API key to authenticate an inference request to a local model
6. Verify the inference request succeeds
7. Revoke the API key
8. Verify the revoked key is immediately rejected at the gateway
9. Create another key with a custom expiration — verify it works

**Expected Results**:
- All API key operations work without external connectivity
- No OIDC provider or external metering system is required for basic API key management
- Local model inference with API key authentication works fully offline
- Key storage backend operates without external database connectivity (or uses a local database)

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
