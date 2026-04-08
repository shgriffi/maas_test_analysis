# TC-TELEM-001: Token usage event emitted after inference request

**Priority**: P0
**Objective**: Verify that a structured token usage event is emitted to the external metering system after each inference request

**Preconditions**:
- Token consumption telemetry BBR plugin is active
- External metering system (e.g., OpenMeter) is configured and reachable
- Model is deployed and accessible through MaaS gateway

**Test Steps**:
1. Send an inference request through the MaaS gateway
2. Verify the inference response is returned successfully
3. Check the external metering system for a new token usage event within 60 seconds
4. Verify the event contains requester identity, group, model name, and provider
5. Verify the event contains tokens_in (prompt tokens) and tokens_out (completion tokens)
6. Verify the event contains a subscription reference
7. Verify the event timestamp is within expected range of the request time

**Expected Results**:
- A token usage event is delivered to the metering system after the inference request completes
- Event contains all required fields: requester identity, group, tokens in/out, model, provider, subscription
- Event delivery occurs within 1 minute of the request

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
