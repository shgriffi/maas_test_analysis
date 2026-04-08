# TC-SEC-004: OIDC token replay attack prevention

**Priority**: P1
**Objective**: Verify that expired or replayed OIDC tokens are rejected by the gateway

**Preconditions**:
- External OIDC provider configured
- Valid OIDC token available for the test user

**Test Steps**:
1. Obtain a valid OIDC JWT token with a short expiration (e.g., 5 minutes)
2. Send an inference request with the valid token — verify it succeeds
3. Wait for the token to expire
4. Send another inference request with the expired token
5. Verify the request is rejected with HTTP 401
6. Capture a valid token and modify the "exp" (expiration) claim to extend it
7. Send an inference request with the tampered token
8. Verify the request is rejected (signature validation fails)

**Expected Results**:
- Expired tokens are rejected with HTTP 401
- Tokens with modified claims (tampered signature) are rejected
- The gateway validates both token expiration and signature integrity
- No inference requests succeed with expired or tampered tokens

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
