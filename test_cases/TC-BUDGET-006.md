# TC-BUDGET-006: Failed budget check calls logged for alerting

**Priority**: P1
**Objective**: Verify that failed budget check calls are logged with actionable details for monitoring and alerting

**Preconditions**:
- Circuit breaker BBR plugin is active
- External metering system is reachable but returns errors (e.g., 500 Internal Server Error)

**Test Steps**:
1. Configure the metering system to return HTTP 500 errors for balance API queries
2. Send an inference request through the MaaS gateway
3. Verify the circuit breaker handles the error according to the configured failure mode
4. Check the BBR plugin logs for a failure entry
5. Verify the log contains: timestamp, error type, HTTP status from metering system, request correlation ID
6. Verify the log level is ERROR or WARN (suitable for alerting)
7. Verify logs include the metering endpoint URL for debugging

**Expected Results**:
- Failed budget check calls produce structured log entries
- Log entries contain sufficient detail to diagnose the issue
- Log severity is appropriate for triggering alerts
- The logging does not expose sensitive information (no API keys or tokens in logs)

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
