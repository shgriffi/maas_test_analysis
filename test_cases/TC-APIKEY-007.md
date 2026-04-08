# TC-APIKEY-007: Show-once key generation — plaintext not retrievable after creation

**Priority**: P1
**Objective**: Verify that the plaintext API key is shown only once at creation and cannot be retrieved again

**Preconditions**:
- User is authenticated with an active subscription

**Test Steps**:
1. Create a new API key and record the plaintext key from the creation response
2. Attempt to retrieve the key details via the Get API key endpoint using the key ID
3. Verify the Get response includes key metadata (id, display name, timestamps) but NOT the plaintext key
4. Verify the plaintext key is not included in the List keys response
5. Verify there is no "regenerate" or "show key" endpoint
6. Verify the stored key uses the key to authenticate at the gateway (confirming the plaintext key was valid)

**Expected Results**:
- Plaintext key appears only in the initial creation response
- Subsequent Get and List operations never include the plaintext key
- No mechanism exists to retrieve or regenerate the plaintext key after creation
- The key functions correctly for authentication, confirming the one-time display was accurate

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
