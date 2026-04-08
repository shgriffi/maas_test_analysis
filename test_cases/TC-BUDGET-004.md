# TC-BUDGET-004: Circuit breaker fail-closed mode when metering system is unreachable

**Priority**: P1
**Objective**: Verify that in fail-closed mode, inference requests are denied when the external metering system is unreachable

**Preconditions**:
- Circuit breaker BBR plugin is active and configured in fail-closed mode
- External metering system is made unreachable

**Test Steps**:
1. Configure the circuit breaker failure mode to "fail-closed"
2. Block network access to the external metering system
3. Send an inference request through the MaaS gateway
4. Verify the budget check attempt fails due to metering system being unreachable
5. Verify the inference request is DENIED
6. Verify the gateway returns an appropriate error response (e.g., HTTP 503)
7. Restore network access to the metering system
8. Send another inference request and verify it succeeds

**Expected Results**:
- In fail-closed mode, metering system unavailability blocks all inference requests
- An appropriate error response is returned to the client (not HTTP 429, which is for budget exhaustion)
- Once the metering system is restored, requests resume normally
- Logs record the failure with sufficient detail for troubleshooting

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
