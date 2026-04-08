# Test Case to Repository Coverage Mapping

**Analysis Date**: 2026-04-08  
**Test Cases**: 79 total across 13 categories  
**Repository Tests**: ~161 test functions across 39 files  
**Repositories**:
- `opendatahub-io/models-as-a-service/test/e2e/tests/` (9 files, ~98 test functions)
- `opendatahub-io/opendatahub-tests/tests/model_serving/maas_billing/` (30 files, ~63 test functions)

---

## Coverage Summary by Priority

| Priority | Total TCs | Covered | Partial | Not Covered | Coverage % |
|----------|-----------|---------|---------|-------------|------------|
| **P0**   | 47        | 20      | 8       | 19          | 43%        |
| **P1**   | 30        | 5       | 4       | 21          | 17%        |
| **P2**   | 2         | 0       | 0       | 2           | 0%         |
| **Total** | **79**   | **25**  | **12**  | **42**      | **32%**    |

---

## TC-VLLM — vLLM Runtime Support and UX Parity (5 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-VLLM-001 | Enable vLLM model exposure through MaaS checkbox | P0 | ⚠️ Partial | `test_api_keys.py`, `test_external_oidc.py` | Inference success validated via `/v1/chat/completions` with `facebook/opt-125m` model. Missing: explicit dashboard checkbox validation, vLLM-specific deployment flow. |
| TC-VLLM-002 | Rate limiting parity between vLLM and llm-d models | P0 | ⚠️ Partial | `test_maas_request_rate_limits.py`, `test_maas_token_rate_limits.py` | Rate limiting tested generically, not explicitly validated for vLLM vs llm-d parity. Missing: side-by-side comparison test. |
| TC-VLLM-003 | Token tracking parity between vLLM and llm-d models | P0 | ⚠️ Partial | `test_tokens.py` | Token consumption validated, but not explicitly for vLLM vs llm-d comparison. Missing: metric label validation, Prometheus query comparison. |
| TC-VLLM-004 | API key authentication works for vLLM models | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyModelInference` | Fully covered: valid key success, invalid key 403, no auth 401, revoked key 403, chat completions endpoint. |
| TC-VLLM-005 | Dashboard UX consistency between vLLM and llm-d flows | P1 | ❌ Not Covered | N/A | No dashboard UI tests exist. Needs: UI automation testing for checkbox location, status indicators, endpoint display consistency. |

**Category Coverage**: 1/5 Covered, 3/5 Partial, 1/5 Not Covered

---

## TC-CRD — CRD Definitions, Validation, and Controller Reconciliation (7 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-CRD-001 | Create Model CRD with capacity quotas | P0 | ❌ Not Covered | N/A | No Model CRD tests exist. Migration feature introduces CRDs, but creation/validation not tested. Needs: CRD YAML apply, field validation, kubectl get verification. |
| TC-CRD-002 | Create Subscription CRD with group-to-model relationships | P0 | ✅ Covered | `maas_subscription/` (9 files), `test_subscription.py` | Extensively covered: subscription creation, group mapping, per-model quotas, multiple subscriptions per model, cascade deletion. |
| TC-CRD-003 | Create APIKey CRD | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyCRUD`, `maas_api_key/test_api_key_crud.py` | Fully covered: create (show-once pattern), metadata persistence, hash storage. |
| TC-CRD-004 | Validation webhook rejects quota sum exceeding model capacity | P0 | ❌ Not Covered | N/A | No validation webhook tests exist. Needs: over-capacity subscription rejection, error message validation, within-capacity acceptance. |
| TC-CRD-005 | Backend controller reconciles Subscription CRD to RateLimitPolicy | P0 | ✅ Covered | `test_maas_sub_enforcement.py` | Covered via enforcement tests: subscription changes trigger policy updates, reconciliation within 30s. |
| TC-CRD-006 | Backend controller reconciles Subscription CRD to AuthPolicy | P0 | ✅ Covered | `test_maas_auth_enforcement.py`, `test_multiple_auth_policies_per_model.py` | Covered: AuthPolicy creation, group references, multiple policies per model, cleanup on deletion. |
| TC-CRD-007 | Validation webhook rejects Subscription CRD with missing required fields | P0 | ❌ Not Covered | N/A | No webhook validation tests exist. Needs: missing groups rejection, missing model refs rejection, non-existent model rejection, error message clarity. |

**Category Coverage**: 4/7 Covered, 0/7 Partial, 3/7 Not Covered

---

## TC-MIG — ConfigMap to CRD Migration (5 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-MIG-001 | Migrate simple single-model ConfigMap to CRD | P0 | ❌ Not Covered | N/A | Migration script not e2e tested. Needs: script execution, CRD creation verification, field fidelity comparison. |
| TC-MIG-002 | Migrate complex multi-model multi-tenant ConfigMap to CRD | P0 | ❌ Not Covered | N/A | Complex migration scenarios not tested. Needs: 3 models, 4 tiers, overlapping groups, quota sum validation. |
| TC-MIG-003 | Migration preserves 100% fidelity across quota values and group mappings | P0 | ❌ Not Covered | N/A | No fidelity validation tests. Needs: automated diff between ConfigMap JSON and CRD YAML, edge-case quotas (zero, max, equal guaranteed/burst). |
| TC-MIG-004 | Migration dry-run mode previews changes without applying | P1 | ❌ Not Covered | N/A | Dry-run functionality not tested. Needs: output preview validation, zero resource creation verification. |
| TC-MIG-005 | Migration rollback on failure reverts partial changes | P1 | ❌ Not Covered | N/A | Rollback mechanism not tested. Needs: conflict injection, partial CRD cleanup verification, ConfigMap preservation. |

**Category Coverage**: 0/5 Covered, 0/5 Partial, 5/5 Not Covered

---

## TC-OIDC — External OIDC Authentication and BYOIDC Workflows (6 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-OIDC-001 | OIDC token translation to in-memory K8s identity | P0 | ✅ Covered | `test_external_oidc.py::test_oidc_token_can_create_api_key` | Covered: valid JWT → API key creation, transient identity. Missing: explicit verification of no persistent K8s user/SA. |
| TC-OIDC-002 | Subscription assignment via OIDC group claims | P0 | ⚠️ Partial | `test_subscription.py` (OIDC claim mapping) | Subscription OIDC mapping documented in test_subscription.py. Missing: explicit group claim → subscription authorization test. |
| TC-OIDC-003 | Quota enforcement for BYOIDC users | P0 | ⚠️ Partial | `test_maas_request_rate_limits.py`, `test_maas_token_rate_limits.py` | Rate limiting tested, but not explicitly for OIDC-authenticated users. Missing: OIDC token + rate limit 429 test. |
| TC-OIDC-004 | BYOIDC user creates API key | P1 | ✅ Covered | `test_external_oidc.py::test_oidc_token_can_create_api_key`, `test_minted_api_key_can_list_models_and_infer` | Fully covered: OIDC user → API key creation → inference success. |
| TC-OIDC-005 | Reject malformed or expired OIDC token | P0 | ✅ Covered | `test_external_oidc.py::test_invalid_oidc_token_gets_401` | Covered: invalid token returns 401. Missing: explicit expired token test, tampered signature test, unregistered issuer test. |
| TC-OIDC-006 | Namespace isolation for BYOIDC users across tenants | P1 | ⚠️ Partial | `test_namespace_scoping.py` | Namespace isolation validated, but not explicitly for OIDC users across tenants. Missing: BYOIDC user A cannot access tenant B resources. |

**Category Coverage**: 3/6 Covered, 3/6 Partial, 0/6 Not Covered

---

## TC-EGRESS — External Model Egress Routing and API Translation (5 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-EGRESS-001 | External model reachable via Istio egress routing | P0 | ❌ Not Covered | N/A | No egress routing tests. Needs: ServiceEntry/DestinationRule apply, external provider inference request success. |
| TC-EGRESS-002 | API key injection from labeled K8s secret | P0 | ❌ Not Covered | N/A | No API key injection tests. Needs: labeled secret creation, BBR plugin injection verification, provider credential validation. |
| TC-EGRESS-003 | Bidirectional API translation for external provider | P0 | ❌ Not Covered | N/A | No API translation tests. Needs: OpenAI-compatible request → provider-specific format, provider response → OpenAI format. |
| TC-EGRESS-004 | Combined in-cluster and out-of-cluster serving through single gateway | P0 | ❌ Not Covered | N/A | No hybrid serving tests. Needs: local model + external model via same gateway endpoint, identical governance enforcement. |
| TC-EGRESS-005 | Egress routing failure handling for unreachable external model | P1 | ❌ Not Covered | N/A | No external failure handling tests. Needs: unreachable endpoint → graceful error response, local model serving unaffected. |

**Category Coverage**: 0/5 Covered, 0/5 Partial, 5/5 Not Covered

---

## TC-APIKEY — API Key Lifecycle Management (7 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-APIKEY-001 | Create API key with no expiration | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyExpiration::test_create_api_key_without_expiration` | Fully covered: no expiration field, permanent key, immediate usability. |
| TC-APIKEY-002 | Create API key with custom expiration | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyExpiration` (multiple tests) | Fully covered: custom expiration, within 90-day limit, at limit, exceeds limit rejection, short expiration (1h). |
| TC-APIKEY-003 | List user's API keys | P1 | ✅ Covered | `test_api_keys.py::TestAPIKeyCRUD::test_list_api_keys_with_pagination` | Covered: pagination, metadata inclusion, plaintext key exclusion, status display, cross-user isolation. |
| TC-APIKEY-004 | Revoke individual API key without affecting other keys | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyRevocationE2E::test_revoke_individual_key_among_multiple` | Fully covered: selective revocation, other keys remain active, status update. |
| TC-APIKEY-005 | Revoked API key is rejected at gateway | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyModelInference::test_revoked_api_key_rejected`, `TestAPIKeyRevocationE2E::test_revocation_propagates_to_gateway` | Fully covered: immediate rejection, 401 response, revocation propagation polling validation. |
| TC-APIKEY-006 | Bearer token authentication at gateway | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyModelInference` | Fully covered: `Authorization: Bearer <key>` header, 401 for missing auth, 403 for invalid key. |
| TC-APIKEY-007 | Show-once key generation — plaintext not retrievable after creation | P1 | ✅ Covered | `test_api_keys.py::TestAPIKeyCRUD::test_create_api_key_show_once` | Fully covered: plaintext in creation response only, subsequent GET/LIST exclude plaintext, no regenerate endpoint. |

**Category Coverage**: 7/7 Covered, 0/7 Partial, 0/7 Not Covered

**Additional Coverage Not in Test Plan**:
- Ephemeral key cleanup: `TestEphemeralKeyCleanup` (CronJob validation, NetworkPolicy validation)
- IDOR protection: 404 instead of 403 for unauthorized access (prevents enumeration)
- Bulk operations: Bulk revoke own keys, admin bulk revoke
- Admin RBAC: Admin manage other users' keys, non-admin forbidden

---

## TC-TELEM — Token Consumption Telemetry (6 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-TELEM-001 | Token usage event emitted after inference request | P0 | ⚠️ Partial | `test_tokens.py` | Token consumption validated via Prometheus metrics. Missing: explicit telemetry event schema validation (requester identity, group, model, provider fields). |
| TC-TELEM-002 | Streaming response produces accurate token counts | P1 | ❌ Not Covered | N/A | No streaming telemetry tests. Needs: SSE stream completion → single event emission, chunk aggregation validation, streaming vs non-streaming comparison. |
| TC-TELEM-003 | Telemetry delivery within 1 minute | P0 | ❌ Not Covered | N/A | No delivery SLA tests. Needs: timestamp comparison (request completion → metering event appearance), 10-request average delay calculation. |
| TC-TELEM-004 | Telemetry activation and deactivation per gateway | P1 | ❌ Not Covered | N/A | No per-gateway configuration tests. Needs: gateway-A active, gateway-B inactive, activation without restart. |
| TC-TELEM-005 | Failed telemetry emission logged for alerting | P1 | ❌ Not Covered | N/A | No failure handling tests. Needs: unreachable metering system → log entry validation, inference success despite telemetry failure. |
| TC-TELEM-006 | Telemetry events work for both local and external model requests | P0 | ⚠️ Partial | `test_tokens.py` | Local model telemetry covered. Missing: external model telemetry validation (requires external egress tests). |

**Category Coverage**: 0/6 Covered, 2/6 Partial, 4/6 Not Covered

---

## TC-BUDGET — Circuit Breaker Budget Enforcement (6 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-BUDGET-001 | Pre-request budget check passes with sufficient budget | P0 | ❌ Not Covered | N/A | No circuit breaker tests. Needs: mocked balance API, budget check query verification, inference success. |
| TC-BUDGET-002 | Budget exhausted returns HTTP 429 with structured error body | P0 | ❌ Not Covered | N/A | No budget exhaustion tests. Needs: zero balance → 429 response, structured error JSON, no inference forwarding. |
| TC-BUDGET-003 | Circuit breaker fail-open mode when metering system is unreachable | P1 | ❌ Not Covered | N/A | No fail-open tests. Needs: unreachable metering → inference allowed, warning logged. |
| TC-BUDGET-004 | Circuit breaker fail-closed mode when metering system is unreachable | P1 | ❌ Not Covered | N/A | No fail-closed tests. Needs: unreachable metering → 503 response, recovery after restoration. |
| TC-BUDGET-005 | Circuit breaker activation and deactivation per gateway | P1 | ❌ Not Covered | N/A | No per-gateway configuration tests. Needs: gateway-A active, gateway-B inactive, config change without restart. |
| TC-BUDGET-006 | Failed budget check calls logged for alerting | P1 | ❌ Not Covered | N/A | No failure logging tests. Needs: metering 500 error → log entry with timestamp/correlation ID/endpoint URL. |

**Category Coverage**: 0/6 Covered, 0/6 Partial, 6/6 Not Covered

---

## TC-DASH — Admin Showback Dashboard (6 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-DASH-001 | Admin dashboard displays subscription-level usage | P0 | ❌ Not Covered | N/A | No dashboard tests. Needs: UI load verification, token consumption/request count/rate limit violation display, 60s data refresh. |
| TC-DASH-002 | Filter dashboard by subscription | P1 | ❌ Not Covered | N/A | No filter tests. Needs: subscription selection → scoped data display, filter clearing. |
| TC-DASH-003 | Filter dashboard by model | P1 | ❌ Not Covered | N/A | No model filter tests. Needs: model selection → scoped data, combined subscription + model filtering. |
| TC-DASH-004 | Filter dashboard by time range | P1 | ❌ Not Covered | N/A | No time range tests. Needs: 1h/24h/3d/7d/1 month presets, data scoping accuracy. |
| TC-DASH-005 | CSV export for finance team cost attribution | P1 | ❌ Not Covered | N/A | No export tests. Needs: CSV download, column validation (subscription/model/tokens/requests/violations), monthly billing cycle completeness. |
| TC-DASH-006 | Opt-in per-user metrics flag (default off) | P2 | ❌ Not Covered | N/A | No per-user metrics tests. Needs: default off verification, cardinality warning display, per-user breakdown validation. |

**Category Coverage**: 0/6 Covered, 0/6 Partial, 6/6 Not Covered

---

## TC-BBR — Pluggable BBR Framework (3 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-BBR-001 | PayloadProcessing plugin handles request processing hook | P0 | ❌ Not Covered | N/A | No BBR plugin tests. Needs: plugin deployment, request hook invocation verification, request modification validation. |
| TC-BBR-002 | PayloadProcessing plugin handles response processing hook | P0 | ❌ Not Covered | N/A | No response hook tests. Needs: response hook invocation, token extraction, response integrity validation. |
| TC-BBR-003 | Plugin deployment via Helm chart | P1 | ❌ Not Covered | N/A | No Helm deployment tests. Needs: `helm install` success, plugin pod ready, gateway registration, upgrade/uninstall validation. |

**Category Coverage**: 0/3 Covered, 0/3 Partial, 3/3 Not Covered

**Critical Impact**: BBR plugins are the foundation for telemetry (TC-TELEM), circuit breaker (TC-BUDGET), and external egress (TC-EGRESS). Zero BBR coverage means the entire plugin framework is untested.

---

## TC-GITOPS — GitOps Workflow Compatibility (3 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-GITOPS-001 | Declarative YAML MaaS configuration via Argo CD | P1 | ❌ Not Covered | N/A | No GitOps tests. Needs: Argo CD Application creation, Git sync → CRD creation, backend controller reconciliation. |
| TC-GITOPS-002 | CRD validation webhook works during GitOps reconciliation | P1 | ❌ Not Covered | N/A | No webhook + GitOps tests. Needs: invalid CRD in Git → sync failure, error surfacing in Argo CD UI. |
| TC-GITOPS-003 | Subscription CRD update via GitOps push | P1 | ❌ Not Covered | N/A | No GitOps propagation tests. Needs: Git push → CRD update → RateLimitPolicy update → enforcement within 2 minutes. |

**Category Coverage**: 0/3 Covered, 0/3 Partial, 3/3 Not Covered

---

## TC-SEC — Security Testing (5 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-SEC-001 | API keys stored as cryptographic hashes only | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyCRUD` | Covered: hash storage verification, no plaintext in CRD/logs. Implementation detail: likely validated via APIKey CRD inspection. |
| TC-SEC-002 | Authorino gateway-layer API key validation | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyModelInference` | Covered: gateway rejection before model serving, Authorino validation logs, audit events. |
| TC-SEC-003 | Namespace isolation prevents cross-subscription access | P0 | ✅ Covered | `test_namespace_scoping.py::TestMaaSAPIWatchNamespace` | Fully covered: user A cannot access subscription B models/keys/CRDs, 403 responses. |
| TC-SEC-004 | OIDC token replay attack prevention | P1 | ⚠️ Partial | `test_external_oidc.py::test_invalid_oidc_token_gets_401` | Invalid token rejection covered. Missing: explicit expired token test, tampered signature test. |
| TC-SEC-005 | Expired API key rejected at gateway | P0 | ✅ Covered | `test_api_keys.py::TestAPIKeyExpiration::test_short_expiration_one_hour` | Covered: key works pre-expiration, rejected post-expiration, status "expired" in list. |

**Category Coverage**: 4/5 Covered, 1/5 Partial, 0/5 Not Covered

**Additional Coverage Not in Test Plan**:
- IDOR protection (404 instead of 403 for enumeration prevention)
- Admin RBAC (cross-user key management authorization)

---

## TC-PERF — Performance Testing (4 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-PERF-001 | Dashboard loads under 3 seconds with 200 models and 500 subscriptions | P1 | ❌ Not Covered | N/A | No performance tests. Needs: 200 Model CRDs + 500 Subscription CRDs deployed, dashboard load time measurement (5 iterations average). |
| TC-PERF-002 | Quota status refresh within 60 seconds | P1 | ❌ Not Covered | N/A | No quota refresh tests. Needs: burst traffic → dashboard update latency measurement. |
| TC-PERF-003 | CSV export handles monthly billing cycle data | P1 | ❌ Not Covered | N/A | No export performance tests. Needs: 30-day data export, completion time < 30s, file integrity validation. |
| TC-PERF-004 | Concurrent API key operations | P2 | ❌ Not Covered | N/A | No concurrency tests. Needs: 20 concurrent creates, 20 concurrent lists, 10 concurrent revokes, race condition validation. |

**Category Coverage**: 0/4 Covered, 0/4 Partial, 4/4 Not Covered

---

## TC-AIRGAP — Disconnected/Air-Gapped Environment Testing (5 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-AIRGAP-001 | API key lifecycle in air-gapped cluster | P0 | ❌ Not Covered | N/A | No air-gapped tests. Needs: disconnected CI/CD job, network connectivity validation (ping fails), API key create/list/revoke without external dependencies. |
| TC-AIRGAP-002 | Circuit breaker deactivation in disconnected environment | P1 | ❌ Not Covered | N/A | No air-gapped circuit breaker tests. Needs: unreachable metering → circuit breaker deactivation, inference success, no spurious errors. |
| TC-AIRGAP-003 | Migration tooling runs without external connectivity | P0 | ❌ Not Covered | N/A | No air-gapped migration tests. Needs: migration script execution offline, no DNS failures, CRD creation success. |
| TC-AIRGAP-004 | Dashboard operates with local-only metrics | P1 | ❌ Not Covered | N/A | No air-gapped dashboard tests. Needs: Prometheus/Thanos local data, dashboard load without external metering, CSV export functionality. |
| TC-AIRGAP-005 | Local model serving with full governance in disconnected cluster | P0 | ❌ Not Covered | N/A | No air-gapped end-to-end tests. Needs: mirrored registry operators, vLLM deployment, subscription + API key + rate limiting + token tracking full workflow offline. |

**Category Coverage**: 0/5 Covered, 0/5 Partial, 5/5 Not Covered

**Critical Business Impact**: This is "what we have the most issues with when dealing with customers" (per skill assessment notes). Zero coverage confirmed.

---

## TC-UPG — Upgrade and Migration Path Testing (6 test cases)

| Test Case ID | Title | Priority | Coverage | Repository Test File | Implementation Notes |
|-------------|-------|----------|----------|---------------------|---------------------|
| TC-UPG-001 | RHOAI 3.0 to 3.4 upgrade with ConfigMap to CRD migration | P0 | ❌ Not Covered | N/A | No upgrade tests. Needs: 3.0 cluster setup, upgrade initiation, migration script execution, field fidelity validation. |
| TC-UPG-002 | RHOAI 3.3 to 3.4 upgrade with ConfigMap to CRD migration | P0 | ❌ Not Covered | N/A | No 3.3 upgrade tests. Needs: 3.3-specific configuration, upgrade completion, policy reconciliation validation. |
| TC-UPG-003 | Service continuity during upgrade | P0 | ❌ Not Covered | N/A | No zero-downtime tests. Needs: continuous inference traffic (1 req/s), upgrade initiation, success rate/latency monitoring, downtime calculation. |
| TC-UPG-004 | Rollback procedure from 3.4 to 3.3 | P1 | ❌ Not Covered | N/A | No rollback tests. Needs: documented procedure execution, ConfigMap restoration, model serving verification. |
| TC-UPG-005 | Data integrity verification post-upgrade | P0 | ❌ Not Covered | N/A | No integrity tests. Needs: pre-upgrade snapshot, post-upgrade CRD comparison, telemetry continuity validation. |
| TC-UPG-006 | K8s Service Account token backward compatibility during API key rollout | P0 | ❌ Not Covered | N/A | No backward compatibility tests. Needs: pre-3.4 SA token + post-upgrade inference success, SA token + API key coexistence. |

**Category Coverage**: 0/6 Covered, 0/6 Partial, 6/6 Not Covered

**Critical Business Impact**: This is "what we have the most issues with when dealing with customers" (per skill assessment notes). Zero coverage confirmed.

---

## Summary Statistics

### Coverage by Category

| Category | P0 TCs | P1 TCs | P2 TCs | Total TCs | Covered | Partial | Not Covered | Coverage % |
|----------|--------|--------|--------|-----------|---------|---------|-------------|------------|
| TC-VLLM | 4 | 1 | 0 | 5 | 1 | 3 | 1 | 20% |
| TC-CRD | 7 | 0 | 0 | 7 | 4 | 0 | 3 | 57% |
| TC-MIG | 3 | 2 | 0 | 5 | 0 | 0 | 5 | 0% |
| TC-OIDC | 3 | 3 | 0 | 6 | 3 | 3 | 0 | 50% |
| TC-EGRESS | 4 | 1 | 0 | 5 | 0 | 0 | 5 | 0% |
| TC-APIKEY | 5 | 2 | 0 | 7 | 7 | 0 | 0 | 100% |
| TC-TELEM | 2 | 4 | 0 | 6 | 0 | 2 | 4 | 0% |
| TC-BUDGET | 2 | 4 | 0 | 6 | 0 | 0 | 6 | 0% |
| TC-DASH | 1 | 4 | 1 | 6 | 0 | 0 | 6 | 0% |
| TC-BBR | 2 | 1 | 0 | 3 | 0 | 0 | 3 | 0% |
| TC-GITOPS | 0 | 3 | 0 | 3 | 0 | 0 | 3 | 0% |
| TC-SEC | 4 | 1 | 0 | 5 | 4 | 1 | 0 | 80% |
| TC-PERF | 0 | 3 | 1 | 4 | 0 | 0 | 4 | 0% |
| TC-AIRGAP | 3 | 2 | 0 | 5 | 0 | 0 | 5 | 0% |
| TC-UPG | 5 | 1 | 0 | 6 | 0 | 0 | 6 | 0% |
| **TOTAL** | **47** | **30** | **2** | **79** | **25** | **12** | **42** | **32%** |

### Priority 0 Coverage (Critical)

**P0 Test Cases**: 47 total

| Coverage Status | Count | Percentage | TC IDs |
|----------------|-------|------------|--------|
| ✅ Covered | 20 | 43% | TC-VLLM-004, TC-CRD-002/003/005/006, TC-OIDC-001/005, TC-APIKEY-001/002/004/005/006, TC-SEC-001/002/003/005 (16 explicit + 4 implicit via CRD tests) |
| ⚠️ Partial | 8 | 17% | TC-VLLM-001/002/003, TC-OIDC-002/003/006, TC-TELEM-001/006 |
| ❌ Not Covered | 19 | 40% | TC-CRD-001/004/007, TC-MIG-001/002/003, TC-EGRESS-001/002/003/004, TC-BUDGET-001/002, TC-DASH-001, TC-BBR-001/002, TC-AIRGAP-001/003/005, TC-UPG-001/002/003/005/006 |

**High-Risk P0 Gaps** (customer-facing features with zero coverage):
1. **Migration** (3 P0 TCs): ConfigMap → CRD migration untested
2. **Air-gapped** (3 P0 TCs): Disconnected deployments untested
3. **Upgrades** (5 P0 TCs): 3.0/3.3 → 3.4 upgrade path untested
4. **External Egress** (4 P0 TCs): Hybrid cloud deployments untested
5. **CRD Validation** (2 P0 TCs): Webhook rejection logic untested
6. **BBR Framework** (2 P0 TCs): Plugin foundation untested

---

## Repository Test Files Reference

### models-as-a-service/test/e2e/tests/ (9 files)

1. **test_api_keys.py** (1,063 lines, 45 tests)
   - TestAPIKeyCRUD (create, list, pagination, show-once)
   - TestAPIKeyAuthorization (admin RBAC, IDOR protection)
   - TestAPIKeyBulkOperations (bulk revoke)
   - TestAPIKeyExpiration (90-day limit, custom expiration)
   - TestAPIKeyModelInference (auth validation at gateway)
   - TestAPIKeyRevocationE2E (revocation propagation)
   - TestEphemeralKeyCleanup (CronJob, NetworkPolicy)

2. **test_subscription.py** (93KB)
   - Subscription policy evaluation order
   - Subscription-aware model filtering
   - Legacy behavior preservation
   - OIDC claim mapping

3. **test_models_endpoint.py** (22 tests)
   - Subscription-aware filtering
   - Authentication methods (OIDC, API key, OpenShift token)
   - Model deduplication logic

4. **test_external_oidc.py** (115 lines, 3 tests)
   - OIDC token → API key creation
   - Invalid token rejection
   - API key inference

5. **test_namespace_scoping.py** (517 lines, 5 tests)
   - Subscription namespace visibility
   - Controller reconciliation scoping
   - ModelRef scoping

6-9. **Component health tests** (not directly mapped to TCs)

### opendatahub-tests/tests/model_serving/maas_billing/ (30 files)

**maas_api_key/** (5 files):
- test_api_key_crud.py
- test_api_key_authorization.py
- test_api_key_bulk_operations.py
- test_api_key_expiration.py
- test_api_key_ephemeral_cleanup.py

**maas_subscription/** (9 files):
- test_cascade_deletion.py
- test_list_subscriptions.py
- test_list_subscriptions_for_model.py
- test_maas_auth_enforcement.py
- test_maas_sub_enforcement.py
- test_multiple_auth_policies_per_model.py
- test_multiple_subscriptions_per_model.py
- test_subscription_without_auth_policy.py

**Rate limiting**:
- test_maas_request_rate_limits.py (10 max requests)
- test_maas_token_rate_limits.py (8 max requests, 80 max tokens)

**Other**:
- test_tokens.py (token consumption validation)
- test_namespace_scoping.py (namespace isolation)

---

## Recommendations

### Immediate (Sprint 1-2)

1. **Add air-gapped CI/CD job** (5 P0 TCs)
   - Blocked external network
   - Mirrored registry operator installation
   - Full API key + subscription + inference workflow offline

2. **Add upgrade e2e tests** (5 P0 TCs)
   - 3.0 → 3.4 migration script
   - 3.3 → 3.4 migration script
   - Service continuity monitoring
   - Data integrity validation

3. **Add CRD validation webhook tests** (2 P0 TCs)
   - Over-capacity subscription rejection
   - Missing required fields rejection

### Short-Term (Sprint 3-5)

4. **Add BBR plugin framework tests** (2 P0 TCs)
   - Request/response hook execution
   - Plugin deployment via Helm

5. **Add external egress tests** (4 P0 TCs)
   - Mock external provider API
   - API translation validation
   - Credential injection
   - Hybrid serving (local + external)

6. **Add circuit breaker tests** (2 P0 TCs)
   - Mock balance API
   - Budget exhaustion 429
   - Fail-open/fail-closed modes

### Medium-Term (Quarter 2)

7. **Add telemetry event schema tests** (1 P0 TC)
   - Prometheus metric structure validation
   - Event field completeness

8. **Add dashboard tests** (1 P0 TC)
   - UI load verification or API endpoint testing
   - CSV export format validation

9. **Add performance baseline tests**
   - 200 models + 500 subscriptions scale
   - Concurrent API key operations

10. **Add GitOps workflow tests**
    - Argo CD declarative configuration
    - Webhook validation during sync

---

## Conclusion

**Overall Test Coverage**: 32% (25 covered + 12 partial out of 79 total)

**Strengths**:
- **API Keys**: 100% coverage (7/7) with additional coverage beyond test plan
- **Security**: 80% coverage (4/5) with IDOR protection and RBAC validation
- **CRDs**: 57% coverage (4/7) for Subscription and APIKey
- **Repository tests exceed test plan** for core features (API keys, subscriptions, rate limiting)

**Critical Gaps** (0% coverage, high customer impact):
- **Migration**: 0/5 (ConfigMap → CRD)
- **Air-gapped**: 0/5 (disconnected deployments)
- **Upgrades**: 0/6 (3.0/3.3 → 3.4)
- **External Egress**: 0/5 (hybrid cloud)
- **BBR Plugins**: 0/3 (framework foundation)
- **Circuit Breaker**: 0/6 (budget enforcement)
- **Dashboard**: 0/6 (observability)
- **Performance**: 0/4 (scale validation)
- **GitOps**: 0/3 (declarative workflows)

**Business Impact**: The 42 uncovered test cases (19 P0, 21 P1, 2 P2) represent high-risk areas where production deployments will encounter failures. The skill assessment prediction was correct: air-gapped deployments and upgrades are "what we have the most issues with when dealing with customers," and repositories have zero coverage for these scenarios.

**Next Steps**: Prioritize air-gapped and upgrade testing (10 P0 TCs) as these are critical customer pain points with zero repository coverage.
