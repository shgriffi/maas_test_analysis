# TC-OIDC-003: Quota enforcement for BYOIDC users

**Priority**: P0
**Objective**: Verify that subscription quotas are enforced for users authenticated via external OIDC providers

**Preconditions**:
- External OIDC provider configured
- Subscription CRD with 5 requests/min quota for the test user's group
- Test user has valid OIDC credentials

**Test Steps**:
1. Obtain a valid JWT token for the BYOIDC test user
2. Send 5 inference requests in rapid succession using the JWT token
3. Verify all 5 requests succeed
4. Send a 6th request within the same rate limit window
5. Verify the 6th request is rate-limited with HTTP 429
6. Wait for the rate limit window to reset
7. Send another request and verify it succeeds

**Expected Results**:
- BYOIDC users are subject to the same quota enforcement as OpenShift-authenticated users
- Rate limiting activates after exceeding the subscription quota
- HTTP 429 response includes rate limit headers indicating when the window resets
- Quota enforcement resets correctly after the time window

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
