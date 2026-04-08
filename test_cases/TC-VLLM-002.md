# TC-VLLM-002: Rate limiting parity between vLLM and llm-d models

**Priority**: P0
**Objective**: Verify that rate limiting governance applies identically to vLLM-served models as it does to llm-d models

**Preconditions**:
- One model deployed via vLLM runtime with MaaS enabled
- One model deployed via llm-d runtime with MaaS enabled
- Both models assigned to the same subscription with identical rate limit quotas

**Test Steps**:
1. Configure a Subscription CRD granting 10 requests per minute to both models
2. Send 10 valid inference requests to the vLLM model in rapid succession
3. Send an 11th request to the vLLM model
4. Verify the 11th request is rate-limited
5. Repeat steps 2-4 for the llm-d model
6. Compare rate limiting behavior between both runtimes

**Expected Results**:
- Both vLLM and llm-d models accept exactly 10 requests within the rate limit window
- The 11th request to both models returns HTTP 429 (Too Many Requests)
- Rate limiting response format is identical for both runtimes

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
