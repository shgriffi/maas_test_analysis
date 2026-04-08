# TC-TELEM-004: Telemetry activation and deactivation per gateway

**Priority**: P1
**Objective**: Verify that token consumption telemetry can be activated and deactivated on a per-gateway basis

**Preconditions**:
- Two MaaS gateway instances configured
- External metering system is reachable

**Test Steps**:
1. Activate telemetry on gateway-A and deactivate it on gateway-B
2. Send an inference request through gateway-A
3. Verify a telemetry event is emitted to the metering system
4. Send an inference request through gateway-B
5. Verify NO telemetry event is emitted for the gateway-B request
6. Activate telemetry on gateway-B
7. Send another inference request through gateway-B
8. Verify a telemetry event IS now emitted for gateway-B

**Expected Results**:
- Telemetry is independently configurable per gateway
- Active gateways emit events; deactivated gateways do not
- Activating telemetry takes effect without gateway restart
- Deactivating telemetry immediately stops event emission

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
