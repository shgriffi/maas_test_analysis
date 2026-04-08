# TC-SEC-005: Expired API key rejected at gateway

**Priority**: P0
**Objective**: Verify that API keys past their expiration date are rejected by the gateway

**Preconditions**:
- API key created with a short custom expiration (e.g., 2 minutes in the future)

**Test Steps**:
1. Create an API key with an expiration 2 minutes in the future
2. Immediately use the key for an inference request — verify it succeeds
3. Wait for the key to expire (2+ minutes)
4. Send another inference request with the expired key
5. Verify the request is rejected with HTTP 401
6. Verify the key status shows as "expired" in the key list
7. Verify the expired key cannot be renewed or extended (must create a new key)

**Expected Results**:
- API key works before its expiration timestamp
- API key is rejected immediately after expiration
- Expired keys show "expired" status in listings
- No mechanism exists to extend an expired key — a new key must be created

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
