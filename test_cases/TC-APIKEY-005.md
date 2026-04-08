# TC-APIKEY-005: Revoked API key is rejected at gateway

**Priority**: P0
**Objective**: Verify that a revoked API key is immediately rejected when used for gateway authentication

**Preconditions**:
- User has an active API key that has been verified working

**Test Steps**:
1. Send an inference request with the active API key and confirm it succeeds
2. Revoke the API key
3. Immediately send another inference request with the same key
4. Verify the request is rejected with HTTP 401 or 403
5. Wait 30 seconds and retry with the revoked key
6. Verify the request is still rejected
7. Verify the error response does not reveal whether the key exists or was revoked (security)

**Expected Results**:
- Revoked keys are rejected immediately (no caching delay)
- HTTP 401 is returned for revoked keys
- Error response is generic (does not distinguish between "invalid key" and "revoked key" to prevent enumeration)

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
