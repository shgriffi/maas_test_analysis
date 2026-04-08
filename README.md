# MaaS GA Platform Test Plan

MaaS (Model-as-a-Service) General Availability platform feature set for RHOAI 3.4 — covering subscription management, authentication, model serving, billing/metering, governance, and admin observability.

## Strategies

| Key | Feature |
|-----|---------|
| [RHAISTRAT-1167](https://redhat.atlassian.net/browse/RHAISTRAT-1167) | Enable vLLM Runtime Support |
| [RHAISTRAT-1117](https://redhat.atlassian.net/browse/RHAISTRAT-1117) | Subscription Model Redesign |
| [RHAISTRAT-1120](https://redhat.atlassian.net/browse/RHAISTRAT-1120) | External OIDC Support |
| [RHAISTRAT-1295](https://redhat.atlassian.net/browse/RHAISTRAT-1295) | External Model Egress |
| [RHAIRFE-1444](https://redhat.atlassian.net/browse/RHAIRFE-1444) | Token Consumption Telemetry |
| [RHAISTRAT-1201](https://redhat.atlassian.net/browse/RHAISTRAT-1201) | API Key Self-Service |
| [RHAISTRAT-1235](https://redhat.atlassian.net/browse/RHAISTRAT-1235) | Admin Showback Dashboard |
| [RHAIRFE-1443](https://redhat.atlassian.net/browse/RHAIRFE-1443) | Circuit Breaker Budget Enforcement |
| [RHAISTRAT-1320](https://redhat.atlassian.net/browse/RHAISTRAT-1320) | Pluggable BBR Framework |

## Documents

- [TestPlan.md](TestPlan.md) — Complete test plan
- [TestPlanGaps.md](TestPlanGaps.md) — Identified gaps requiring additional input

## Test Cases

- [test_cases/INDEX.md](test_cases/INDEX.md) — Complete test case index
- **79 test cases** across 15 categories
- **Priority distribution**: P0: 47 | P1: 30 | P2: 2

## Test Implementation

Automated tests will be implemented in the project test suite under the appropriate feature directories.
