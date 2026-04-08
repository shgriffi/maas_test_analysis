# TC-TELEM-003: Telemetry delivery within 1 minute

**Priority**: P0
**Objective**: Verify that token consumption telemetry events are delivered to the external metering system within the 1-minute SLA

**Preconditions**:
- Telemetry BBR plugin is active and configured
- External metering system is reachable and accepting events

**Test Steps**:
1. Record the current timestamp
2. Send an inference request through the MaaS gateway
3. Record the request completion timestamp
4. Poll the external metering system for the corresponding usage event
5. Record the timestamp when the event first appears in the metering system
6. Calculate the delivery delay: metering_event_time - request_completion_time
7. Repeat for 10 requests and calculate average and maximum delivery delay

**Expected Results**:
- All 10 events are delivered to the metering system
- Maximum delivery delay is under 60 seconds
- Average delivery delay is significantly under 60 seconds
- No events are lost or duplicated

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
