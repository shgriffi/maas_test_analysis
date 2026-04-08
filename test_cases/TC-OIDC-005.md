# TC-OIDC-005: Reject malformed or expired OIDC token

**Priority**: P0
**Objective**: Verify that the gateway rejects malformed, expired, or tampered OIDC tokens

**Test Steps**:
1. Send an inference request with a completely invalid JWT string (random characters)
2. Verify the request is rejected with HTTP 401
3. Send an inference request with a JWT that has an invalid signature (valid structure but tampered payload)
4. Verify the request is rejected with HTTP 401
5. Send an inference request with an expired JWT (valid issuer but past expiration)
6. Verify the request is rejected with HTTP 401
7. Send an inference request with a JWT from an unregistered OIDC issuer
8. Verify the request is rejected with HTTP 401

**Expected Results**:
- Random string tokens return HTTP 401
- Tampered JWT (invalid signature) returns HTTP 401
- Expired JWT returns HTTP 401
- JWT from unregistered issuer returns HTTP 401
- Error responses do not leak internal authentication details

**Test Data**:
```bash
# Malformed token
curl -s -o /dev/null -w "%{http_code}" -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Authorization: Bearer not-a-valid-jwt-token" \
  -H "Content-Type: application/json" \
  -d '{"model": "granite-3b-code", "messages": [{"role": "user", "content": "test"}]}'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
