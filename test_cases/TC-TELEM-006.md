# TC-TELEM-006: Telemetry events work for both local and external model requests

**Priority**: P0
**Objective**: Verify that token consumption telemetry events are emitted for both local (in-cluster) and external model inference requests

**Preconditions**:
- Local model deployed via vLLM or llm-d with MaaS enabled
- External model configured via Istio egress
- Telemetry BBR plugin active on the gateway

**Test Steps**:
1. Send an inference request to the local model through the MaaS gateway
2. Verify a telemetry event is emitted with the local model name and "local" provider
3. Send an inference request to the external model through the MaaS gateway
4. Verify a telemetry event is emitted with the external model name and correct provider
5. Compare event structures between local and external model events
6. Verify both events contain accurate token counts

**Expected Results**:
- Telemetry events are emitted for both local and external model requests
- Events correctly identify the model and provider type
- Token counts are accurate for both model types
- Event structure is consistent regardless of model location

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
