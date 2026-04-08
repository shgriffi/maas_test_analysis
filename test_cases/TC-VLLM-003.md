# TC-VLLM-003: Token tracking parity between vLLM and llm-d models

**Priority**: P0
**Objective**: Verify that token consumption tracking produces consistent metrics for vLLM-served models compared to llm-d models

**Preconditions**:
- One model deployed via vLLM runtime with MaaS enabled
- One model deployed via llm-d runtime with MaaS enabled
- Token consumption metrics scraping is active

**Test Steps**:
1. Send identical inference requests to both the vLLM and llm-d models with the same prompt and max_tokens
2. Query Prometheus for token consumption metrics for the vLLM model
3. Query Prometheus for token consumption metrics for the llm-d model
4. Compare the metric labels, formats, and values between the two runtimes
5. Verify that prompt_tokens and completion_tokens are recorded for both

**Expected Results**:
- Token consumption metrics are recorded for vLLM models with the same metric names and label structure as llm-d
- Prompt tokens and completion tokens are tracked separately for both runtimes
- Token counts are consistent with the actual model output length

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
