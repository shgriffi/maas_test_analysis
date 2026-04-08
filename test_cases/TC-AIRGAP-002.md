# TC-AIRGAP-002: Circuit breaker deactivation in disconnected environment

**Priority**: P1
**Objective**: Verify that the circuit breaker can be deactivated in air-gapped deployments where no external metering system is reachable

**Preconditions**:
- Air-gapped cluster with no external metering system access
- Circuit breaker BBR plugin installed

**Test Steps**:
1. Verify the cluster cannot reach any external metering endpoints
2. Attempt to send an inference request with the circuit breaker active
3. Verify the request behavior matches the configured failure mode (fail-open allows it, fail-closed denies it)
4. Deactivate the circuit breaker for the gateway
5. Send an inference request
6. Verify the request succeeds without any budget check attempt
7. Verify no errors appear in logs related to metering system connectivity
8. Verify local model serving and governance (rate limiting) continue working normally

**Expected Results**:
- Circuit breaker can be deactivated per-gateway for disconnected environments
- With circuit breaker deactivated, no budget check queries are attempted
- Local model serving, rate limiting, and API key authentication work independently of the circuit breaker
- No spurious errors in logs from failed metering connections

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
