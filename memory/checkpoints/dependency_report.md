# Data Fabric Governance Scanner — Dependency Report

**Product:** sn_data_fabric_governance_scanner  
**Scope:** x_sn_data_fabric_governance_scanner  
**Last Updated:** 2026-05-31  

---

## Internal Dependencies (Python)

| Dependency | Version | Purpose | Required |
|-----------|---------|---------|----------|
| `requests` | ≥2.28 | HTTP REST calls to ServiceNow CMDB API | Yes |
| `argparse` | stdlib | CLI argument parsing | Yes |
| `json` | stdlib | Report serialization | Yes |
| `collections.Counter` | stdlib | Class distribution counting | Yes |
| `tempfile` | stdlib | Test report file generation | Test only |
| `unittest.mock` | stdlib | Mocking REST responses in tests | Test only |
| `pytest` | ≥7.0 | Test runner | Test only |

---

## ServiceNow Platform Dependencies

| Plugin / Table | Plugin ID | Required | Purpose |
|---------------|-----------|----------|---------|
| CMDB (cmdb_ci) | com.snc.cmdb | Yes | Core CI table for governance scans |
| REST API (api/now/table) | com.glide.rest.api | Yes | CMDB data access via REST |
| CSDM Data Model | com.snc.csm | Optional | Enhanced class hierarchy awareness |

---

## External Integration Dependencies

| Integration | Protocol | Direction | Purpose |
|------------|----------|-----------|---------|
| ServiceNow Instance | HTTPS/REST | Outbound | Fetch CMDB records |
| CI/CD Pipeline | File-based | Outbound | Consume JSON governance reports |
| Power BI / Tableau | File-based | Outbound | Import JSON/MD for dashboarding |

---

## Role Requirements

| Role | Required | Purpose |
|------|----------|---------|
| snc_read_only | Yes | REST API read access to cmdb_ci |
| admin (or equivalent) | Yes | Basic auth for REST endpoint |

---

## Environment Variables (Optional)

| Variable | Default | Purpose |
|----------|---------|---------|
| `SN_INSTANCE` | — | Instance URL (falls back to --instance CLI arg) |
| `SN_USER` | — | Username (falls back to --user CLI arg) |
| `SN_PASSWORD` | — | Password (falls back to --password CLI arg) |

---

## Compatibility Matrix

| ServiceNow Release | CMDB API Version | Status |
|-------------------|------------------|--------|
| Washington DC | v2 | Verified |
| Yokohama | v2 | Verified |
| Zurich | v2 | Verified |
| Australia | v2 | Target release |

---

## Risk of Missing Dependencies

| Missing Dependency | Impact | Severity |
|-------------------|--------|----------|
| `requests` not installed | Scanner cannot fetch CMDB data — complete failure | P0 |
| CMDB plugin disabled | API returns empty results — silent failure | P0 |
| REST API disabled | 403 Forbidden — all scans fail | P0 |
| Insufficient role (no read access) | 401 Unauthorized | P1 |
| Network timeout (>30s) | Partial data or empty results | P2 |
