# TC-BUDGET-002: Budget exhausted returns HTTP 429 with structured error body

**Priority**: P0
**Objective**: Verify that inference requests are denied with HTTP 429 and a structured error body when the user's budget is exhausted

**Preconditions**:
- Circuit breaker BBR plugin is active
- External metering system reports budget exhausted for the test user's subscription

**Test Steps**:
1. Configure or deplete the test user's budget in the external metering system to zero
2. Send an inference request through the MaaS gateway
3. Verify the circuit breaker queries the metering system and receives a "budget exhausted" response
4. Verify the gateway returns HTTP 429 to the client
5. Verify the response body contains a structured error with details about budget exhaustion
6. Verify the inference request was NOT forwarded to the model

**Expected Results**:
- HTTP 429 is returned when budget is exhausted
- Response body contains a structured error (JSON) indicating budget exhaustion
- No inference request is sent to the model (saving compute resources)
- The error response is returned promptly (not after inference timeout)

**Expected Response**:
```json
{
  "error": {
    "type": "budget_exhausted",
    "message": "Token budget for subscription has been exhausted",
    "code": 429
  }
}
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
