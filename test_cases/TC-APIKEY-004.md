# TC-APIKEY-004: Revoke individual API key without affecting other keys

**Priority**: P0
**Objective**: Verify that revoking one API key does not affect other active keys belonging to the same user

**Preconditions**:
- User has created 2 active API keys (key-A and key-B)
- Both keys have been verified to work for gateway authentication

**Test Steps**:
1. Revoke key-A via the API key management endpoint
2. Verify the revoke response returns success
3. Attempt to use key-A for an inference request
4. Verify key-A is rejected with HTTP 401
5. Use key-B for an inference request
6. Verify key-B still works and the inference request succeeds
7. List the user's keys and verify key-A shows status "revoked" while key-B shows status "active"

**Expected Results**:
- Revoking key-A returns a success response
- Key-A is immediately rejected at the gateway after revocation
- Key-B continues to function normally
- Key list reflects the correct status for both keys

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
