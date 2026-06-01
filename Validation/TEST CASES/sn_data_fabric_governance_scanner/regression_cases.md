# Regression Cases: Data Fabric Governance Scanner

**Product:** sn_data_fabric_governance_scanner  
**Author:** Vladimir Kapustin  
**Last Updated:** 2026-05-31  

---

## Regression Case Matrix

| ID | Case | Trigger | Expected Behavior | Source Test |
|----|------|---------|-------------------|-------------|
| R01 | Idempotent execution — same data twice produces identical output | Run analyze() on same records twice | Score, orphan_count, duplicate_count, missing_count are identical; no state mutation between runs | test_fetch_cmdb (implicit) |
| R02 | Format consistency — JSON report structure stable across versions | Run generate_reports() with identical analysis data | JSON keys are stable: total, score, orphans, duplicate_count, missing_count, class_distribution | test_generate_json_report |
| R03 | Markdown report structure stable | Run generate_reports() with identical analysis data | MD file contains "Score:" header, class distribution table with backtick-wrapped class names | test_generate_md_report |
| R04 | Empty CMDB edge case — score 0 and empty arrays | Pass empty list to analyze() | Score=0, orphan_count=0, duplicate_count=0, missing_count=0; no division by zero | test_empty_cmdb_handling |
| R05 | Single record edge case — score stays at 100 | Pass 1 healthy record to analyze() | Score=100 (or near), no false positives | test_governance_score |
| R06 | CLI non-zero exit on invalid args | Run CLI with missing required arguments | Exit code 2 (argparse error), no stack trace to stdout | test_cli_invocation (implicit) |
| R07 | Report file overwrite — second run replaces files | Run generate_reports() twice with same prefix | Second run overwrites first, file contains second run's data only | test_generate_md_report |
| R08 | Unicode handling in CI names | Pass records with Cyrillic/Chinese/emoji names | Report files encoded as UTF-8 (`ensure_ascii=False`), special chars preserved | Not yet covered (gap) |
| R09 | Large class distribution — 20+ unique sys_class_name values | Pass records from 20+ different CI classes | Counter correctly aggregates all 20+ classes, JSON serializes without truncation | test_class_distribution (gap) |

---

## Regression Run Protocol

1. `git checkout <tag>` or reference commit hash
2. `pip install -r requirements.txt` (or ensure `requests`, `pytest` are installed)
3. `pytest tests/test_governance_scanner.py -v --tb=short`
4. Verify all 9 regression cases pass (or document which are gaps)
5. Diff regression output with baseline from prior release

---

## Known Gaps

| ID | Gap | Priority | Plan |
|----|-----|----------|------|
| R08 | Unicode/emoji CI name test missing | P2 | Add in v1.1 |
| R09 | 20+ class distribution test missing | P2 | Add in v1.1 |
