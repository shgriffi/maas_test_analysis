# TC-MIG-003: Migration preserves 100% fidelity across quota values and group mappings

**Priority**: P0
**Objective**: Verify field-by-field fidelity between source ConfigMap and generated CRDs for all quota types and group assignments

**Preconditions**:
- ConfigMap with varied quota configurations: zero quotas, maximum quotas, equal guaranteed and burst limits

**Test Steps**:
1. Create a ConfigMap with edge-case quota values: zero tokens/min for one tier, maximum values for another, guaranteed equal to burst for a third
2. Run the migration script
3. For each Subscription CRD, compare every quota field against the original ConfigMap values using diff
4. Verify group membership lists match exactly (same groups, same order or documented ordering)
5. Verify model references in Subscriptions point to the correct Model CRDs

**Expected Results**:
- Zero quotas are preserved (not dropped or defaulted)
- Maximum quota values are preserved without overflow or truncation
- Guaranteed-equals-burst configurations are preserved (not collapsed to a single value)
- Group membership lists match exactly between ConfigMap and Subscription CRDs

**Validation**:
- Automated diff between ConfigMap JSON values and CRD spec fields returns zero differences

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
