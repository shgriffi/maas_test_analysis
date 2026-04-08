# TC-EGRESS-005: Egress routing failure handling for unreachable external model

**Priority**: P1
**Objective**: Verify that the gateway handles external model connectivity failures gracefully with appropriate error responses

**Preconditions**:
- External model configured via ServiceEntry pointing to an unreachable or non-existent endpoint
- Or simulate by applying a NetworkPolicy blocking egress traffic

**Test Steps**:
1. Configure a ServiceEntry pointing to a non-existent external model endpoint
2. Send an inference request targeting the unreachable external model
3. Verify the gateway returns an appropriate error response (not a generic 500)
4. Verify the error response includes a meaningful message indicating the external model is unreachable
5. Verify the gateway remains healthy and continues serving local models
6. Apply a NetworkPolicy blocking egress to a previously working external model
7. Send a request and verify the timeout/connection error is handled gracefully

**Expected Results**:
- Unreachable external models produce a clear error response (e.g., HTTP 502 or 504)
- Error message indicates the external model endpoint is unavailable
- Local model serving is unaffected by external model failures
- Gateway does not crash or enter an error state due to egress failures

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
