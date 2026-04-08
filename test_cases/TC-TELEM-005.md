# TC-TELEM-005: Failed telemetry emission logged for alerting

**Priority**: P1
**Objective**: Verify that failed telemetry emission attempts are logged with sufficient detail for alerting

**Preconditions**:
- Telemetry BBR plugin is active
- External metering system is configured but made unavailable (e.g., wrong endpoint or blocked network)

**Test Steps**:
1. Configure the telemetry plugin to point to an unreachable metering endpoint
2. Send an inference request through the MaaS gateway
3. Verify the inference request still succeeds (telemetry failure should not block inference)
4. Check the BBR plugin logs for a failed emission entry
5. Verify the log entry includes: timestamp, error type, destination endpoint, and request correlation ID
6. Verify the log level is appropriate for alerting (ERROR or WARN)

**Expected Results**:
- Inference requests are not blocked by telemetry emission failures
- Failed emissions produce log entries with actionable details
- Log entries include enough information to diagnose the failure
- Logs are at an alertable severity level

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
