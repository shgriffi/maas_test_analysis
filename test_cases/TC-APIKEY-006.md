# TC-APIKEY-006: Bearer token authentication at gateway

**Priority**: P0
**Objective**: Verify that API keys work via the standard `Authorization: Bearer <key>` HTTP header for gateway authentication

**Preconditions**:
- Active API key created for a user with subscription access

**Test Steps**:
1. Send an inference request with the API key in the `Authorization: Bearer <key>` header
2. Verify the request is authenticated and succeeds
3. Send an inference request with the key in a non-standard header (e.g., `X-API-Key`)
4. Verify the non-standard header is rejected (key must be in Authorization header)
5. Send a request with `Authorization: Bearer` but no key value
6. Verify the request is rejected with HTTP 401
7. Send a request with `Authorization: Basic <key>` (wrong scheme)
8. Verify the request is rejected

**Expected Results**:
- `Authorization: Bearer <key>` is the accepted authentication method
- Non-standard key headers are not accepted
- Missing or malformed Authorization headers return HTTP 401
- Wrong authorization scheme (Basic instead of Bearer) is rejected

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
