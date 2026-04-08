# MaaS GA Platform — Test Case Index

**Total Test Cases**: 79
**Priority Distribution**: P0: 47 | P1: 30 | P2: 2

**Parent Test Plan**: [TestPlan.md](../TestPlan.md)

---

## TC-VLLM — vLLM Runtime Support and UX Parity (5 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-VLLM-001](TC-VLLM-001.md) | Enable vLLM model exposure through MaaS checkbox | P0 |
| [TC-VLLM-002](TC-VLLM-002.md) | Rate limiting parity between vLLM and llm-d models | P0 |
| [TC-VLLM-003](TC-VLLM-003.md) | Token tracking parity between vLLM and llm-d models | P0 |
| [TC-VLLM-004](TC-VLLM-004.md) | API key authentication works for vLLM models | P0 |
| [TC-VLLM-005](TC-VLLM-005.md) | Dashboard UX consistency between vLLM and llm-d flows | P1 |

## TC-CRD — CRD Definitions, Validation, and Controller Reconciliation (7 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-CRD-001](TC-CRD-001.md) | Create Model CRD with capacity quotas | P0 |
| [TC-CRD-002](TC-CRD-002.md) | Create Subscription CRD with group-to-model relationships | P0 |
| [TC-CRD-003](TC-CRD-003.md) | Create APIKey CRD | P0 |
| [TC-CRD-004](TC-CRD-004.md) | Validation webhook rejects quota sum exceeding model capacity | P0 |
| [TC-CRD-005](TC-CRD-005.md) | Backend controller reconciles Subscription CRD to RateLimitPolicy | P0 |
| [TC-CRD-006](TC-CRD-006.md) | Backend controller reconciles Subscription CRD to AuthPolicy | P0 |
| [TC-CRD-007](TC-CRD-007.md) | Validation webhook rejects Subscription CRD with missing required fields | P0 |

## TC-MIG — ConfigMap to CRD Migration (5 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-MIG-001](TC-MIG-001.md) | Migrate simple single-model ConfigMap to CRD | P0 |
| [TC-MIG-002](TC-MIG-002.md) | Migrate complex multi-model multi-tenant ConfigMap to CRD | P0 |
| [TC-MIG-003](TC-MIG-003.md) | Migration preserves 100% fidelity across quota values and group mappings | P0 |
| [TC-MIG-004](TC-MIG-004.md) | Migration dry-run mode previews changes without applying | P1 |
| [TC-MIG-005](TC-MIG-005.md) | Migration rollback on failure reverts partial changes | P1 |

## TC-OIDC — External OIDC Authentication and BYOIDC Workflows (6 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-OIDC-001](TC-OIDC-001.md) | OIDC token translation to in-memory K8s identity | P0 |
| [TC-OIDC-002](TC-OIDC-002.md) | Subscription assignment via OIDC group claims | P0 |
| [TC-OIDC-003](TC-OIDC-003.md) | Quota enforcement for BYOIDC users | P0 |
| [TC-OIDC-004](TC-OIDC-004.md) | BYOIDC user creates API key | P1 |
| [TC-OIDC-005](TC-OIDC-005.md) | Reject malformed or expired OIDC token | P0 |
| [TC-OIDC-006](TC-OIDC-006.md) | Namespace isolation for BYOIDC users across tenants | P1 |

## TC-EGRESS — External Model Egress Routing and API Translation (5 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-EGRESS-001](TC-EGRESS-001.md) | External model reachable via Istio egress routing | P0 |
| [TC-EGRESS-002](TC-EGRESS-002.md) | API key injection from labeled K8s secret | P0 |
| [TC-EGRESS-003](TC-EGRESS-003.md) | Bidirectional API translation for external provider | P0 |
| [TC-EGRESS-004](TC-EGRESS-004.md) | Combined in-cluster and out-of-cluster serving through single gateway | P0 |
| [TC-EGRESS-005](TC-EGRESS-005.md) | Egress routing failure handling for unreachable external model | P1 |

## TC-APIKEY — API Key Lifecycle Management (7 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-APIKEY-001](TC-APIKEY-001.md) | Create API key with no expiration | P0 |
| [TC-APIKEY-002](TC-APIKEY-002.md) | Create API key with custom expiration | P0 |
| [TC-APIKEY-003](TC-APIKEY-003.md) | List user's API keys | P1 |
| [TC-APIKEY-004](TC-APIKEY-004.md) | Revoke individual API key without affecting other keys | P0 |
| [TC-APIKEY-005](TC-APIKEY-005.md) | Revoked API key is rejected at gateway | P0 |
| [TC-APIKEY-006](TC-APIKEY-006.md) | Bearer token authentication at gateway | P0 |
| [TC-APIKEY-007](TC-APIKEY-007.md) | Show-once key generation — plaintext not retrievable after creation | P1 |

## TC-TELEM — Token Consumption Telemetry (6 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-TELEM-001](TC-TELEM-001.md) | Token usage event emitted after inference request | P0 |
| [TC-TELEM-002](TC-TELEM-002.md) | Streaming response produces accurate token counts | P1 |
| [TC-TELEM-003](TC-TELEM-003.md) | Telemetry delivery within 1 minute | P0 |
| [TC-TELEM-004](TC-TELEM-004.md) | Telemetry activation and deactivation per gateway | P1 |
| [TC-TELEM-005](TC-TELEM-005.md) | Failed telemetry emission logged for alerting | P1 |
| [TC-TELEM-006](TC-TELEM-006.md) | Telemetry events work for both local and external model requests | P0 |

## TC-BUDGET — Circuit Breaker Budget Enforcement (6 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-BUDGET-001](TC-BUDGET-001.md) | Pre-request budget check passes with sufficient budget | P0 |
| [TC-BUDGET-002](TC-BUDGET-002.md) | Budget exhausted returns HTTP 429 with structured error body | P0 |
| [TC-BUDGET-003](TC-BUDGET-003.md) | Circuit breaker fail-open mode when metering system is unreachable | P1 |
| [TC-BUDGET-004](TC-BUDGET-004.md) | Circuit breaker fail-closed mode when metering system is unreachable | P1 |
| [TC-BUDGET-005](TC-BUDGET-005.md) | Circuit breaker activation and deactivation per gateway | P1 |
| [TC-BUDGET-006](TC-BUDGET-006.md) | Failed budget check calls logged for alerting | P1 |

## TC-DASH — Admin Showback Dashboard (6 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-DASH-001](TC-DASH-001.md) | Admin dashboard displays subscription-level usage | P0 |
| [TC-DASH-002](TC-DASH-002.md) | Filter dashboard by subscription | P1 |
| [TC-DASH-003](TC-DASH-003.md) | Filter dashboard by model | P1 |
| [TC-DASH-004](TC-DASH-004.md) | Filter dashboard by time range | P1 |
| [TC-DASH-005](TC-DASH-005.md) | CSV export for finance team cost attribution | P1 |
| [TC-DASH-006](TC-DASH-006.md) | Opt-in per-user metrics flag (default off) | P2 |

## TC-BBR — Pluggable BBR Framework (3 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-BBR-001](TC-BBR-001.md) | PayloadProcessing plugin handles request processing hook | P0 |
| [TC-BBR-002](TC-BBR-002.md) | PayloadProcessing plugin handles response processing hook | P0 |
| [TC-BBR-003](TC-BBR-003.md) | Plugin deployment via Helm chart | P1 |

## TC-GITOPS — GitOps Workflow Compatibility (3 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-GITOPS-001](TC-GITOPS-001.md) | Declarative YAML MaaS configuration via Argo CD | P1 |
| [TC-GITOPS-002](TC-GITOPS-002.md) | CRD validation webhook works during GitOps reconciliation | P1 |
| [TC-GITOPS-003](TC-GITOPS-003.md) | Subscription CRD update via GitOps push | P1 |

## TC-SEC — Security Testing (5 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-SEC-001](TC-SEC-001.md) | API keys stored as cryptographic hashes only | P0 |
| [TC-SEC-002](TC-SEC-002.md) | Authorino gateway-layer API key validation | P0 |
| [TC-SEC-003](TC-SEC-003.md) | Namespace isolation prevents cross-subscription access | P0 |
| [TC-SEC-004](TC-SEC-004.md) | OIDC token replay attack prevention | P1 |
| [TC-SEC-005](TC-SEC-005.md) | Expired API key rejected at gateway | P0 |

## TC-PERF — Performance Testing (4 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-PERF-001](TC-PERF-001.md) | Dashboard loads under 3 seconds with 200 models and 500 subscriptions | P1 |
| [TC-PERF-002](TC-PERF-002.md) | Quota status refresh within 60 seconds | P1 |
| [TC-PERF-003](TC-PERF-003.md) | CSV export handles monthly billing cycle data | P1 |
| [TC-PERF-004](TC-PERF-004.md) | Concurrent API key operations | P2 |

## TC-AIRGAP — Disconnected/Air-Gapped Environment Testing (5 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-AIRGAP-001](TC-AIRGAP-001.md) | API key lifecycle in air-gapped cluster | P0 |
| [TC-AIRGAP-002](TC-AIRGAP-002.md) | Circuit breaker deactivation in disconnected environment | P1 |
| [TC-AIRGAP-003](TC-AIRGAP-003.md) | Migration tooling runs without external connectivity | P0 |
| [TC-AIRGAP-004](TC-AIRGAP-004.md) | Dashboard operates with local-only metrics | P1 |
| [TC-AIRGAP-005](TC-AIRGAP-005.md) | Local model serving with full governance in disconnected cluster | P0 |

## TC-UPG — Upgrade and Migration Path Testing (6 test cases)

| Test Case ID | Title | Priority |
|-------------|-------|----------|
| [TC-UPG-001](TC-UPG-001.md) | RHOAI 3.0 to 3.4 upgrade with ConfigMap to CRD migration | P0 |
| [TC-UPG-002](TC-UPG-002.md) | RHOAI 3.3 to 3.4 upgrade with ConfigMap to CRD migration | P0 |
| [TC-UPG-003](TC-UPG-003.md) | Service continuity during upgrade | P0 |
| [TC-UPG-004](TC-UPG-004.md) | Rollback procedure from 3.4 to 3.3 | P1 |
| [TC-UPG-005](TC-UPG-005.md) | Data integrity verification post-upgrade | P0 |
| [TC-UPG-006](TC-UPG-006.md) | K8s Service Account token backward compatibility during API key rollout | P0 |
