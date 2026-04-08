# MaaS GA Platform — Implementation Delta and Coverage Gaps

**Analysis Date**: 2026-04-08  
**Source Documents**:
- Test Plan: `maas_ga_platform/TestPlan.md` (79 test cases)
- Repository Mapping: `output/TEST_CASE_TO_REPOSITORY_MAPPING.md`
- Comprehensive Gap Analysis: `output/TEST_PLAN_VS_REPOSITORY_COMPREHENSIVE_GAP_ANALYSIS.md`

---

## Part 1: Test Cases Requiring Implementation

**Summary**: 42 test cases from the test plan have ZERO repository coverage and need implementation.

**Priority Breakdown**:
- **P0**: 19 test cases (40% of P0 tests uncovered)
- **P1**: 21 test cases (70% of P1 tests uncovered)
- **P2**: 2 test cases (100% of P2 tests uncovered)

---

### Critical P0 Test Cases — Must Implement (19 test cases)

#### Category: Migration (3 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-MIG-001** | Migrate simple single-model ConfigMap to CRD | ConfigMap → CRD migration is the foundation of 3.4 upgrade. Zero coverage means migration will fail in production. |
| **TC-MIG-002** | Migrate complex multi-model multi-tenant ConfigMap to CRD | Multi-tenant customers have complex configurations. Untested migration = data loss risk. |
| **TC-MIG-003** | Migration preserves 100% fidelity across quota values and group mappings | Quota drift = service degradation or overages. Fidelity validation is mandatory. |

**Implementation Priority**: **CRITICAL (Implement First)**  
**Blocking**: TC-UPG-001, TC-UPG-002 (upgrades depend on migration)  
**TestPlanGaps.md flags**: Migration script behavior undefined, rollback strategy missing

---

#### Category: Air-Gapped (3 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-AIRGAP-001** | API key lifecycle in air-gapped cluster | Financial/government customers require disconnected deployments. Zero coverage = production failures. |
| **TC-AIRGAP-003** | Migration tooling runs without external connectivity | Migration must work offline for air-gapped upgrades. No DNS = no migration. |
| **TC-AIRGAP-005** | Local model serving with full governance in disconnected cluster | End-to-end air-gapped workflow. Zero coverage = "what we have the most issues with when dealing with customers." |

**Implementation Priority**: **CRITICAL (Implement First)**  
**Customer Impact**: HIGH — "what we have the most issues with when dealing with customers"  
**TestPlanGaps.md flags**: Air-gapped deployment strategy undefined

---

#### Category: Upgrades (5 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-UPG-001** | RHOAI 3.0 to 3.4 upgrade with ConfigMap to CRD migration | 3.0 is an active LTS release. Untested upgrade = customer outages during 3.4 adoption. |
| **TC-UPG-002** | RHOAI 3.3 to 3.4 upgrade with ConfigMap to CRD migration | 3.3 is current GA. This is the most common upgrade path for customers. |
| **TC-UPG-003** | Service continuity during upgrade | Downtime = SLA violations. Zero-downtime claims require validation. |
| **TC-UPG-005** | Data integrity verification post-upgrade | Data corruption detection. Silent failures = production issues discovered weeks later. |
| **TC-UPG-006** | K8s Service Account token backward compatibility during API key rollout | Breaking old clients = customer regressions. Coexistence must be tested. |

**Implementation Priority**: **CRITICAL (Implement First)**  
**Customer Impact**: HIGH — "what we have the most issues with when dealing with customers"  
**TestPlanGaps.md flags**: Migration script implementation missing, service continuity behavior undefined

---

#### Category: External Egress (4 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-EGRESS-001** | External model reachable via Istio egress routing | Hybrid deployments (on-prem + cloud) require egress. Zero coverage = no cloud provider support. |
| **TC-EGRESS-002** | API key injection from labeled K8s secret | Credential management for external providers. Zero coverage = credentials leaked in logs/configs. |
| **TC-EGRESS-003** | Bidirectional API translation for external provider | OpenAI compatibility promise. Zero coverage = API mismatches in production. |
| **TC-EGRESS-004** | Combined in-cluster and out-of-cluster serving through single gateway | Unified governance across hybrid deployments. Zero coverage = governance gaps. |

**Implementation Priority**: **HIGH (Implement Second)**  
**Blocking**: External model integrations (AWS Bedrock, Azure OpenAI)  
**TestPlanGaps.md flags**: Supported providers not enumerated, API translation mappings undefined

---

#### Category: CRD Validation (2 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-CRD-001** | Create Model CRD with capacity quotas | Model CRD is the anchor for subscription quotas. Zero coverage = no capacity enforcement. |
| **TC-CRD-004** | Validation webhook rejects quota sum exceeding model capacity | Prevents over-subscription. Zero coverage = quota drift → service degradation. |
| **TC-CRD-007** | Validation webhook rejects Subscription CRD with missing required fields | Input validation prevents invalid CRDs in production. Zero coverage = undefined behavior. |

**Implementation Priority**: **HIGH (Implement Second)**  
**TestPlanGaps.md flags**: Webhook validation rules incomplete beyond quota sum check

---

#### Category: BBR Framework (2 P0 test cases)

| Test Case ID | Title | Why Critical |
|-------------|-------|--------------|
| **TC-BBR-001** | PayloadProcessing plugin handles request processing hook | BBR is the foundation for telemetry and circuit breaker. Zero coverage = plugin framework untested. |
| **TC-BBR-002** | PayloadProcessing plugin handles response processing hook | Token extraction depends on response hook. Zero coverage = telemetry broken. |

**Implementation Priority**: **HIGH (Implement Second)**  
**Blocking**: TC-TELEM-* (telemetry), TC-BUDGET-* (circuit breaker)  
**TestPlanGaps.md flags**: BBR plugin interface specification missing (ADR required)

---

### High-Priority P1 Test Cases — Should Implement (21 test cases)

#### Category: Migration (2 P1 test cases)

- **TC-MIG-004**: Migration dry-run mode previews changes without applying
- **TC-MIG-005**: Migration rollback on failure reverts partial changes

**Why needed**: Production migrations require dry-run validation and rollback capability.

**Implementation Priority**: **HIGH (Implement Second)**

---

#### Category: Air-Gapped (2 P1 test cases)

- **TC-AIRGAP-002**: Circuit breaker deactivation in disconnected environment
- **TC-AIRGAP-004**: Dashboard operates with local-only metrics

**Why needed**: Air-gapped clusters require graceful degradation when external integrations are unavailable.

**Implementation Priority**: **HIGH (Implement Second)**

---

#### Category: Upgrades (1 P1 test case)

- **TC-UPG-004**: Rollback procedure from 3.4 to 3.3

**Why needed**: Emergency rollback for failed upgrades.

**Implementation Priority**: **HIGH (Implement Second)**

---

#### Category: External Egress (1 P1 test case)

- **TC-EGRESS-005**: Egress routing failure handling for unreachable external model

**Why needed**: Graceful degradation for hybrid deployments.

**Implementation Priority**: **HIGH (Implement Second)**

---

#### Category: Telemetry (4 P1 test cases)

- **TC-TELEM-002**: Streaming response produces accurate token counts
- **TC-TELEM-003**: Telemetry delivery within 1 minute
- **TC-TELEM-004**: Telemetry activation and deactivation per gateway
- **TC-TELEM-005**: Failed telemetry emission logged for alerting

**Why needed**: Telemetry SLA validation, failure handling. Depends on TC-BBR-* completion.

**Implementation Priority**: **MEDIUM (Implement Third)**

---

#### Category: Circuit Breaker/Budget (4 P1 test cases)

- **TC-BUDGET-003**: Circuit breaker fail-open mode when metering system is unreachable
- **TC-BUDGET-004**: Circuit breaker fail-closed mode when metering system is unreachable
- **TC-BUDGET-005**: Circuit breaker activation and deactivation per gateway
- **TC-BUDGET-006**: Failed budget check calls logged for alerting

**Why needed**: Production resilience, alerting. Depends on TC-BBR-* completion.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**TestPlanGaps.md flags**: Fail-open vs fail-closed decision criteria undefined

---

#### Category: Dashboard (4 P1 test cases)

- **TC-DASH-002**: Filter dashboard by subscription
- **TC-DASH-003**: Filter dashboard by model
- **TC-DASH-004**: Filter dashboard by time range
- **TC-DASH-005**: CSV export for finance team cost attribution

**Why needed**: Admin observability, cost attribution.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**TestPlanGaps.md flags**: Prometheus metric catalog missing, CSV export format undefined

---

#### Category: BBR Framework (1 P1 test case)

- **TC-BBR-003**: Plugin deployment via Helm chart

**Why needed**: Helm is the standard deployment mechanism for BBR plugins.

**Implementation Priority**: **HIGH (Implement Second)**  
**TestPlanGaps.md flags**: Helm chart structure undefined

---

#### Category: GitOps (3 P1 test cases)

- **TC-GITOPS-001**: Declarative YAML MaaS configuration via Argo CD
- **TC-GITOPS-002**: CRD validation webhook works during GitOps reconciliation
- **TC-GITOPS-003**: Subscription CRD update via GitOps push

**Why needed**: GitOps is the preferred deployment model for production clusters.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**TestPlanGaps.md flags**: GitOps reconciliation behavior undefined

---

### Low-Priority P2 Test Cases — Nice to Have (2 test cases)

- **TC-DASH-006**: Opt-in per-user metrics flag (default off) — Cardinality explosion prevention
- **TC-PERF-004**: Concurrent API key operations — Concurrency stress testing

**Implementation Priority**: **LOW (Implement Fourth)**

---

## Part 2: Gaps in Both Test Plan and Repositories

**Summary**: Best practices analysis identified coverage gaps that NEITHER the test plan NOR the repositories address.

---

### Critical Missing Coverage (High Risk)

#### 1. **Disaster Recovery**

**What's Missing**:
- Database backup/restore testing (API key storage)
- API key recovery after control plane failure
- Subscription state recovery after ETCD corruption
- CRD restoration from backup after accidental deletion

**Why Critical**: Production clusters require DR procedures. Zero coverage = undefined recovery process.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**Recommendation**: Add 4 test cases to TC-PERF or new TC-DR category

---

#### 2. **Performance / Load Testing**

**What's Missing**:
- 1000 concurrent API keys (scale testing)
- 100 concurrent model inference requests (gateway throughput)
- Rate limiter behavior under sustained load (Limitador capacity)
- Subscription policy evaluation latency under load (controller performance)

**Why Critical**: Production scalability requirements undefined. Zero coverage = capacity planning guesswork.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**Recommendation**: Expand TC-PERF from 4 to 8 test cases

**Existing TC-PERF-004 is similar but insufficient**: "Concurrent API key operations" (P2) tests 20 concurrent operations, not 1000. Missing: sustained load, latency SLA validation, resource limit testing.

---

#### 3. **Chaos Engineering**

**What's Missing**:
- Network partition during inference (gateway ↔ model connectivity loss)
- Control plane restart during API key creation (state consistency)
- Limitador crash during rate-limited request (graceful degradation)
- ETCD split-brain scenario (CRD consistency)

**Why Critical**: Production resilience untested. Zero coverage = undefined failure modes.

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Add new TC-CHAOS category with 4 test cases

---

#### 4. **Security Hardening**

**What's Missing**:
- API key brute-force protection (rate limiting on `/v1/api-keys`)
- SQL injection prevention (if using relational storage)
- Injection attacks on API key name field (XSS, command injection)
- JWT token expiration and rotation testing (OIDC tokens)

**Why Critical**: Security vulnerabilities = CVEs in production. Zero coverage = attack surface undefined.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**Recommendation**: Expand TC-SEC from 5 to 9 test cases

**Existing TC-SEC-004 is similar but insufficient**: "OIDC token replay attack prevention" (P1) is partial coverage. Missing: token rotation, brute-force protection, injection attacks.

---

#### 5. **Multi-Tenancy Isolation**

**What's Missing**:
- Namespace quota enforcement (prevent tenant A from exhausting cluster resources)
- Resource limits per subscription (CPU/memory capping)
- Cross-tenant data leakage validation (ensure user A cannot see user B's keys beyond IDOR protection)

**Why Critical**: Multi-tenant security = production requirement. Zero coverage = tenant isolation untested.

**Implementation Priority**: **MEDIUM (Implement Third)**  
**Recommendation**: Expand TC-SEC with 3 multi-tenancy test cases

**Existing TC-SEC-003 is similar but insufficient**: "Namespace isolation prevents cross-subscription access" (P0) validates authorization, not resource isolation or quota enforcement.

---

#### 6. **Observability**

**What's Missing**:
- OpenTelemetry trace propagation validation (request ID across gateway → model)
- Distributed tracing end-to-end (client → gateway → model → response)
- Prometheus metric cardinality validation (prevent cardinality explosion)
- Alert rule validation (ensure alerts fire correctly)

**Why Critical**: Production debugging requires observability. Zero coverage = blind operations.

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Add new TC-OBSERVABILITY category with 4 test cases

---

#### 7. **Compliance / Audit**

**What's Missing**:
- Audit log completeness (all API key operations logged)
- PII handling in logs (ensure API keys are redacted)
- Retention policy enforcement (90-day expiration, telemetry retention)

**Why Critical**: Compliance requirements (SOC 2, HIPAA) = production blocker. Zero coverage = compliance gaps.

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Expand TC-SEC with 3 compliance test cases

---

### Medium Priority Missing Coverage (Medium Risk)

#### 8. **Edge Cases**

**What's Missing**:
- Empty model list (user with subscription but no models)
- Subscription with zero models (invalid CRD)
- API key creation with Unicode/emoji in name field
- Extremely long API key name (1000 characters)

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Add to TC-APIKEY as TC-APIKEY-008 through TC-APIKEY-011

---

#### 9. **Idempotency**

**What's Missing**:
- Create API key with same name twice (should return different keys or error?)
- Revoke already-revoked key (currently returns 404, validated in repos, but not in test plan)

**Note**: Repository test `test_api_keys.py::TestAPIKeyRevocationE2E::test_double_revoke_returns_404` covers double revoke. Test plan TC-APIKEY-* does NOT include this scenario.

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Document repository test TC-APIKEY-008 as "already covered, no new implementation needed"

---

#### 10. **Configuration Drift**

**What's Missing**:
- MaxExpirationDays changed while keys exist (do existing keys get updated?)
- Subscription deleted while API key still references it (cascade behavior)

**Implementation Priority**: **LOW (Implement Fourth)**  
**Recommendation**: Add to TC-CRD as TC-CRD-008, TC-CRD-009

---

### Low Priority Missing Coverage (Low Risk, Nice to Have)

#### 11. **Documentation Testing**

**What's Missing**:
- All README examples execute successfully
- API documentation examples are syntactically correct

**Implementation Priority**: **LOWEST (Not e2e tests)**  
**Recommendation**: Add to CI/CD pipeline, not e2e tests

---

#### 12. **Developer Experience**

**What's Missing**:
- Local development setup works (minikube, kind, etc.)
- Error messages are actionable (clear guidance on how to fix)

**Implementation Priority**: **LOWEST (Not e2e tests)**  
**Recommendation**: Add to onboarding documentation, not e2e tests

---

## Summary Tables

### Part 1: Test Cases Requiring Implementation (42 total)

| Priority | Count | Implementation Order |
|----------|-------|---------------------|
| **P0 Critical** (Migration, Air-gapped, Upgrades) | 11 | **1st - CRITICAL** |
| **P0 High** (External Egress, CRD, BBR) | 8 | **2nd - HIGH** |
| **P1 High** (Migration rollback, Air-gapped degradation, Upgrades rollback) | 5 | **2nd - HIGH** |
| **P1 Medium** (Telemetry, Circuit Breaker, Dashboard) | 13 | **3rd - MEDIUM** |
| **P1 Low** (GitOps) | 3 | **3rd - MEDIUM** |
| **P2** (Dashboard per-user metrics, Performance concurrency) | 2 | **4th - LOW** |

---

### Part 2: Gaps in Both Test Plan and Repositories (26+ test cases needed)

| Gap Category | Test Cases Needed | Priority | Implementation Order |
|--------------|-------------------|----------|---------------------|
| **Disaster Recovery** | 4 | High | **3rd - MEDIUM** |
| **Performance / Load Testing** | 4 (expand TC-PERF) | High | **3rd - MEDIUM** |
| **Security Hardening** | 4 (expand TC-SEC) | High | **3rd - MEDIUM** |
| **Multi-Tenancy Isolation** | 3 (expand TC-SEC) | High | **3rd - MEDIUM** |
| **Chaos Engineering** | 4 (new TC-CHAOS) | Medium | **4th - LOW** |
| **Observability** | 4 (new TC-OBSERVABILITY) | Medium | **4th - LOW** |
| **Compliance / Audit** | 3 (expand TC-SEC) | Medium | **4th - LOW** |
| **Edge Cases** | 4 (expand TC-APIKEY) | Medium | **4th - LOW** |
| **Idempotency** | 2 (expand TC-APIKEY, TC-CRD) | Medium | **4th - LOW** |
| **Configuration Drift** | 2 (expand TC-CRD) | Medium | **4th - LOW** |
| **Documentation / DevEx** | N/A (not e2e tests) | Low | **Not e2e tests** |

**Total Estimated**: 34 new test cases (26 best practices + 8 edge cases/idempotency/config drift)

---

## Implementation Roadmap (Priority Order)

### Priority 1: CRITICAL (Implement First)

**Focus**: Customer pain points — air-gapped, upgrades, migration

**Test Cases** (11 P0):
1. **TC-MIG-001, TC-MIG-002, TC-MIG-003** — ConfigMap → CRD migration fidelity
2. **TC-AIRGAP-001, TC-AIRGAP-003, TC-AIRGAP-005** — Air-gapped end-to-end workflow
3. **TC-UPG-001, TC-UPG-002, TC-UPG-003, TC-UPG-005, TC-UPG-006** — Upgrade path validation (3.0/3.3 → 3.4)

**Why Priority 1**: "What we have the most issues with when dealing with customers" — air-gapped deployments and upgrades are the highest customer pain points with ZERO current coverage.

**Prerequisites (Blockers)**:
- Migration script implementation (currently missing per TestPlanGaps.md)
- Air-gapped CI/CD job setup (blocked external network access)
- RHOAI 3.0 and 3.3 test clusters for upgrade validation

---

### Priority 2: HIGH (Implement Second)

**Focus**: External integrations, validation framework, rollback procedures

**Test Cases** (13 total: 8 P0 + 5 P1):

**P0 Test Cases** (8):
1. **TC-EGRESS-001, TC-EGRESS-002, TC-EGRESS-003, TC-EGRESS-004** — External model egress (hybrid cloud)
2. **TC-CRD-001, TC-CRD-004, TC-CRD-007** — CRD validation webhooks
3. **TC-BBR-001, TC-BBR-002** — BBR plugin framework (foundation for telemetry + circuit breaker)

**P1 Test Cases** (5):
1. **TC-MIG-004, TC-MIG-005** — Migration dry-run and rollback
2. **TC-AIRGAP-002, TC-AIRGAP-004** — Air-gapped graceful degradation
3. **TC-UPG-004** — Upgrade rollback procedure
4. **TC-EGRESS-005** — External provider failure handling
5. **TC-BBR-003** — BBR Helm chart deployment

**Why Priority 2**: BBR framework is foundational for telemetry and circuit breaker (blocks Priority 3). External egress enables hybrid deployments. Validation webhooks prevent production misconfigurations.

**Prerequisites (Blockers)**:
- BBR plugin interface spec (ADR required per TestPlanGaps.md)
- External provider API mocks (AWS Bedrock, Azure OpenAI)
- Validation webhook implementation

---

### Priority 3: MEDIUM (Implement Third)

**Focus**: Telemetry, circuit breaker, dashboard, production readiness gaps

**Test Cases** (31 total: 16 P1 + 15 best practices):

**P1 Test Cases** (16):
1. **TC-TELEM-002, TC-TELEM-003, TC-TELEM-004, TC-TELEM-005** — Telemetry SLA and failure handling
2. **TC-BUDGET-003, TC-BUDGET-004, TC-BUDGET-005, TC-BUDGET-006** — Circuit breaker resilience (fail-open/fail-closed)
3. **TC-DASH-002, TC-DASH-003, TC-DASH-004, TC-DASH-005** — Dashboard filtering and CSV export
4. **TC-GITOPS-001, TC-GITOPS-002, TC-GITOPS-003** — GitOps workflows (declarative deployment)

**Best Practices Gaps** (15):
1. **Disaster Recovery** (4 test cases) — Database backup/restore, control plane failure recovery
2. **Performance / Load Testing** (4 test cases) — 1000 concurrent keys, 100 concurrent requests, sustained load
3. **Security Hardening** (4 test cases) — Brute-force protection, injection attacks, JWT rotation
4. **Multi-Tenancy Isolation** (3 test cases) — Namespace quota enforcement, resource limits, cross-tenant leakage

**Why Priority 3**: Production observability (dashboard, telemetry) and resilience (circuit breaker, DR) are critical for GA readiness. Depends on Priority 2 BBR framework completion.

**Prerequisites (Blockers)**:
- BBR plugin framework complete (Priority 2 dependency)
- Dashboard UI implementation (Prometheus queries, CSV export)
- GitOps setup (Argo CD or Flux)

---

### Priority 4: LOW (Implement Fourth)

**Focus**: Edge cases, chaos engineering, observability, compliance

**Test Cases** (21 total: 2 P2 + 19 best practices):

**P2 Test Cases** (2):
1. **TC-DASH-006** — Per-user metrics opt-in (cardinality explosion prevention)
2. **TC-PERF-004** — Concurrent API key operations

**Best Practices Gaps** (19):
1. **Chaos Engineering** (4 test cases) — Network partition, control plane restart, component crash
2. **Observability** (4 test cases) — OpenTelemetry traces, distributed tracing, metric cardinality, alerts
3. **Compliance / Audit** (3 test cases) — Audit logs, PII redaction, retention enforcement
4. **Edge Cases** (4 test cases) — Empty model list, Unicode in names, extremely long names
5. **Idempotency** (2 test cases) — Duplicate creates, double revoke (already covered in repos)
6. **Configuration Drift** (2 test cases) — Config changes, cascade behavior

**Why Priority 4**: Production hardening and edge cases. Important for mature product quality but lower priority than customer pain points (Priority 1) and core features (Priority 2-3).

**Prerequisites**: None (all independent tests)

---

## Recommendations

### For Test Plan Updates

1. **Add missing test cases** from Part 2 (Best Practices Gaps) to test plan categories:
   - Expand TC-PERF from 4 to 8 test cases (add load testing)
   - Expand TC-SEC from 5 to 15 test cases (add security hardening, multi-tenancy, compliance)
   - Add new TC-CHAOS category (4 test cases)
   - Add new TC-OBSERVABILITY category (4 test cases)
   - Add new TC-DR category (4 test cases)

2. **Document repository coverage** that test plan missed:
   - Ephemeral key cleanup (repository has tests, plan doesn't)
   - IDOR protection (repository has tests, plan doesn't)
   - Double revoke handling (repository has tests, plan doesn't)
   - Bulk operations (repository has tests, plan partially covers)

3. **Update TestPlanGaps.md** as prerequisites are resolved:
   - Remove gap when BBR plugin interface spec (ADR) is published
   - Remove gap when migration script is implemented
   - Remove gap when external provider list is finalized

---

### For Repository Implementation

1. **Priority 1 (CRITICAL) blockers** (must resolve before implementing tests):
   - Migration script must exist before TC-MIG-* can be tested
   - Air-gapped CI/CD job must be configured before TC-AIRGAP-* can run
   - RHOAI 3.0/3.3 test clusters must be provisioned before TC-UPG-* can execute

2. **Priority 2 (HIGH) blockers**:
   - BBR plugin interface spec (ADR) must be published before TC-BBR-* can be implemented
   - External provider API mocks must exist before TC-EGRESS-* can be tested
   - Validation webhooks must be implemented before TC-CRD-004/007 can be tested

3. **Priority 3 (MEDIUM) dependencies**:
   - Dashboard UI must be implemented before TC-DASH-* can be tested
   - GitOps integration must be configured before TC-GITOPS-* can be tested
   - Circuit breaker depends on BBR framework (Priority 2 completion)
   - Telemetry depends on BBR framework (Priority 2 completion)

---

## Final Summary

**Test Plan Coverage**: 79 test cases defined  
**Repository Coverage**: 25 fully covered (32%), 12 partially covered (15%), 42 not covered (53%)

**Implementation Required**:
- **Part 1**: 42 test cases from test plan need implementation (19 P0, 21 P1, 2 P2)
- **Part 2**: 34+ test cases missing from both test plan and repositories (best practices gaps)

**Total Test Cases Needed**: ~76 test cases (42 from plan + 34 from gaps)

**Customer Impact**: The 11 P0 critical test cases (migration, air-gapped, upgrades) represent "what we have the most issues with when dealing with customers" and must be implemented first (Priority 1: CRITICAL).

**Business Risk**: 40% of P0 test cases have zero coverage. Production deployments in air-gapped environments and upgrades from 3.0/3.3 are untested, representing the highest customer risk.
