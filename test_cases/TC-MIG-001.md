# TC-MIG-001: Migrate simple single-model ConfigMap to CRD

**Priority**: P0
**Objective**: Verify that the migration script converts a simple single-model ConfigMap JSON configuration from RHOAI 3.0/3.3 format to 3.4 Subscription CRDs

**Preconditions**:
- Legacy ConfigMap JSON from a 3.0 or 3.3 deployment with a single model and single tier
- Migration script installed and accessible

**Test Steps**:
1. Create a ConfigMap in the legacy 3.0/3.3 JSON format with one model and one user tier
2. Run the migration script targeting the ConfigMap
3. Verify the script completes without errors
4. Verify a Model CRD was created with matching capacity values
5. Verify a Subscription CRD was created with matching group mappings and quotas
6. Compare every field value between the original ConfigMap and the generated CRDs

**Expected Results**:
- Migration completes successfully with zero errors
- Model CRD capacity quotas match the original ConfigMap values exactly
- Subscription CRD group mappings and per-model quotas match the original values exactly
- No data is lost or transformed incorrectly during migration

**Test Data**:
```json
{
  "models": [
    {
      "name": "granite-3b-code",
      "totalTokensPerMinute": 50000,
      "totalRequestsPerMinute": 100
    }
  ],
  "tiers": [
    {
      "name": "analytics-team",
      "groups": ["analytics-team"],
      "models": {
        "granite-3b-code": {
          "tokensPerMinute": 10000,
          "requestsPerMinute": 20
        }
      }
    }
  ]
}
```

**Automation Status**: To be filled later in the process.

**Notes**: To be filled later in the process.
