# TC-VLLM-005: Dashboard UX consistency between vLLM and llm-d flows

**Priority**: P1
**Objective**: Verify that the OpenShift AI dashboard provides a consistent user experience for managing vLLM and llm-d model deployments through MaaS

**Preconditions**:
- One model deployed via vLLM runtime
- One model deployed via llm-d runtime
- Both accessible through the OpenShift AI dashboard

**Test Steps**:
1. Navigate to the model serving page in the OpenShift AI dashboard
2. Open the deployment configuration for the vLLM model
3. Verify the MaaS checkbox location, label, and behavior match the llm-d model
4. Enable MaaS for both models
5. Compare the post-enablement UI state for both (status indicators, endpoint display, governance options)
6. Navigate to the admin dashboard and verify both models appear with consistent formatting

**Expected Results**:
- MaaS checkbox is in the same position with the same label for both runtimes
- Status indicators (enabled, pending, error) display consistently
- Gateway endpoint format is identical regardless of runtime
- Admin dashboard shows both runtime types with consistent column formatting

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
