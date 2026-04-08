# TC-BUDGET-003: Circuit breaker fail-open mode when metering system is unreachable

**Priority**: P1
**Objective**: Verify that in fail-open mode, inference requests are allowed to proceed when the external metering system is unreachable

**Preconditions**:
- Circuit breaker BBR plugin is active and configured in fail-open mode
- External metering system is made unreachable (network block or shutdown)

**Test Steps**:
1. Configure the circuit breaker failure mode to "fail-open"
2. Block network access to the external metering system
3. Send an inference request through the MaaS gateway
4. Verify the budget check attempt fails due to metering system being unreachable
5. Verify the inference request is ALLOWED to proceed despite the budget check failure
6. Verify the inference response is returned to the client
7. Check logs for a warning entry about the failed budget check

**Expected Results**:
- In fail-open mode, metering system unavailability does not block inference requests
- Requests are forwarded to the model normally
- A warning is logged about the failed budget check for alerting
- No data is lost — the request proceeds without budget validation

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
