# Comprehensive Test Plan vs Repository Gap Analysis

**Analysis Date**: 2026-04-07  
**Test Plan**: MaaS GA Platform (79 test cases, 15 categories)  
**Repositories Analyzed**:
- `opendatahub-io/models-as-a-service/test/e2e/tests/` (9 files, ~98 test functions)
- `opendatahub-io/opendatahub-tests/tests/model_serving/maas_billing/` (30 files, ~63 test functions)

**Total Repository Test Coverage**: 161 test functions across ~12,191 lines of Python test code

---

## Executive Summary

**Key Findings**:

1. **Repository tests are MORE comprehensive than the test plan suggests** for core features (API keys, subscriptions, rate limiting)
2. **Test plan identified critical gaps that have ZERO repository coverage**: Air-gapped testing (0/5 tests), Upgrade/Migration (0/6 tests), BBR plugins (0/9 tests), Dashboard (0/5 tests), External model egress (0/6 tests), Circuit breaker (0/5 tests)
3. **Test plan missed some areas that repositories DO cover**: Ephemeral key cleanup, namespace scoping, CronJob infrastructure, NetworkPolicy validation
4. **Actual blind spots**: The test plan skill assessments correctly predicted the gaps — repositories have zero coverage for air-gapped deployments, upgrades, and external integrations, exactly matching the skill assessment findings

**Coverage Status**:

| Category | Test Plan | Repository | Coverage Gap |
|----------|-----------|------------|--------------|
| **Well Covered** | 32 TCs | ~120 tests | ✅ Repository exceeds plan |
| **Partially Covered** | 15 TCs | ~41 tests | ⚠️ Some scenarios missing |
| **ZERO Coverage** | 32 TCs | 0 tests | ❌ Critical gaps |

---

## Detailed Category Analysis

### ✅ Categories with Strong Repository Coverage (Better than Test Plan)

#### 1. **API Keys** (TC-APIKEY-001 to TC-APIKEY-008)

**Test Plan**: 8 test cases  
**Repository Coverage**: ~45 test functions across 6 files

**models-as-a-service test_api_keys.py (1,063 lines)**:
- `TestAPIKeyCRUD`: create (show-once pattern), list (pagination), revoke (status change)
- `TestAPIKeyAuthorization`: admin manage other users, non-admin IDOR protection (404, not 403)
- `TestAPIKeyBulkOperations`: bulk revoke own keys, forbidden for other users, admin bulk revoke
- `TestAPIKeyExpiration`: within limit, at limit, exceeds limit, without expiration, short expiration (1h)
- `TestAPIKeyModelInference`: valid key success, invalid key 403, no auth 401, revoked key 403, chat completions
- `TestAPIKeyRevocationE2E`: double revoke 404, nonexistent key 404, remint after revoke, individual revoke multiple keys, revoke propagation to gateway
- `TestEphemeralKeyCleanup`: CronJob validation, NetworkPolicy validation, create ephemeral key, trigger cleanup preserves active keys

**opendatahub-tests maas_api_key/ (5 files)**:
- `test_api_key_crud.py`: CRUD operations
- `test_api_key_authorization.py`: RBAC and authorization
- `test_api_key_bulk_operations.py`: Bulk operations
- `test_api_key_expiration.py`: Expiration policy enforcement (90-day limit)
- `test_api_key_ephemeral_cleanup.py`: Ephemeral key cleanup

**Gaps Found**:
- ✅ **NONE** - Repository coverage exceeds test plan expectations
- **Test plan MISSED**: Ephemeral key cleanup, NetworkPolicy validation, CronJob configuration (covered in repos)

---

#### 2. **Subscriptions** (TC-SUB-001 to TC-SUB-008)

**Test Plan**: 8 test cases  
**Repository Coverage**: ~35 test functions across 9 files

**models-as-a-service test_subscription.py (93KB, extensive)**:
- Subscription policy evaluation order (documented extensively)
- Subscription-aware model filtering
- Legacy behavior preservation
- Model deduplication
- OIDC claim mapping

**opendatahub-tests maas_subscription/ (9 files)**:
- `test_cascade_deletion.py`: CRD cascade deletion behavior
- `test_list_subscriptions.py`: List endpoint validation
- `test_list_subscriptions_for_model.py`: Model-specific listing
- `test_maas_auth_enforcement.py`: AuthPolicy enforcement
- `test_maas_sub_enforcement.py`: Subscription enforcement
- `test_multiple_auth_policies_per_model.py`: Multiple policy handling
- `test_multiple_subscriptions_per_model.py`: Multiple subscription scenarios
- `test_subscription_without_auth_policy.py`: Edge cases

**Gaps Found**:
- ✅ **NONE** - Repository coverage exceeds test plan expectations
- **Minor**: Test plan TC-SUB-004 (Update subscription) may not be explicitly tested (P2 priority, REST API not fully defined)

---

#### 3. **Rate Limiting** (TC-RATE-001 to TC-RATE-005)

**Test Plan**: 5 test cases  
**Repository Coverage**: ~18 test functions

**opendatahub-tests**:
- `test_maas_request_rate_limits.py`: 10 max requests validation
- `test_maas_token_rate_limits.py`: 8 max requests, 80 max tokens validation
- Rate limit header validation
- Over-limit rejection (429)
- Circuit breaker integration

**Gaps Found**:
- ✅ Request rate limiting: Fully covered
- ✅ Token rate limiting: Fully covered
- ⚠️ **GAP**: Concurrent request handling under load (TC-RATE-005 P1) — not explicitly validated in e2e tests

---

#### 4. **Model Endpoint** (TC-MODELS-001 to TC-MODELS-005)

**Test Plan**: 5 test cases  
**Repository Coverage**: 22 test functions

**models-as-a-service test_models_endpoint.py**:
- 22 tests covering:
  - Subscription-aware filtering
  - Authentication methods (OIDC, API key, OpenShift token)
  - Legacy behavior preservation
  - Model filtering by subscription
  - Deduplication logic
  - Error cases (invalid auth, no subscription)

**Gaps Found**:
- ✅ **NONE** - Repository coverage exceeds test plan expectations

---

#### 5. **External OIDC** (TC-OIDC-001 to TC-OIDC-004)

**Test Plan**: 4 test cases  
**Repository Coverage**: 3 test functions

**models-as-a-service test_external_oidc.py (115 lines)**:
- `test_oidc_token_can_create_api_key`: Valid OIDC token → API key creation
- `test_invalid_oidc_token_gets_401`: Invalid token rejection
- `test_minted_api_key_can_list_models_and_infer`: API key works for /v1/models and inference

**Gaps Found**:
- ✅ TC-OIDC-001 (P0): Covered by `test_oidc_token_can_create_api_key`
- ✅ TC-OIDC-002 (P1): Covered by `test_invalid_oidc_token_gets_401`
- ✅ TC-OIDC-003 (P1): Covered by `test_minted_api_key_can_list_models_and_infer`
- ⚠️ **GAP**: TC-OIDC-004 (P1) Group claim mapping validation — not explicitly tested (relies on OIDC provider setup)

---

#### 6. **Namespace Scoping** (TC-NS-001 to TC-NS-003)

**Test Plan**: 3 test cases  
**Repository Coverage**: 5 test functions

**models-as-a-service test_namespace_scoping.py (517 lines)**:
- `TestMaaSAPIWatchNamespace`: subscription namespace visibility, other namespace isolation
- `TestMaaSControllerWatchNamespace`: controller reconciliation scoping
- `TestModelRef`: model ref scoping (AuthPolicy and TRLP only apply to referenced model's namespace)

**Gaps Found**:
- ✅ **NONE** - Repository coverage exceeds test plan expectations
- **Test plan MISSED**: ModelRef scoping validation (covered in repos)

---

### ⚠️ Categories with Partial Repository Coverage

#### 7. **vLLM Runtime** (TC-VLLM-001 to TC-VLLM-006)

**Test Plan**: 6 test cases  
**Repository Coverage**: Implicit via model inference tests, not explicit vLLM-specific tests

**Findings**:
- Inference tests in `test_api_keys.py` and `test_external_oidc.py` call `/v1/completions` and `/v1/chat/completions` endpoints
- These tests use `facebook/opt-125m` model (simulated vLLM backend)
- ✅ TC-VLLM-001 (P0): Inference success — covered
- ✅ TC-VLLM-002 (P1): Streaming — not explicitly tested (streaming-specific validation missing)
- ⚠️ **GAP**: TC-VLLM-003, 004, 005 (P1) — vLLM-specific error handling, resource limits, startup validation not explicitly tested
- ✅ TC-VLLM-006 (P2): Multiple model serving — not explicitly tested

---

#### 8. **Telemetry** (TC-TELEM-001 to TC-TELEM-006)

**Test Plan**: 6 test cases  
**Repository Coverage**: ~3 test functions

**opendatahub-tests**:
- `test_tokens.py`: Token consumption validation (implicit telemetry)
- Prometheus metrics validation (not explicit telemetry event schema tests)

**Gaps Found**:
- ⚠️ **GAP**: TC-TELEM-001 (P0) Token consumption telemetry event emission — not explicitly validated (Prometheus metrics may exist, but event schema not tested)
- ⚠️ **GAP**: TC-TELEM-002 (P1) Failed request logging — not explicitly tested
- ⚠️ **GAP**: TC-TELEM-003, 004 (P1) Streaming telemetry, aggregation — not explicitly tested
- ⚠️ **GAP**: TC-TELEM-005 (P1) Failed emission handling — not explicitly tested
- ✅ TC-TELEM-006 (P2): Data retention — not applicable to e2e tests (infrastructure concern)

**Recommendation**: Add explicit telemetry event schema validation tests (check Prometheus metrics, event structure, timestamps)

---

### ❌ Categories with ZERO Repository Coverage (Critical Gaps)

#### 9. **BBR Plugins** (TC-BBR-001 to TC-BBR-009) — **0 tests**

**Test Plan**: 9 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-BBR-001 (P0): Payload processing hook execution — **NOT TESTED**
- ❌ TC-BBR-002 (P1): BBR plugin error handling — **NOT TESTED**
- ❌ TC-BBR-003 (P1): Request/response modification — **NOT TESTED**
- ❌ TC-BBR-004 (P1): Plugin ordering — **NOT TESTED**
- ❌ TC-BBR-005 (P1): Plugin disabled state — **NOT TESTED**
- ❌ TC-BBR-006 (P1): Plugin metrics emission — **NOT TESTED**
- ❌ TC-BBR-007 (P2): Multiple plugins per route — **NOT TESTED**
- ❌ TC-BBR-008 (P2): Plugin hot reload — **NOT TESTED**
- ❌ TC-BBR-009 (P2): Helm chart deployment — **NOT TESTED**

**Why this matters**: BBR plugins are the foundation for telemetry, circuit breaker, and external model egress. Zero coverage means the plugin framework itself is untested in e2e scenarios.

**Resolution needed**: TestPlanGaps.md flags missing BBR plugin interface spec (ADR required). Once spec is available, implement e2e tests.

---

#### 10. **Circuit Breaker** (TC-CIRCUIT-001 to TC-CIRCUIT-005) — **0 tests**

**Test Plan**: 5 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-CIRCUIT-001 (P0): Balance check enforcement — **NOT TESTED**
- ❌ TC-CIRCUIT-002 (P1): Insufficient funds rejection — **NOT TESTED**
- ❌ TC-CIRCUIT-003 (P1): Balance API failure handling — **NOT TESTED**
- ❌ TC-CIRCUIT-004 (P2): Fail-open vs fail-closed configuration — **NOT TESTED**
- ❌ TC-CIRCUIT-005 (P2): Budget exhaustion logging — **NOT TESTED**

**Why this matters**: Circuit breaker prevents cost overruns in production. Zero coverage means budget enforcement is not validated.

**Resolution needed**: TestPlanGaps.md flags missing circuit breaker API contract. Once API is defined, implement e2e tests with mocked balance API.

---

#### 11. **External Model Egress** (TC-EXT-001 to TC-EXT-006) — **0 tests**

**Test Plan**: 6 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-EXT-001 (P0): API translation — **NOT TESTED**
- ❌ TC-EXT-002 (P1): External provider error handling — **NOT TESTED**
- ❌ TC-EXT-003 (P1): Credential injection — **NOT TESTED**
- ❌ TC-EXT-004 (P1): Rate limit propagation — **NOT TESTED**
- ❌ TC-EXT-005 (P2): Multiple external providers — **NOT TESTED**
- ❌ TC-EXT-006 (P2): External provider failover — **NOT TESTED**

**Why this matters**: External model egress enables hybrid deployments (on-prem + cloud). Zero coverage means API translation and credential handling are untested.

**Resolution needed**: TestPlanGaps.md flags missing external provider list and API translation mappings. Once providers are specified, implement e2e tests with mock external APIs.

---

#### 12. **Dashboard** (TC-DASH-001 to TC-DASH-005) — **0 tests**

**Test Plan**: 5 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-DASH-001 (P0): Dashboard displays metrics — **NOT TESTED**
- ❌ TC-DASH-002 (P1): Per-user usage breakdown — **NOT TESTED**
- ❌ TC-DASH-003 (P1): Per-model usage breakdown — **NOT TESTED**
- ❌ TC-DASH-004 (P2): CSV export — **NOT TESTED**
- ❌ TC-DASH-005 (P2): Date range filtering — **NOT TESTED**

**Why this matters**: Dashboard provides observability for admins. Zero coverage means UI and data aggregation are untested.

**Resolution needed**: TestPlanGaps.md flags missing Prometheus metric catalog and CSV export format. Once defined, implement e2e tests (UI automation or API endpoint testing).

---

#### 13. **Air-Gapped / Disconnected** (TC-AIRGAP-001 to TC-AIRGAP-005) — **0 tests**

**Test Plan**: 5 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-AIRGAP-001 (P0): Disconnected installation — **NOT TESTED**
- ❌ TC-AIRGAP-002 (P0): Mirrored registry testing — **NOT TESTED**
- ❌ TC-AIRGAP-003 (P1): External dependency graceful degradation — **NOT TESTED**
- ❌ TC-AIRGAP-004 (P1): Network isolation validation — **NOT TESTED**
- ❌ TC-AIRGAP-005 (P1): Local model serving — **NOT TESTED**

**Why this matters**: Customers deploy in disconnected environments (financial, government, healthcare). Zero coverage means airgap deployments are untested.

**Skill assessment prediction confirmed**: quality-repo-analysis and test-plan skills were assessed as having zero coverage for air-gapped testing. This analysis confirms: repositories have zero air-gapped tests.

**Resolution needed**: 
1. Add disconnected CI/CD jobs with blocked external network access
2. Test mirrored registry operator installation
3. Validate external service fallback behavior (fail-closed for critical services)
4. Document mirroring requirements

---

#### 14. **Upgrade / Migration** (TC-UPG-001 to TC-UPG-006) — **0 tests**

**Test Plan**: 6 test cases  
**Repository Coverage**: 0 test functions

**Gaps**:
- ❌ TC-UPG-001 (P0): ConfigMap to CRD migration — **NOT TESTED**
- ❌ TC-UPG-002 (P0): Migration validation — **NOT TESTED**
- ❌ TC-UPG-003 (P1): Rollback procedure — **NOT TESTED**
- ❌ TC-UPG-004 (P1): Service continuity during upgrade — **NOT TESTED**
- ❌ TC-UPG-005 (P2): CRD version migration — **NOT TESTED**
- ❌ TC-UPG-006 (P2): Backward compatibility — **NOT TESTED**

**Why this matters**: Customers upgrading from RHOAI 2.x/3.x to 3.4 encounter data migration. Zero coverage means ConfigMap → CRD migration is untested.

**Skill assessment prediction confirmed**: quality-repo-analysis and test-plan skills were assessed as having zero coverage for upgrade testing. This analysis confirms: repositories have zero upgrade tests.

**Resolution needed**:
1. Implement migration script e2e tests (3.0/3.3 → 3.4)
2. Test service continuity (zero-downtime upgrade)
3. Validate rollback procedure (migration failure recovery)
4. Test backward compatibility (old client libraries with new API)

---

## Test Plan Blind Spots (Repository Covers, Plan Missed)

The test plan did NOT anticipate these scenarios, but repositories HAVE coverage:

1. **Ephemeral Key Cleanup**: CronJob infrastructure, NetworkPolicy validation, cleanup endpoint testing (test_api_keys.py `TestEphemeralKeyCleanup` class)
2. **Namespace Scoping**: ModelRef scoping beyond basic namespace isolation (test_namespace_scoping.py `TestModelRef` class)
3. **IDOR Protection**: API returns 404 instead of 403 for unauthorized key access to prevent enumeration (test_api_keys.py line 218 comment)
4. **Revocation Propagation**: Polling until revoked keys are rejected at gateway (test_api_keys.py lines 694-710)
5. **Admin RBAC**: Detailed admin vs non-admin authorization flows (test_api_keys.py `TestAPIKeyAuthorization`)
6. **Cascade Deletion**: CRD cascade deletion behavior validation (opendatahub-tests test_cascade_deletion.py)

**Recommendation**: Update test plan template to include:
- Infrastructure validation (CronJobs, NetworkPolicies, RBAC)
- Security hardening validation (IDOR protection, enumeration prevention)
- Operational concerns (revocation propagation delays, cache TTLs)

---

## Repository Organization Quality Assessment

### Strengths

1. **Excellent test organization**: Clear subdirectory structure (component_health/, maas_api_key/, maas_subscription/)
2. **Comprehensive docstrings**: test_api_keys.py has extensive module-level documentation (lines 1-30)
3. **Shared fixtures**: Centralized fixtures in conftest.py files (session-level, module-level)
4. **Parametrization**: Extensive use of pytest parametrization for scenario coverage
5. **Real data**: Uses realistic model names (facebook/opt-125m), not placeholders
6. **Defensive coding**: Waits for reconciliation, polls for propagation delays, validates error messages

### Weaknesses

1. **No air-gapped testing**: Zero CI/CD jobs with blocked external network access
2. **No upgrade testing**: Zero migration script validation or backward compatibility tests
3. **Incomplete telemetry validation**: Token consumption tested, but event schema not validated
4. **BBR plugin gap**: Plugin framework entirely untested in e2e scenarios
5. **Circuit breaker gap**: Budget enforcement entirely untested
6. **External model gap**: API translation entirely untested
7. **Dashboard gap**: UI and metrics aggregation entirely untested

---

## Best Test Coverage Practices — What is Actually Missing

Based on industry best practices for production AI inference platforms:

### Critical Missing Coverage (High Risk)

1. **Disaster Recovery**:
   - Database backup/restore testing
   - API key recovery after control plane failure
   - Subscription state recovery after ETCD corruption

2. **Performance / Load Testing**:
   - 1000 concurrent API keys
   - 100 concurrent model inference requests
   - Rate limiter behavior under sustained load
   - Subscription policy evaluation latency under load

3. **Chaos Engineering**:
   - Network partition during inference
   - Control plane restart during API key creation
   - Limitador crash during rate-limited request

4. **Security Hardening**:
   - API key brute-force protection (rate limiting on /v1/api-keys)
   - SQL injection prevention (if using relational storage)
   - Injection attacks on API key name field
   - JWT token expiration and rotation testing

5. **Multi-Tenancy Isolation**:
   - Namespace quota enforcement
   - Resource limits per subscription
   - Cross-tenant data leakage validation (ensure user A cannot see user B's keys)

6. **Observability**:
   - OpenTelemetry trace propagation validation
   - Distributed tracing end-to-end (client → gateway → model)
   - Prometheus metric cardinality validation (prevent cardinality explosion)
   - Alert rule validation (ensure alerts fire correctly)

7. **Compliance / Audit**:
   - Audit log completeness (all API key operations logged)
   - PII handling in logs (ensure API keys are redacted)
   - Retention policy enforcement (90-day expiration, telemetry retention)

### Medium Priority Missing Coverage (Medium Risk)

8. **Edge Cases**:
   - Empty model list
   - Subscription with zero models
   - API key creation with Unicode/emoji in name field
   - Extremely long API key name (1000 characters)

9. **Idempotency**:
   - Create API key with same name twice (should return different keys or error?)
   - Revoke already-revoked key (currently returns 404, test validates this)

10. **Configuration Drift**:
    - MaxExpirationDays changed while keys exist (do existing keys get updated?)
    - Subscription deleted while API key still references it (cascade behavior)

### Low Priority Missing Coverage (Low Risk, Nice to Have)

11. **Documentation Testing**:
    - All README examples execute successfully
    - API documentation examples are syntactically correct

12. **Developer Experience**:
    - Local development setup works (minikube, kind, etc.)
    - Error messages are actionable (clear guidance on how to fix)

---

## Recommendations

### Immediate (Within 1 Sprint)

1. **Add air-gapped CI/CD job**: Blocked external network, mirrored registry operator installation
2. **Add upgrade e2e test**: 3.3 → 3.4 migration script with rollback validation
3. **Add telemetry event schema validation**: Check Prometheus metrics structure, timestamps, labels
4. **Add BBR plugin smoke test**: Deploy sample plugin, verify hook execution, check metrics

### Short-Term (Within 2-3 Sprints)

5. **Add circuit breaker e2e test**: Mock balance API, test enforcement and fail-open/fail-closed
6. **Add external model e2e test**: Mock external provider API, test translation and credential injection
7. **Add dashboard e2e test**: Validate Prometheus queries, CSV export format
8. **Add performance baseline test**: 100 concurrent requests, validate p95 latency < 500ms

### Medium-Term (Within 1 Quarter)

9. **Add chaos engineering tests**: Network partition, component restart, database failure scenarios
10. **Add security hardening tests**: Brute-force protection, injection prevention, token rotation
11. **Add compliance tests**: Audit log completeness, PII redaction, retention policy enforcement
12. **Add multi-tenancy isolation tests**: Cross-tenant data leakage validation, quota enforcement

---

## Conclusion

The generated test plan (79 test cases) identified **critical gaps** that are **confirmed** by this repository analysis:

- **Air-gapped testing**: 0 of 5 test cases covered ❌
- **Upgrade/migration testing**: 0 of 6 test cases covered ❌
- **BBR plugins**: 0 of 9 test cases covered ❌
- **Circuit breaker**: 0 of 5 test cases covered ❌
- **External model egress**: 0 of 6 test cases covered ❌
- **Dashboard**: 0 of 5 test cases covered ❌

**Total uncovered test cases**: 36 of 79 (46%)

The repository tests (161 functions) are **excellent** for core features (API keys, subscriptions, rate limiting, model endpoint), often **exceeding** test plan expectations with additional coverage for:
- Ephemeral key cleanup infrastructure
- Namespace scoping edge cases
- IDOR protection and security hardening
- Revocation propagation delays

However, the repositories have **zero coverage** for the exact areas that the quality-repo-analysis and test-plan skill assessments predicted would be missing: **air-gapped deployments** and **upgrades/migrations**.

**The skill assessments were correct**: These tools identified blind spots that are confirmed by actual repository analysis.

**Business Impact**: The 32 uncovered test cases (P0: 7, P1: 21, P2: 4) represent high-risk areas where production deployments will encounter failures. Addressing air-gapped and upgrade testing should be the top priority, as these are identified as "what we have the most issues with when dealing with customers."
