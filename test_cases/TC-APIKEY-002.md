# TC-APIKEY-002: Create API key with custom expiration

**Priority**: P0
**Objective**: Verify that a user can create an API key with a customer-defined expiration date

**Preconditions**:
- User is authenticated and has an active subscription

**Test Steps**:
1. Send a create API key request with a display name and an expiration date 30 days in the future
2. Verify the response returns HTTP 201 with the plaintext key
3. Verify the expiration date in the response matches the requested date
4. Use the key immediately to authenticate an inference request — verify it succeeds
5. Create another key with an expiration date in the past
6. Verify the creation is rejected or the key is immediately unusable

**Expected Results**:
- API key with future expiration is created successfully
- Expiration timestamp is persisted and returned in key metadata
- Key functions normally before expiration
- Keys with past expiration dates are rejected at creation or immediately inactive

**Test Data**:
```bash
curl -X POST https://<maas-gateway>/api/v1/keys \
  -H "Authorization: Bearer <user-auth-token>" \
  -H "Content-Type: application/json" \
  -d '{
    "displayName": "Quarterly rotation key",
    "expiresAt": "2026-07-07T00:00:00Z"
  }'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
