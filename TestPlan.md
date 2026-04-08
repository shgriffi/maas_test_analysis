# MaaS GA Platform Test Plan
**QE Team – MaaS Platform Feature Set Testing**

## Document Information
- **Feature**: MaaS GA Platform (Model-as-a-Service General Availability)
- **Strategies**:
  - [RHAISTRAT-1167](https://redhat.atlassian.net/browse/RHAISTRAT-1167) — Enable vLLM Runtime Support
  - [RHAISTRAT-1117](https://redhat.atlassian.net/browse/RHAISTRAT-1117) — Subscription Model Redesign
  - [RHAISTRAT-1120](https://redhat.atlassian.net/browse/RHAISTRAT-1120) — External OIDC Support
  - [RHAISTRAT-1295](https://redhat.atlassian.net/browse/RHAISTRAT-1295) — External Model Egress
  - [RHAIRFE-1444](https://redhat.atlassian.net/browse/RHAIRFE-1444) — Token Consumption Telemetry
  - [RHAISTRAT-1201](https://redhat.atlassian.net/browse/RHAISTRAT-1201) — API Key Self-Service
  - [RHAISTRAT-1235](https://redhat.atlassian.net/browse/RHAISTRAT-1235) — Admin Showback Dashboard
  - [RHAIRFE-1443](https://redhat.atlassian.net/browse/RHAIRFE-1443) — Circuit Breaker Budget Enforcement
  - [RHAISTRAT-1320](https://redhat.atlassian.net/browse/RHAISTRAT-1320) — Pluggable BBR Framework
- **Team**: QE Team
- **Version**: 1.0
- **Last Updated**: 2026-04-07

---

## 1. Executive Summary

### 1.1 Purpose

This test plan covers the MaaS (Model-as-a-Service) GA platform feature set for RHOAI 3.4, comprising nine interdependent strategies that transform MaaS from a tech preview into a production-ready, enterprise-grade AI inference gateway. Testing validates end-to-end readiness across subscription management, authentication, model serving (local and external), billing/metering integration, governance, and administrative observability.

These features collectively enable service providers (Ethan Group ~$1M deal, Phoenix 500 GPU deployment) and enterprise customers (OCBC Bank, BRZ, Telenor) to deploy MaaS in production with multi-tenant billing, external authentication, hybrid model serving, and comprehensive governance.

### 1.2 Scope

#### In Scope (QE Team Responsibilities)

- vLLM runtime integration with MaaS gateway and governance (rate limiting, token tracking, API keys)
- MaaS dashboard checkbox functionality for vLLM deployments and UX parity with llm-d
- CRD definitions and validation webhooks for Model, Subscription, and APIKey entities
- Backend controller reconciliation of Subscription CRDs to Kuadrant RateLimitPolicy/AuthPolicy
- Migration tooling for ConfigMap → CRD conversion (3.0/3.3 → 3.4) with 100% fidelity
- OIDC token translation and BYOIDC user workflows (API key creation, subscription assignment, quota enforcement)
- Namespace isolation for BYOIDC users across customer tenants
- Istio egress routing to external model endpoints (ServiceEntry + DestinationRule)
- API key injection plugin for external provider authentication
- Bidirectional API translation between OpenAI-compatible format and provider-specific formats
- Combined serving through single gateway endpoint (in-cluster + out-of-cluster models)
- Post-request token consumption telemetry emission via BBR plugin
- SSE-aware token counting for streaming responses
- Asynchronous event delivery to external metering systems (<1 minute delay)
- API key lifecycle management (create, list, get, update metadata, revoke) via REST API
- "Show once" key generation with cryptographic hash storage
- Gateway-layer API key validation via Authorino integration
- Self-service key management UI in OpenShift AI console
- Admin usage dashboard embedded in RHOAI Dashboard page (Thanos Querier integration)
- Subscription-level and model-level usage filtering with time range selection
- CSV export mechanism for finance team cost attribution
- Opt-in per-user metrics flag with cardinality warnings (TP, default off)
- Pre-request budget enforcement via BBR plugin querying external metering system
- HTTP 429 responses with structured error bodies when budget exhausted
- Configurable failure modes (fail-open or fail-closed) for circuit breaker
- PayloadProcessing plugin interface in Gateway API Inference Extension (IGW upstream)
- Plugin deployment via Helm charts
- GitOps compatibility (Argo CD, Flux) for all MaaS CRD-based configuration
- Air-gapped/disconnected environment support for API key management

#### Out of Scope (Other Teams)

- Pattern A vs. Pattern B authentication architecture decision itself (decision to be made by engineering)
- Mid-term migration to RHCL PolicyPlan (documented, not implemented in 3.4)
- Multi-cloud provider API translation beyond one reference provider
- Historical usage data migration from pre-3.4 systems
- Real-time streaming dashboard updates faster than 60 seconds
- Per-user metrics in GA (opt-in TP feature only, default off)
- BBR plugin configuration by file (configuration by code initially)
- Forked or downstream-only BBR framework code (all code must be merged upstream)
- Custom OAuth flows beyond standard OIDC
- Multi-cluster federated identity
- Multiple authentication backends per cluster

### 1.3 Test Objectives

1. Validate that vLLM-served models can be exposed through MaaS gateway with identical UX and governance capabilities (rate limiting, token tracking, API keys) as llm-d models
2. Verify that the CRD-based subscription model supports multi-tenancy, prevents invalid configurations via validation webhooks, and successfully migrates from ConfigMap JSON with 100% fidelity
3. Confirm that external OIDC authentication enables users to access MaaS models, create API keys, and receive proper subscription assignments without requiring OpenShift accounts
4. Demonstrate end-to-end request flow from inference gateway through Istio egress to external model providers with automated API key injection and bidirectional API translation
5. Ensure token consumption telemetry accurately captures usage (including streaming responses) and delivers structured events to external metering systems asynchronously with less than 1 minute delay
6. Validate that user-controlled API keys (permanent or with custom expiration) work for gateway authentication, support individual revocation, and provide audit trails in fully disconnected environments
7. Verify that the admin showback dashboard displays subscription-level usage (token consumption, request counts, rate limit violations) with CSV export capability and sub-3-second load times for up to 200 models and 500 subscriptions
8. Confirm that pre-request budget enforcement queries external metering systems and returns HTTP 429 responses when budgets are exhausted, with configurable failure modes and comprehensive error logging
9. Validate that the pluggable BBR framework supports request/response processing hooks and can be deployed via Helm charts with at least one plugin demonstrated end-to-end
10. Validate that air-gapped/disconnected deployments support full API key lifecycle management, local model serving, and dashboard functionality without external dependencies
11. Verify that upgrades from RHOAI 3.0 and 3.3 to 3.4 preserve 100% configuration fidelity, maintain service continuity for active inference requests, and provide rollback capability

---

## 2. Test Strategy

### 2.1 Test Levels

- **API Integration Testing** — REST API endpoint testing for API key management (create, list, revoke), external OIDC authentication, and inference gateway routing (local and external model requests)
- **Data Validation Testing** — CRD schema validation (Model, Subscription, APIKey), ConfigMap-to-CRD migration fidelity, webhook validation (quota sum checks), and token consumption telemetry event structure
- **Functional Testing** — MaaS governance (rate limiting, token tracking, quota enforcement), subscription assignment via OIDC claims, API key authentication flow, dashboard filtering/sorting/export, circuit breaker budget enforcement, and vLLM/llm-d runtime parity
- **UI Testing** — Dashboard consistency between vLLM and llm-d flows, API key self-service management UI, admin showback dashboard (stat cards, tables, CSV export), and MaaS checkbox behavior
- **Performance Testing** — Dashboard load times (target: <3s for 200 models, 500 subscriptions), quota status refresh latency (target: ≤60s), telemetry event delivery delay (target: <1 minute), and egress routing throughput
- **Security Testing** — API key cryptographic hashing, "show once" key generation, Authorino gateway-layer validation, OIDC token translation to K8s identity, namespace isolation for BYOIDC users, and air-gapped/disconnected environment support
- **Integration Testing** — Istio egress routing (ServiceEntry + DestinationRule), API translation (OpenAI-compatible ↔ provider-specific formats), external metering system integration (OpenMeter reference), BBR plugin framework with request/response hooks, and GitOps workflows (Argo CD, Flux)
- **Migration Testing** — ConfigMap JSON to CRD conversion with 100% fidelity for 3.0/3.3 upgraders

### 2.2 Test Types

- **Positive Testing** — Valid workflows for vLLM model exposure, API key creation/revocation, OIDC authentication (Azure AD, Okta), external model egress, telemetry event emission, budget checks passing, and subscription assignment
- **Negative Testing** — Invalid CRD configurations (quota sum > capacity), expired/revoked API keys, budget exhausted (HTTP 429), failed egress routing, metering system unavailable (fail-open/fail-closed modes), malformed OIDC tokens, and missing provider credentials
- **Boundary Testing** — Large datasets (CSV export for monthly billing cycle), high cardinality (opt-in per-user metrics with cardinality warning), concurrent API key operations, rate limit violation thresholds, and combined in-cluster + out-of-cluster model serving
- **Regression Testing** — Existing llm-d functionality remains intact after vLLM support, backward compatibility during ConfigMap → CRD migration, and no disruption to existing K8s Service Account token flows during API key rollout

### 2.3 Test Priorities

- **P0 (Critical)** — Core MaaS GA blockers: CRD-based subscription model working end-to-end, API key authentication functional, vLLM runtime support enabled, migration tooling producing valid CRDs, dashboard accessible to admins, and no data loss during upgrades
- **P1 (High)** — Customer-specific requirements for go-live: external OIDC integration (classic enterprise + CSP patterns), circuit breaker budget enforcement (Ethan Group hard requirement), token consumption telemetry webhook delivery, external model egress routing, and GitOps compatibility
- **P2 (Medium)** — Enhancement and optimization features: opt-in per-user metrics, dashboard load time optimization (<3s), telemetry delivery latency (<1 min), and advanced API translation for multiple providers

---

## 3. Test Environment

### 3.1 Test Cluster Configuration

- OpenShift cluster with RHOAI 3.4 installed
- KServe serving runtime support (vLLM and llm-d runtimes)
- Istio service mesh configured for egress routing
- Gateway API with Inference Extension support
- Kuadrant operator (for RateLimitPolicy and AuthPolicy)
- Authorino operator (for API key authentication at gateway layer)
- Limitador operator (for rate limiting metrics)
- Thanos Querier (for admin dashboard metrics aggregation)
- PostgreSQL or equivalent database for API key storage backend
- Prometheus with metrics scraping capability

### 3.2 Test Data Requirements

- Sample vLLM-served model deployments (at least one model)
- Sample llm-d-served model deployments (for parity testing)
- Subscription CRD YAML files (valid and invalid configurations for webhook testing)
- Model CRD YAML files with capacity quotas
- APIKey CRD YAML files
- Legacy ConfigMap JSON files (from 3.0/3.3 for migration testing)
- External model provider credentials (at least one of: Bedrock, Azure OpenAI, Vertex AI) as K8s secrets
- Sample inference requests (OpenAI-compatible format and provider-specific formats)
- Token consumption event payloads for metering system integration
- OIDC provider configuration (Azure AD or Okta test tenant)
- JWT tokens from external OIDC providers
- User and group claims for subscription assignment testing
- ServiceEntry and DestinationRule manifests for external model endpoints
- Limitador metrics sample data (for dashboard testing at scale: 200 models, 500 subscriptions)
- CSV export test data (monthly billing cycle scale)
- ConfigMap JSON files from real customer deployments (3.0, 3.3) with varying complexity (simple single-model, complex multi-model multi-tenant)
- Pre-upgrade baseline metrics and active inference workload for service continuity testing
- Air-gapped cluster configuration manifests (mirrored registries, disconnected operator catalogs)

### 3.3 Test Users

- **Cluster administrators** (cluster-admin role)
- **Namespace administrators** (namespace-admin role)
- **Platform administrators** with MaaS admin RBAC
- **Data scientists** belonging to multiple subscriptions (e.g., "Analytics Team" and "Production Apps")
- **External developers** consuming model endpoints via API keys
- **OIDC-authenticated users** (without OpenShift accounts) — corporate Azure AD users, CSP multi-tenant external customers
- **Service accounts** for gateway components
- **Unprivileged users** for quota enforcement and namespace isolation testing
- **Finance team users** for CSV export access

---

## 4. Components Under Test

### 4.1 CRD and Subscription Management

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| Model CRD | K8s CRD | Define model entities with capacity quotas | P0 |
| Subscription CRD | K8s CRD | Define subscription entities with group-to-model relationships and per-model quotas | P0 |
| APIKey CRD | K8s CRD | Define API key entities with validation | P0 |
| Subscription CRD validation webhook | K8s Webhook | Reject invalid configurations (e.g., sum of guaranteed quotas > model capacity) | P0 |
| Backend controller reconciliation | K8s Controller | Reconcile Subscription CRDs to Kuadrant RateLimitPolicy/AuthPolicy | P0 |
| ConfigMap → CRD migration script | CLI/Script | Convert 3.0/3.3 ConfigMap JSON to 3.4 Subscription CRDs with 100% fidelity | P0 |
| GitOps YAML configuration | Config Files | Declarative YAML for all MaaS configuration (Argo CD, Flux compatible) | P1 |

### 4.2 Authentication and API Keys

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| OIDC token translation | Authentication | Translate OIDC tokens to in-memory K8s identity | P0 |
| Subscription assignment via OIDC claims | Authentication | Assign subscriptions based on OIDC user/group claims | P0 |
| Quota enforcement for BYOIDC users | Gateway Policy | Enforce subscription quotas for OIDC-authenticated users | P0 |
| Namespace isolation for BYOIDC users | K8s RBAC | Ensure proper namespace isolation across customer tenants | P1 |
| API key REST API: Create | REST API | Create new API keys (permanent or with optional expiration) | P0 |
| API key REST API: List | REST API | List user's API keys | P1 |
| API key REST API: Get | REST API | Retrieve metadata for specific API key | P1 |
| API key REST API: Update | REST API | Update API key metadata | P2 |
| API key REST API: Revoke | REST API | Revoke individual API keys | P0 |
| `Authorization: Bearer <key>` | HTTP Header | Standard bearer token authentication at gateway | P0 |
| Authorino API key validation | Gateway Policy | Validate API keys at gateway layer | P0 |
| Cryptographic hash storage | Backend Storage | Store API keys as cryptographic hashes only | P0 |

### 4.3 Model Serving and Gateway

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| MaaS checkbox (vLLM deployments) | UI Component | Enable vLLM models to be exposed through MaaS gateway | P0 |
| Combined serving endpoint | Gateway Endpoint | Single gateway endpoint for in-cluster and out-of-cluster models | P0 |
| Istio ServiceEntry (external models) | Istio Config | Define external model endpoints for egress routing | P0 |
| Istio DestinationRule (external models) | Istio Config | Configure egress routing behavior for external models | P0 |
| API key injection BBR plugin | BBR Plugin | Inject provider credentials from labeled K8s secrets | P0 |
| API translation BBR plugin | BBR Plugin | Bidirectional request/response translation for at least one provider | P0 |

### 4.4 Metering, Telemetry, and Budget Enforcement

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| Token consumption telemetry BBR plugin | BBR Plugin | Extract token usage from inference responses and emit events | P0 |
| SSE-aware token counting | BBR Plugin | Accurate token counts for streaming responses | P1 |
| Metering telemetry async emission | BBR Plugin | Deliver usage events to external metering system (<1 min delay) | P0 |
| Metering event backup logging | BBR Plugin | Log events as backup if integration fails | P2 |
| Failed emission logging | BBR Plugin | Log failed emission calls for alerting | P1 |
| Pre-request budget check | BBR Plugin | Query external metering system's balance API before request | P0 |
| HTTP 429 response (budget exhausted) | Gateway Response | Deny requests with structured error body when budget exhausted | P0 |
| Circuit breaker activation/deactivation | Config | Enable/disable circuit breaker per-gateway | P1 |
| Configurable failure mode | Config | Configure circuit breaker behavior on metering system failure (fail-open/fail-closed) | P1 |

### 4.5 Dashboard and Observability

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| Admin usage dashboard page | UI Component | Embedded dashboard in RHOAI for subscription-level usage | P0 |
| Thanos Querier integration | Backend Integration | Query token consumption, request counts, rate limit violations | P0 |
| Subscription filter | UI Component | Filter dashboard by subscription | P1 |
| Model filter | UI Component | Filter dashboard by model | P1 |
| Time range filter | UI Component | Filter dashboard by time range (1h, 24h, 3d, 7d, 1 month) | P1 |
| Table with filter and sorting | UI Component | Display usage data with interactive table | P2 |
| High-level stat cards | UI Component | Display summary statistics | P2 |
| CSV export | UI Component | Export usage data for finance team | P1 |
| Opt-in per-user metrics flag | Config/Feature Flag | Enable per-user breakdown within subscriptions (TP, default off) | P2 |

### 4.6 BBR Plugin Framework

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| PayloadProcessing plugin interface | IGW API | Request and response processing hooks | P0 |
| Plugin deployment via Helm charts | Helm Chart | Deploy BBR plugins via Helm | P1 |
| Plaintext key display (show once) | UI Component | Show plaintext key only once at creation | P1 |
| Self-service key management UI | UI Component | Dashboard UI for API key lifecycle management | P1 |

### 4.7 Disconnected/Air-Gapped Environment Support

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| API key management in air-gapped clusters | Functional | Full API key lifecycle without external OIDC or metering | P0 |
| Circuit breaker deactivation for disconnected deployments | Config | Verify circuit breaker can be disabled when no metering system is reachable | P1 |
| Migration tooling execution without external connectivity | CLI/Script | ConfigMap → CRD migration runs fully offline | P0 |
| Dashboard functionality without external metering system | UI Component | Showback dashboard operates with local Prometheus/Thanos only | P1 |
| Local-only model serving (vLLM/llm-d) | Functional | MaaS governance works for local models without egress | P0 |
| Disconnected operator installation | Infrastructure | RHOAI 3.4 operators install from mirrored registries | P1 |
| Telemetry graceful degradation | BBR Plugin | Telemetry plugin logs locally when external metering unreachable | P2 |

### 4.8 Upgrade and Migration Paths

| Component | Type | Purpose | Priority |
|-----------|------|---------|----------|
| RHOAI 3.0 → 3.4 upgrade path | Upgrade | In-place upgrade with ConfigMap → CRD migration | P0 |
| RHOAI 3.3 → 3.4 upgrade path | Upgrade | In-place upgrade with ConfigMap → CRD migration | P0 |
| Service continuity during upgrade | Upgrade | Active inference requests continue during upgrade process | P0 |
| Rollback procedure from 3.4 to 3.3 | Upgrade | Documented and tested rollback path | P1 |
| Data integrity post-upgrade | Validation | Subscription quotas, API keys, telemetry continuity verified after upgrade | P0 |
| K8s Service Account token backward compatibility | Authentication | Existing SA tokens continue working during API key rollout | P0 |
| Migration dry-run mode | CLI/Script | Preview migration changes without applying them | P1 |
| Migration rollback on failure | CLI/Script | Automatic rollback if migration encounters errors | P1 |

---

## 5. Test Cases

**79 test cases** have been generated across 15 categories. Priority distribution: **P0: 47 | P1: 30 | P2: 2**.

**Test Cases Directory**: [test_cases/](test_cases/)
**Complete Test Case Index**: [test_cases/INDEX.md](test_cases/INDEX.md)

### 5.1 Test Case Organization

| Category | Test Cases | Priority Distribution |
|----------|------------|----------------------|
| TC-VLLM | 5 | P0: 4, P1: 1 |
| TC-CRD | 7 | P0: 7 |
| TC-MIG | 5 | P0: 3, P1: 2 |
| TC-OIDC | 6 | P0: 4, P1: 2 |
| TC-EGRESS | 5 | P0: 4, P1: 1 |
| TC-APIKEY | 7 | P0: 5, P1: 2 |
| TC-TELEM | 6 | P0: 3, P1: 3 |
| TC-BUDGET | 6 | P0: 2, P1: 4 |
| TC-DASH | 6 | P0: 1, P1: 4, P2: 1 |
| TC-BBR | 3 | P0: 2, P1: 1 |
| TC-GITOPS | 3 | P1: 3 |
| TC-SEC | 5 | P0: 4, P1: 1 |
| TC-PERF | 4 | P1: 3, P2: 1 |
| TC-AIRGAP | 5 | P0: 3, P1: 2 |
| TC-UPG | 6 | P0: 5, P1: 1 |

### 5.2 Test Case Naming Convention

Test cases follow the naming pattern: `TC-<CATEGORY>-<NUMBER>`

- **TC-VLLM** — vLLM runtime support and UX parity
- **TC-CRD** — CRD definitions, validation webhooks, and controller reconciliation
- **TC-MIG** — ConfigMap → CRD migration testing
- **TC-OIDC** — External OIDC authentication and BYOIDC workflows
- **TC-EGRESS** — External model egress routing and API translation
- **TC-APIKEY** — API key lifecycle management (CRUD, auth, revocation)
- **TC-TELEM** — Token consumption telemetry and metering events
- **TC-BUDGET** — Circuit breaker budget enforcement
- **TC-DASH** — Admin showback dashboard and CSV export
- **TC-BBR** — Pluggable BBR framework and plugin deployment
- **TC-GITOPS** — GitOps workflow compatibility
- **TC-SEC** — Security testing (crypto, namespace isolation, RBAC)
- **TC-PERF** — Performance testing (dashboard load, telemetry latency)
- **TC-AIRGAP** — Disconnected/air-gapped environment testing
- **TC-UPG** — Upgrade and migration path testing

---

## 6. Risks and Mitigation

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| ConfigMap → CRD migration fails to preserve 100% fidelity, breaking existing subscriptions for 3.0/3.3 upgraders | High | Medium | Develop comprehensive migration test suite with real customer ConfigMap samples; provide rollback procedure; include dry-run mode for migration script |
| OIDC token translation to in-memory K8s identity (spike required) may reveal architectural infeasibility | High | Medium | Complete spike before GA commitment; document Pattern A vs Pattern B decision in ADR; validate with upstream Kubernetes authentication patterns |
| External metering system dependency introduces tight coupling and failure modes | High | Medium | Implement configurable fail-open/fail-closed modes for circuit breaker; ensure telemetry events are logged as backup if integration fails; provide retry/backoff configuration |
| Istio egress routing to external LLM providers may face network policy, certificate, or authentication challenges | Medium | High | E2E testing with at least one external provider before GA; document firewall/proxy requirements; provide troubleshooting guide for common egress failures |
| API key cryptographic hashing implementation may have security vulnerabilities or performance issues at scale | High | Low | Security review of hashing algorithm choice; load testing with thousands of concurrent key validations; ensure pluggable storage backend for future optimization |
| Dashboard performance degrades with high cardinality (200 models, 500 subscriptions, opt-in per-user metrics) | Medium | Medium | Performance testing against target thresholds (<3s load, ≤60s refresh); implement pagination and lazy loading; warn users about cardinality impact of per-user metrics |
| BBR plugin framework upstream merge delays block dependent features (telemetry, circuit breaker) | High | Medium | Prioritize upstream PayloadProcessing interface design and review; maintain no-fork commitment; have contingency plan for downstream patch if upstream delays |
| vLLM governance (rate limiting, token tracking) behaves differently than llm-d, creating inconsistent UX | Medium | Medium | Test parity between vLLM and llm-d for all governance features; document any runtime-specific limitations; ensure dashboard UX abstracts runtime differences |
| API key "show once" pattern requires secure client-side handling; users may lose keys | Low | High | Provide clear UX warnings at key creation; document key recovery impossibility; implement key metadata (name, last-used timestamp) for user tracking |
| GitOps workflows (Argo CD, Flux) may encounter CRD validation webhook race conditions during reconciliation | Medium | Medium | Test declarative YAML workflows with common GitOps tools; ensure webhook validation errors are descriptive; provide GitOps deployment guide |
| Air-gapped/disconnected environments may lack external metering system connectivity for circuit breaker and telemetry | Medium | Low | Ensure circuit breaker can be deactivated per-gateway; document air-gapped deployment patterns; test fully disconnected API key management |
| CSV export for monthly billing cycles may time out or exhaust memory with high-volume usage data | Medium | Medium | Load testing with realistic monthly data volumes; implement streaming CSV generation if needed; document data retention limits (≥1 month) |
| API translation between OpenAI-compatible format and provider-specific formats may miss edge cases or streaming nuances | Medium | Medium | Test bidirectional translation with at least one provider before GA; document supported/unsupported features per provider; handle streaming responses explicitly |
| Subscription CRD quota validation may have complex edge cases beyond sum checks | Low | Medium | Comprehensive unit tests for webhook validation logic; integration tests with multiple subscriptions per model; clear error messages for invalid configurations |
| Multi-tenant namespace isolation for BYOIDC users may have privilege escalation vulnerabilities | High | Low | Security review of OIDC claims → K8s RBAC mapping; test cross-subscription access attempts; document least-privilege subscription design patterns |
| Upgrade from 3.0/3.3 to 3.4 may require downtime or service disruption, breaking service continuity requirements | High | Medium | Test in-place upgrade with active inference traffic; validate migration script can run concurrently with live requests; document maintenance window if required |
| Air-gapped deployments may encounter image registry, operator catalog, or dependency resolution issues | Medium | Medium | Test complete air-gapped installation from scratch; validate all container images available in disconnected registries; document mirroring requirements for RHOAI 3.4 operators |

---

## 7. Test Environment Requirements

### 7.1 Infrastructure

- Single OpenShift cluster with sufficient nodes for multi-tenant testing
- KServe operator with vLLM and llm-d serving runtimes installed
- Istio service mesh with egress gateway configured
- Gateway API operator with Inference Extension
- Kuadrant operator (RateLimitPolicy, AuthPolicy reconciliation)
- Authorino operator (gateway-layer validation)
- Limitador operator (rate limiting)
- Thanos Querier for metrics aggregation
- External OIDC provider (Azure AD or Okta test instance)
- External LLM provider endpoint (at least one of: Bedrock, Azure OpenAI, Vertex AI test account)
- External metering/billing system for webhook integration (OpenMeter as reference)
- S3-compatible storage for model artifacts
- Container registry for custom runtime images
- Disconnected OpenShift cluster with mirrored registries and operator catalogs (air-gapped testing)
- RHOAI 3.0 and 3.3 baseline clusters for upgrade path testing
- Network policy simulation for air-gapped environment validation

### 7.2 Configuration

- Gateway API InferenceGateway custom resources
- Subscription CRD instances (with per-model quotas, guaranteed vs burst limits)
- Model CRD instances
- APIKey CRD instances
- Kuadrant RateLimitPolicy and AuthPolicy resources
- Istio ServiceEntry and DestinationRule for external model egress
- K8s secrets with external provider API keys (labeled for injection)
- BBR plugin configuration files (token telemetry, circuit breaker, API translation)
- OIDC provider configuration (client ID, issuer URL, group claims mapping)
- Admission webhook configurations for CRD validation
- Feature flags for opt-in user-centric metrics (default off for TP)
- Failure mode configuration for circuit breaker (fail-open vs fail-closed)
- Catalog sources for RHOAI 3.4 operators
- ConfigMap for legacy 3.0/3.3 configurations (migration testing)

### 7.3 Test Tools

- `oc` (OpenShift CLI) for cluster management
- `kubectl` for Kubernetes resource operations
- `curl` or `httpie` for API endpoint testing (OpenAI-compatible inference requests)
- `jq` for JSON response parsing
- `istioctl` for service mesh diagnostics
- `kustomize` for GitOps deployment testing (Argo CD or Flux)
- Prometheus query tools (PromQL) for metrics validation
- CSV validation tools for export functionality
- JWT decoding tools for OIDC token inspection
- API testing framework (Pytest with requests library)
- Load testing tools for dashboard performance validation
- Log aggregation tools for BBR plugin failure logging and alerting validation
- OpenMeter CLI or API client for metering integration testing
- Migration script for ConfigMap → CRD conversion testing
- Network isolation tools (firewall rules, network policies) for air-gapped simulation
- ConfigMap comparison and diff tools for migration fidelity validation
- Active traffic generation tools for service continuity testing during upgrades

---

## 8. Appendix

### 8.1 Test Case Summary

| Category | Total | P0 | P1 | P2 |
|----------|-------|----|----|-----|
| TC-VLLM | 5 | 4 | 1 | 0 |
| TC-CRD | 7 | 7 | 0 | 0 |
| TC-MIG | 5 | 3 | 2 | 0 |
| TC-OIDC | 6 | 4 | 2 | 0 |
| TC-EGRESS | 5 | 4 | 1 | 0 |
| TC-APIKEY | 7 | 5 | 2 | 0 |
| TC-TELEM | 6 | 3 | 3 | 0 |
| TC-BUDGET | 6 | 2 | 4 | 0 |
| TC-DASH | 6 | 1 | 4 | 1 |
| TC-BBR | 3 | 2 | 1 | 0 |
| TC-GITOPS | 3 | 0 | 3 | 0 |
| TC-SEC | 5 | 4 | 1 | 0 |
| TC-PERF | 4 | 0 | 3 | 1 |
| TC-AIRGAP | 5 | 3 | 2 | 0 |
| TC-UPG | 6 | 5 | 1 | 0 |
| **Total** | **79** | **47** | **30** | **2** |

### 8.2 Component Coverage

| Component | Test Cases | Coverage |
|-----------|------------|----------|
| Model CRD | TC-CRD-001 | |
| Subscription CRD | TC-CRD-002 | |
| APIKey CRD | TC-CRD-003 | |
| Subscription CRD validation webhook | TC-CRD-004, TC-CRD-007 | |
| Backend controller reconciliation | TC-CRD-005, TC-CRD-006 | |
| ConfigMap → CRD migration script | TC-MIG-001, TC-MIG-002, TC-MIG-003 | |
| GitOps YAML configuration | TC-GITOPS-001, TC-GITOPS-002, TC-GITOPS-003 | |
| OIDC token translation | TC-OIDC-001 | |
| Subscription assignment via OIDC claims | TC-OIDC-002 | |
| Quota enforcement for BYOIDC users | TC-OIDC-003 | |
| Namespace isolation for BYOIDC users | TC-OIDC-006 | |
| API key REST API: Create | TC-APIKEY-001, TC-APIKEY-002 | |
| API key REST API: List | TC-APIKEY-003 | |
| API key REST API: Get | TC-APIKEY-007 | |
| API key REST API: Update | | |
| API key REST API: Revoke | TC-APIKEY-004, TC-APIKEY-005 | |
| Authorization: Bearer <key> | TC-APIKEY-006 | |
| Authorino API key validation | TC-SEC-002 | |
| Cryptographic hash storage | TC-SEC-001 | |
| MaaS checkbox (vLLM) | TC-VLLM-001, TC-VLLM-005 | |
| Combined serving endpoint | TC-EGRESS-004 | |
| Istio egress routing (ServiceEntry + DestinationRule) | TC-EGRESS-001 | |
| API key injection BBR plugin | TC-EGRESS-002 | |
| API translation BBR plugin | TC-EGRESS-003 | |
| Token consumption telemetry BBR plugin | TC-TELEM-001, TC-TELEM-006 | |
| SSE-aware token counting | TC-TELEM-002 | |
| Metering telemetry async emission | TC-TELEM-003 | |
| Metering event backup logging | TC-TELEM-005 | |
| Failed emission logging | TC-TELEM-005 | |
| Pre-request budget check | TC-BUDGET-001 | |
| HTTP 429 response (budget exhausted) | TC-BUDGET-002 | |
| Circuit breaker activation/deactivation | TC-BUDGET-005 | |
| Configurable failure mode (fail-open/fail-closed) | TC-BUDGET-003, TC-BUDGET-004 | |
| Admin usage dashboard page | TC-DASH-001 | |
| Thanos Querier integration | TC-DASH-001 | |
| Subscription filter | TC-DASH-002 | |
| Model filter | TC-DASH-003 | |
| Time range filter | TC-DASH-004 | |
| Table with filter and sorting | TC-DASH-002, TC-DASH-003 | |
| High-level stat cards | TC-DASH-001 | |
| CSV export | TC-DASH-005, TC-PERF-003 | |
| Opt-in per-user metrics flag | TC-DASH-006 | |
| PayloadProcessing plugin interface | TC-BBR-001, TC-BBR-002 | |
| Plugin deployment via Helm charts | TC-BBR-003 | |
| Plaintext key display (show once) | TC-APIKEY-007 | |
| Self-service key management UI | TC-APIKEY-001, TC-APIKEY-003 | |
| API key management in air-gapped clusters | TC-AIRGAP-001 | |
| Circuit breaker deactivation (disconnected) | TC-AIRGAP-002 | |
| Migration tooling (offline execution) | TC-AIRGAP-003 | |
| Dashboard (local-only operation) | TC-AIRGAP-004 | |
| Local-only model serving | TC-AIRGAP-005 | |
| Disconnected operator installation | TC-AIRGAP-005 | |
| Telemetry graceful degradation | TC-TELEM-005 | |
| RHOAI 3.0 → 3.4 upgrade path | TC-UPG-001 | |
| RHOAI 3.3 → 3.4 upgrade path | TC-UPG-002 | |
| Service continuity during upgrade | TC-UPG-003 | |
| Rollback procedure (3.4 → 3.3) | TC-UPG-004 | |
| Data integrity post-upgrade | TC-UPG-005 | |
| K8s SA token backward compatibility | TC-UPG-006 | |
| Migration dry-run mode | TC-MIG-004 | |
| Migration rollback on failure | TC-MIG-005 | |

### 8.3 Document Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-04-07 | QA Team | Initial test plan covering 9 MaaS GA strategies |
| 1.1 | 2026-04-07 | QA Team | Added disconnected/air-gapped and upgrade/migration sections after review |

---

**End of Test Plan**
