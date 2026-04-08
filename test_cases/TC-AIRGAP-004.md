# TC-AIRGAP-004: Dashboard operates with local-only metrics

**Priority**: P1
**Objective**: Verify that the admin showback dashboard operates using local Prometheus/Thanos data without requiring external metering system connectivity

**Preconditions**:
- Air-gapped cluster with local Prometheus and Thanos Querier
- No external metering system reachable
- Active model traffic generating local metrics

**Test Steps**:
1. Verify the cluster has no external network connectivity
2. Generate inference traffic against local models to produce metrics
3. Navigate to the admin usage dashboard
4. Verify the dashboard loads and displays subscription-level usage data
5. Verify token consumption and request count metrics are populated from local Prometheus/Thanos
6. Apply subscription, model, and time range filters — verify they work
7. Export a CSV and verify it contains the local metrics data
8. Verify no errors or warnings about external metering connectivity appear in the dashboard

**Expected Results**:
- Dashboard is fully functional with local-only metrics
- All filtering and export features work without external dependencies
- Metrics from Limitador and Prometheus are sufficient for the dashboard
- No UI errors or degraded states due to missing external metering

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
