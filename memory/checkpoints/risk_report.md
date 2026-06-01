# Data Fabric Governance Scanner — Risk Report

**Product:** sn_data_fabric_governance_scanner  
**Last Updated:** 2026-05-31  
**Methodology:** Each risk is classified by severity (P0=blocker, P1=major, P2=minor, P3=cosmetic), probability (H/M/L), and impact.

---

## P0 — Critical / Blockers

| ID | Risk | Probability | Impact | Mitigation |
|----|------|------------|--------|------------|
| P0-01 | CMDB table empty (no records) — scanner returns score 0 with zero findings, user thinks everything is fine | M | H | Validate `total > 0` in report, add explicit "EMPTY CMDB" warning |
| P0-02 | ServiceNow instance unreachable — REST call fails, empty list returned silently | L | H | Add connectivity pre-check before fetch, log unreachable status explicitly |
| P0-03 | Authentication failure (401) — empty result set, no indication of auth error | L | H | Catch HTTPError explicitly, surface auth failure as separate status field |
| P0-04 | `requests` library not installed — ImportError at runtime, scanner cannot start | M | H | Document in README troubleshooting, add import check in CLI entry with helpful message |

---

## P1 — Major

| ID | Risk | Probability | Impact | Mitigation |
|----|------|------------|--------|------------|
| P1-01 | Very large CMDB (500K+ CIs) — fetch_cmdb capped at 500 records, missing 99.9% of data → misleading score | H | M | Add pagination support via offset, document limit in report metadata |
| P1-02 | Duplicate detection is name+class based — same CI with different sys_ids but identical identity creates false duplicate | M | M | Add correlation_id field to benchmark, document detection methodology |
| P1-03 | Operational status values differ across releases — hardcoded "operational_status" field may not exist on all CI classes | M | M | Make required_fields configurable via CLI flag |
| P1-04 | JSON report contains raw record data (orphans list) — potential PII leakage if CI names contain user info | L | M | Add `--redact` flag to strip names from exported reports |

---

## P2 — Minor

| ID | Risk | Probability | Impact | Mitigation |
|----|------|------------|--------|------------|
| P2-01 | Rate limiting on ServiceNow REST API — repeated scans hit throttle, subsequent requests return 429 | L | L | Add `--delay` flag between paginated requests |
| P2-02 | Class distribution Counter uses tuples as keys — output JSON serializes tuples as arrays, harder to parse | H | L | Flatten tuple keys to "name | class" string in report |
| P2-03 | Timeout of 30s on REST call not configurable — slow instances may timeout on large result sets | M | L | Add `--timeout` CLI flag |
| P2-04 | Report prefix collision — concurrent scans with same prefix overwrite each other's reports | L | L | Auto-append timestamp to prefix: `{prefix}_{datetime}` |

---

## P3 — Cosmetic

| ID | Risk | Probability | Impact | Mitigation |
|----|------|------------|--------|------------|
| P3-01 | Markdown report class distribution section empty when no data — confusing blank section | L | L | Add "No data" row when distribution is empty |
| P3-02 | Governance score rounding may hide small improvements — user runs scan after fixing 1 item, score unchanged | M | L | Display score with 1 decimal, document minimum detectable change |

---

## Summary

| Severity | Count | Critical Path? |
|----------|-------|----------------|
| P0 | 4 | Yes — P0-01 (empty CMDB silence) is the highest priority fix |
| P1 | 4 | P1-01 (500-record cap) affects enterprise-scale usability |
| P2 | 4 | All addressable in v1.1 |
| P3 | 2 | Low urgency |
| **Total** | **14** | — |
