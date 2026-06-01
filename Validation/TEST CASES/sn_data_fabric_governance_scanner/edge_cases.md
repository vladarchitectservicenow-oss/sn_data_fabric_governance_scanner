# Edge Cases: Data Fabric Governance Scanner

**Product:** sn_data_fabric_governance_scanner  
**Author:** Vladimir Kapustin  
**Last Updated:** 2026-05-31  

---

## Edge Case Catalog

| ID | Edge Case | Description | Expected Handling | Covered? |
|----|-----------|-------------|-------------------|----------|
| E01 | Empty CMDB (zero records) | Instance has no cmdb_ci records; table exists but is empty | Return total=0, score=0, empty arrays; no division by zero or NoneType error | ✅ test_empty_cmdb_handling |
| E02 | 50,000+ records | Pagination cap at 500; scanner only sees first 500 records | Score reflects partial view — metadata should indicate sample size | ⚠️ Not tested (500-record cap known) |
| E03 | Null/missing fields | CI records where name is null (not empty string) vs "" (empty string) | analyze() treats empty string as missing; null values should also count as missing | ⚠️ Partial — test uses "" not None |
| E04 | Missing plugin (CMDB not activated) | ServiceNow instance without CMDB plugin | REST API returns 404 or empty result; scanner returns [] gracefully | ✅ test_fetch_cmdb (mock) |
| E05 | Network timeout (>30s) | Instance slow to respond, requests exceeds 30s timeout | requests.get raises Timeout exception; fetch_cmdb catches Exception, returns [] | ✅ Exception catch in fetch_cmdb |
| E06 | Unicode in CI names | CI names with emoji, Cyrillic, Chinese, right-to-left scripts | json.dump with ensure_ascii=False preserves characters; MD report renders correctly | ⚠️ Not tested |
| E07 | Concurrent scan writes | Two instances of scanner run simultaneously with same output prefix | Second run overwrites first report; no file-locking mechanism | ⚠️ Not tested — no locking |
| E08 | Malformed JSON response | ServiceNow returns HTML error page instead of JSON | requests.get → json() parse fails → Exception caught; returns [] | ✅ Exception catch handles |

---

## Risk Assessment

| Edge Case | Severity | Likelihood | Risk Level |
|-----------|----------|------------|------------|
| E01 (empty CMDB) | P1 | Medium | Acceptable — handled cleanly |
| E02 (50K+ records) | P1 | High | **Gap** — needs pagination support |
| E03 (null vs empty) | P2 | Medium | Acceptable — test enhancement planned |
| E04 (missing CMDB) | P2 | Low | Acceptable — handled gracefully |
| E05 (network timeout) | P2 | Medium | Acceptable — caught by broad except |
| E06 (unicode names) | P3 | Low | Acceptable — ensure_ascii=False handles |
| E07 (concurrent writes) | P3 | Low | Acceptable — documented limitation |
| E08 (malformed JSON) | P2 | Low | Acceptable — caught by broad except |

---

## Recommendations for v1.1

1. Add pagination support for >500 records (E02)
2. Add explicit null vs empty string distinction in analyze() (E03)
3. Add file-locking or timestamp-based prefix to prevent concurrent write collisions (E07)
4. Add specific HTTP status code handling (404, 429, 500) instead of broad Exception catch
