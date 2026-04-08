# TC-EGRESS-004: Combined in-cluster and out-of-cluster model serving through single gateway

**Priority**: P0
**Objective**: Verify that the MaaS gateway serves both local (in-cluster) and external (out-of-cluster) models through a single endpoint

**Preconditions**:
- Local vLLM or llm-d model deployed and exposed through MaaS
- External model configured via Istio egress with API key injection and API translation
- Both models assigned to the same subscription

**Test Steps**:
1. Send an inference request to the local model through the MaaS gateway
2. Verify the request is served by the in-cluster model and returns a valid response
3. Send an inference request to the external model through the same MaaS gateway base URL
4. Verify the request is routed to the external provider and returns a valid response
5. Verify both responses conform to the same OpenAI-compatible format
6. Verify governance (rate limiting, authentication) applies consistently to both models
7. Verify the client experience is identical regardless of model location

**Expected Results**:
- Both local and external models are accessible through the same gateway base URL
- Model selection is done via the "model" field in the request body
- Response format is identical for both local and external models
- Rate limiting and authentication apply uniformly to both model types

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
