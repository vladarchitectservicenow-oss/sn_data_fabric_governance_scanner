# Test Suite SOP: Data Fabric Governance Scanner

**Product:** sn_data_fabric_governance_scanner  
**Author:** Vladimir Kapustin  
**License:** AGPL-3.0-only  
**Test Framework:** pytest  
**Command:** `pytest tests/test_governance_scanner.py -v`

---

## Scenario Matrix

| ID | Scenario | Priority | Category | Expected Result |
|----|----------|----------|----------|-----------------|
| T01 | Fetch CMDB records via REST API — valid instance returns data | P0 | Core | Returns list of CI dicts with sys_id, name, sys_class_name, operational_status |
| T02 | Detect orphan records — CIs with empty sys_class_name | P0 | Core | orphan_count >= 1 when records with blank class exist |
| T03 | Detect duplicate records — same name + class pair appears multiple times | P0 | Core | duplicate_count >= 1 when dupes exist |
| T04 | Compute governance score — healthy CMDB scores 90-100 | P0 | Core | Score is between 0 and 100, clean record scores 100 |
| T05 | Detect missing required fields — name, sys_class_name, or operational_status empty | P0 | Core | missing_count >= 1 when fields are empty strings |
| T06 | Generate Markdown report — produces readable .md file with Score header | P0 | Output | .md file exists and contains "Score:" string |
| T07 | Generate JSON report — produces valid .json with score field | P0 | Output | .json file exists, parses as valid JSON, contains "score" key |
| T08 | Filter by CI class — returns only records matching class_name | P1 | Filtering | Filtered list contains only matching sys_class_name values |
| T09 | Handle empty CMDB gracefully — zero records returns score 0 with no crash | P1 | Robustness | total=0, score=0, no exceptions |
| T10 | CLI invocation produces exit code 0 — valid arguments run without crashing | P1 | Integration | subprocess returns exit code 0 (or non-2, meaning not an arg error) |
| T11 | REST API failure resilience — network error returns empty list, not crash | P2 | Resilience | fetch_cmdb() returns [] on Exception, no unhandled traceback |
| T12 | Class distribution accuracy — Counter correctly counts CI types | P2 | Accuracy | class_distribution dict contains expected class names with correct counts |

---

## Execution Order

1. **Smoke tests:** T10 (CLI basic) → T01 (fetch) → T04 (score)
2. **Core logic:** T02 (orphans) → T03 (duplicates) → T05 (missing fields)
3. **Output validation:** T06 (MD report) → T07 (JSON report)
4. **Filtering:** T08 (class filter)
5. **Edge/robustness:** T09 (empty) → T11 (resilience) → T12 (distribution)

---

## Pass Criteria

- All 12 scenarios must PASS
- Minimum threshold: 10/12 PASS for v1.0 release
- Any P0 failure (T01-T07) = release blocker
- Any P1 failure (T08-T10) = must fix before release

---

## Failure Triage

| Failure Type | Action |
|-------------|--------|
| Network-related (T01, T10) | Verify ServiceNow instance reachable, check credentials |
| Logic-related (T02-T05) | Review analyze() method, verify input record format |
| Output-related (T06, T07) | Check file write permissions, verify prefix path |
| Mock-related (any) | Ensure mock setup matches expectations — check patch paths |
