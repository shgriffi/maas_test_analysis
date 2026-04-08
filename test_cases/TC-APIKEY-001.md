# TC-APIKEY-001: Create API key with no expiration

**Priority**: P0
**Objective**: Verify that a user can create a permanent API key (no expiration) through the self-service API

**Preconditions**:
- User is authenticated and has an active subscription
- API key self-service endpoint is available

**Test Steps**:
1. Send a create API key request with a display name and no expiration date
2. Verify the response returns HTTP 201
3. Verify the response body contains the plaintext API key
4. Verify the response includes key metadata (id, display name, creation timestamp)
5. Verify no expiration field is present (or is null) in the response
6. Use the plaintext key to authenticate an inference request
7. Verify the inference request succeeds

**Expected Results**:
- API key is created successfully with HTTP 201
- Plaintext key is returned in the creation response
- Key has no expiration date
- Key is immediately usable for gateway authentication

**Test Data**:
```bash
curl -X POST https://<maas-gateway>/api/v1/keys \
  -H "Authorization: Bearer <user-auth-token>" \
  -H "Content-Type: application/json" \
  -d '{
    "displayName": "Production integration key"
  }'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
