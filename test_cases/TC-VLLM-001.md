# TC-VLLM-001: Enable vLLM model exposure through MaaS checkbox

**Priority**: P0
**Objective**: Verify that a vLLM-served model can be exposed through MaaS gateway using the dashboard checkbox

**Preconditions**:
- vLLM ServingRuntime is installed on the cluster
- A model is deployed using the vLLM runtime

**Test Steps**:
1. Navigate to the OpenShift AI dashboard model serving page
2. Locate the vLLM-deployed model in the model list
3. Verify that the MaaS checkbox is available for the vLLM deployment
4. Enable the MaaS checkbox for the vLLM model
5. Wait for the model to become accessible through the MaaS gateway
6. Send an inference request to the model through the MaaS gateway endpoint

**Expected Results**:
- MaaS checkbox appears for vLLM deployments (not only llm-d)
- Enabling the checkbox successfully exposes the model through the MaaS gateway
- Inference requests through the gateway return valid model responses

**Test Data**:
```bash
curl -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Authorization: Bearer <api-key>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "granite-3b-vllm",
    "messages": [{"role": "user", "content": "What is Kubernetes?"}],
    "max_tokens": 100
  }'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
