# TC-BUDGET-005: Circuit breaker activation and deactivation per gateway

**Priority**: P1
**Objective**: Verify that the circuit breaker can be activated and deactivated independently per gateway

**Preconditions**:
- Two MaaS gateway instances configured
- External metering system configured with budget for test user

**Test Steps**:
1. Activate the circuit breaker on gateway-A and deactivate it on gateway-B
2. Deplete the test user's budget in the metering system
3. Send an inference request through gateway-A
4. Verify the request is denied with HTTP 429 (budget exhausted, circuit breaker active)
5. Send an inference request through gateway-B
6. Verify the request succeeds (circuit breaker is deactivated, no budget check occurs)
7. Activate the circuit breaker on gateway-B
8. Send another request through gateway-B and verify it is now denied

**Expected Results**:
- Circuit breaker enforcement is independently configurable per gateway
- Active gateways enforce budget checks; deactivated gateways skip them
- Configuration changes take effect without requiring gateway restart

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
