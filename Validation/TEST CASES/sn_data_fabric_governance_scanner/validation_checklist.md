# Validation Checklist: Data Fabric Governance Scanner

**Product:** sn_data_fabric_governance_scanner  
**Author:** Vladimir Kapustin  
**Checklist Version:** 1.0  
**Last Updated:** 2026-05-31  

---

## Pre-Release Gates

### Phase 1 — Documentation

- [ ] architecture_summary.md ≥40 lines with Mermaid diagram + component table + data flow + benchmarks
- [ ] dependency_report.md ≥30 lines with Python deps + ServiceNow plugins + roles + compatibility matrix
- [ ] risk_report.md ≥10 risks with P0-P3 severity + probability + impact + mitigation
- [ ] execution_plan.md ≥30 lines with phase breakdown + deliverable checklist

### Phase 2 — Validation Suite

- [ ] test_suite_SOP.md ≥10 TXX-format scenarios (T01-T12 minimum)
- [ ] regression_cases.md ≥8 RXX-format cases (R01-R09)
- [ ] edge_cases.md ≥6 entries with handling description (E01-E08)
- [ ] validation_checklist.md ≥8 items (this file)

### Phase 3 — Code Quality

- [ ] All tests pass: `pytest tests/test_governance_scanner.py -v` returns 0 failures
- [ ] No hardcoded credentials in source code
- [ ] .gitignore excludes `__pycache__/`, `*.pyc`, `reports/`

### Phase 4 — README

- [ ] README.md ≥2000 words
- [ ] README includes Mermaid architecture diagram
- [ ] README includes ROI analysis table
- [ ] README includes Troubleshooting section (symptom → cause → resolution)
- [ ] README has zero duplicate sections: `grep '^## ' README.md | sort | uniq -d` is empty
- [ ] README license header matches LICENSE file (AGPL-3.0): `grep -i 'license:' README.md | head -1`

### Phase 5 — Licensing

- [ ] LICENSE file has copyright header: `Copyright (C) 2026 Vladimir Kapustin`
- [ ] All source files have copyright header: grep for "Vladimir Kapustin" across `src/`

### Phase 6 — Git Push

- [ ] `git add -A` stages all changes
- [ ] `git diff --cached --stat` shows expected files
- [ ] Commit with conventional message: `fix: regenerate Phase 1+2 docs, deduplicate README, fix LICENSE copyright`
- [ ] Push succeeds via `x-access-token` URL
- [ ] GitHub API verifies branch exists: `GET /repos/vladarchitectservicenow-oss/sn_data_fabric_governance_scanner/branches`
- [ ] DONE.marker file exists at repo root

---

## Sign-off

| Role | Name | Date | Status |
|------|------|------|--------|
| Author | Vladimir Kapustin | 2026-05-31 | ✅ |
| QA | — | — | Pending |
