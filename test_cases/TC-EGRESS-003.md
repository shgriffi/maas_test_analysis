# TC-EGRESS-003: Bidirectional API translation for external provider

**Priority**: P0
**Objective**: Verify that the API translation plugin converts requests from OpenAI-compatible format to provider-specific format and responses back to OpenAI-compatible format

**Preconditions**:
- External model endpoint configured and reachable
- API translation BBR plugin configured for at least one provider
- Provider credentials available via labeled K8s secret

**Test Steps**:
1. Send an inference request in OpenAI-compatible format through the MaaS gateway targeting an external model
2. Verify the gateway translates the request to the provider's specific format before forwarding
3. Verify the external provider processes the request successfully
4. Verify the gateway translates the provider's response back to OpenAI-compatible format
5. Validate the response structure matches the standard OpenAI chat completions format (id, object, choices, usage fields)

**Expected Results**:
- Client sends a standard OpenAI-compatible request and receives a standard OpenAI-compatible response
- The provider receives the request in its native format
- Response fields (id, object, created, model, choices, usage) conform to OpenAI chat completions schema
- Token usage (prompt_tokens, completion_tokens, total_tokens) is accurately reflected in the translated response

**Test Data**:
```bash
# Client sends OpenAI-compatible format
curl -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Authorization: Bearer sk-maas-user-key-123" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4-azure-external",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Explain containers in one sentence."}
    ],
    "max_tokens": 50,
    "temperature": 0.7
  }'
```

**Expected Response**:
```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1712505600,
  "model": "gpt-4-azure-external",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Containers are lightweight, portable units that package an application with its dependencies."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 25,
    "completion_tokens": 15,
    "total_tokens": 40
  }
}
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
