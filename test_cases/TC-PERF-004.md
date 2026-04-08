# TC-PERF-004: Concurrent API key operations

**Priority**: P2
**Objective**: Verify that the API key management system handles concurrent create, list, and revoke operations without errors

**Preconditions**:
- Multiple authenticated users with active subscriptions
- API key self-service endpoint available

**Test Steps**:
1. Spawn 20 concurrent API key creation requests from different users
2. Verify all 20 keys are created successfully with unique key values
3. Spawn 20 concurrent list requests from the same users
4. Verify each user sees only their own keys
5. Spawn 10 concurrent revoke requests targeting different keys
6. Verify all 10 revocations succeed
7. Verify revoked keys are immediately rejected at the gateway
8. Verify no race conditions (duplicate key IDs, missed revocations, cross-user key leakage)

**Expected Results**:
- All concurrent operations complete without errors
- No duplicate API key values are generated
- Revocations take effect immediately even under concurrent load
- No cross-user data leakage occurs under concurrent access
- Response times remain acceptable under concurrent load

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
