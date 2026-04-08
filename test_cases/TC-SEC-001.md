# TC-SEC-001: API keys stored as cryptographic hashes only

**Priority**: P0
**Objective**: Verify that API keys are stored as cryptographic hashes in the backend and the plaintext key is never persisted

**Preconditions**:
- API key self-service is enabled
- Access to the backend storage (database or CRD inspection)

**Test Steps**:
1. Create a new API key and record the returned plaintext key
2. Inspect the APIKey CRD resource on the cluster
3. Verify the stored value is a cryptographic hash (not the plaintext key)
4. Verify the hash length and format is consistent with a secure hashing algorithm (e.g., SHA-256, bcrypt)
5. Inspect the backend database (if applicable) for the key record
6. Verify the database also stores only the hash
7. Verify there are no plaintext key values in controller logs or events

**Expected Results**:
- The APIKey CRD stores a hash, not the plaintext key
- The hash is not reversible (one-way cryptographic hash)
- No plaintext keys appear in logs, events, or audit trails
- The stored hash format is consistent with industry-standard hashing

**Validation**:
- `kubectl get apikey jsmith-key-01 -o jsonpath='{.spec.keyHash}'` returns a hash value, not the plaintext key
- `kubectl logs -l app=maas-controller | grep -i "sk-"` returns no matches

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
