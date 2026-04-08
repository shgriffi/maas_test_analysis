# TC-VLLM-004: API key authentication works for vLLM models

**Priority**: P0
**Objective**: Verify that API key authentication is enforced for vLLM models exposed through MaaS gateway

**Preconditions**:
- vLLM model deployed and exposed through MaaS gateway
- Valid API key created for a user with subscription access to the model

**Test Steps**:
1. Send an inference request to the vLLM model without an API key
2. Verify the request is rejected with HTTP 401 or 403
3. Send an inference request with an invalid API key
4. Verify the request is rejected
5. Send an inference request with the valid API key
6. Verify the request succeeds and returns a model response

**Expected Results**:
- Requests without API keys are rejected (HTTP 401)
- Requests with invalid API keys are rejected (HTTP 401 or 403)
- Requests with valid API keys succeed and return inference results
- Authentication behavior is identical to llm-d model endpoints

**Test Data**:
```bash
# No auth - should fail
curl -s -o /dev/null -w "%{http_code}" -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "granite-3b-vllm", "messages": [{"role": "user", "content": "Hello"}]}'

# Valid auth - should succeed
curl -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Authorization: Bearer sk-maas-abc123def456" \
  -H "Content-Type: application/json" \
  -d '{"model": "granite-3b-vllm", "messages": [{"role": "user", "content": "Hello"}]}'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
