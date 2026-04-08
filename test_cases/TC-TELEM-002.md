# TC-TELEM-002: Streaming response produces accurate token counts

**Priority**: P1
**Objective**: Verify that SSE-aware token counting accurately captures token usage for streaming inference responses

**Preconditions**:
- Token consumption telemetry BBR plugin is active
- External metering system is configured
- Model supports streaming responses

**Test Steps**:
1. Send a streaming inference request (stream: true) through the MaaS gateway
2. Verify the SSE stream is returned to the client
3. Wait for the stream to complete
4. Check the external metering system for the token usage event
5. Verify prompt_tokens matches the input prompt token count
6. Verify completion_tokens matches the total tokens across all streamed chunks
7. Compare the token counts with a non-streaming request using the same prompt and model

**Expected Results**:
- Token usage event is emitted after the streaming response completes (not per-chunk)
- Completion token count accurately reflects the total across all streamed chunks
- Token counts for streaming and non-streaming requests with identical inputs are consistent
- No duplicate events are emitted for a single streaming request

**Test Data**:
```bash
curl -X POST https://<maas-gateway>/v1/chat/completions \
  -H "Authorization: Bearer sk-maas-abc123" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "granite-3b-code",
    "messages": [{"role": "user", "content": "Write a hello world in Python"}],
    "max_tokens": 100,
    "stream": true
  }'
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
