# TC-EGRESS-001: External model reachable via Istio egress routing

**Priority**: P0
**Objective**: Verify that an external LLM provider endpoint is reachable through the inference gateway via Istio ServiceEntry and DestinationRule configuration

**Preconditions**:
- Istio service mesh configured with egress gateway
- External LLM provider test account available (e.g., Azure OpenAI)
- ServiceEntry and DestinationRule manifests configured for the external provider

**Test Steps**:
1. Apply the Istio ServiceEntry defining the external model endpoint
2. Apply the Istio DestinationRule configuring TLS and connection settings
3. Verify the ServiceEntry and DestinationRule are accepted by the cluster
4. Send an inference request through the MaaS gateway targeting the external model
5. Verify the request is routed through the Istio egress gateway to the external provider
6. Verify the inference response is returned successfully through the gateway

**Expected Results**:
- ServiceEntry and DestinationRule are created without errors
- Inference requests to external models are routed through the Istio egress gateway
- External provider returns a valid inference response
- Response is returned to the client through the MaaS gateway

**Test Data**:
```yaml
apiVersion: networking.istio.io/v1beta1
kind: ServiceEntry
metadata:
  name: azure-openai-external
  namespace: maas-egress
spec:
  hosts:
    - rhoai-test.openai.azure.com
  ports:
    - number: 443
      name: https
      protocol: TLS
  location: MESH_EXTERNAL
  resolution: DNS
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
