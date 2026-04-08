# TC-APIKEY-003: List user's API keys

**Priority**: P1
**Objective**: Verify that a user can list all their API keys with metadata (but not plaintext keys)

**Preconditions**:
- User has created at least 3 API keys (one permanent, one with future expiration, one revoked)

**Test Steps**:
1. Send a list API keys request authenticated as the user
2. Verify the response returns HTTP 200 with an array of key objects
3. Verify each key object includes: id, display name, creation timestamp, expiration (if set), status
4. Verify the plaintext key is NOT included in any list entry
5. Verify all 3 keys appear in the list (including the revoked one with appropriate status)
6. Verify a different user's keys are NOT included in the list

**Expected Results**:
- All user's keys are returned with metadata
- Plaintext keys are never exposed in list responses
- Revoked keys appear with a "revoked" status
- Keys from other users are not visible

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
