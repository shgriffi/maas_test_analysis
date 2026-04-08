# TC-AIRGAP-003: Migration tooling runs without external connectivity

**Priority**: P0
**Objective**: Verify that the ConfigMap-to-CRD migration script runs successfully in an air-gapped environment without requiring external network access

**Preconditions**:
- Air-gapped cluster with RHOAI 3.0 or 3.3 ConfigMap-based configuration
- Migration script available locally (not fetched from internet)

**Test Steps**:
1. Verify the cluster has no external network connectivity
2. Verify legacy ConfigMap JSON exists on the cluster
3. Run the migration script
4. Verify the script does not attempt any external network calls (no DNS resolution failures, no HTTP timeouts)
5. Verify Model, Subscription, and APIKey CRDs are created successfully
6. Verify 100% fidelity between the ConfigMap and the generated CRDs
7. Run the dry-run mode and verify it also works offline

**Expected Results**:
- Migration script completes successfully in a disconnected environment
- No external dependencies are required for migration (no package downloads, no remote APIs)
- CRDs are created with full fidelity
- Dry-run mode also works offline

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
