# TC-BBR-003: Plugin deployment via Helm chart

**Priority**: P1
**Objective**: Verify that BBR plugins can be deployed to the inference gateway via Helm charts

**Preconditions**:
- Helm 3 installed
- Inference gateway cluster accessible
- BBR plugin Helm chart available

**Test Steps**:
1. Install a BBR plugin using the Helm chart with default values
2. Verify the Helm release is created successfully
3. Verify the plugin pod is running and ready
4. Verify the plugin is registered with the inference gateway
5. Send an inference request and verify the plugin is active (hooks are invoked)
6. Upgrade the Helm release with modified configuration values
7. Verify the plugin is updated without downtime
8. Uninstall the Helm release and verify the plugin is cleanly removed

**Expected Results**:
- Helm chart installs the plugin successfully with standard `helm install`
- Plugin is operational after installation
- Helm upgrade applies configuration changes
- Helm uninstall cleanly removes all plugin resources

**Test Data**:
```bash
helm install telemetry-plugin ./charts/bbr-plugin \
  --namespace maas-plugins \
  --set metering.endpoint=https://openmeter.example.com/v1/events \
  --set metering.enabled=true
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
