# TC-SEC-002: Authorino gateway-layer API key validation

**Priority**: P0
**Objective**: Verify that Authorino validates API keys at the gateway layer before requests reach the model serving infrastructure

**Preconditions**:
- Authorino operator installed and configured for the MaaS gateway
- Valid and invalid API keys available

**Test Steps**:
1. Send an inference request with a valid API key
2. Verify Authorino validates the key at the gateway layer (check Authorino logs)
3. Verify the request reaches the model only after Authorino approval
4. Send an inference request with an invalid API key
5. Verify Authorino rejects the key at the gateway layer
6. Verify the request never reaches the model serving pod (check model pod logs)
7. Verify Authorino produces an audit event for both the accepted and rejected key attempts

**Expected Results**:
- All API key validation occurs at the Authorino gateway layer
- Valid keys pass Authorino validation and reach the model
- Invalid keys are rejected by Authorino before reaching the model
- Authorino logs include validation events for security auditing
- No inference compute is consumed for requests with invalid keys

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
