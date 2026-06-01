# Data Fabric Governance Scanner — Execution Plan

**Product:** sn_data_fabric_governance_scanner  
**Created:** 2026-05-31  
**Author:** Vladimir Kapustin  

---

## Phase 1: Analysis & Planning (COMPLETED)

### Actions
1. ✅ Clone repository from `vladarchitectservicenow-oss/sn_data_fabric_governance_scanner`
2. ✅ Inspect source code (`src/governance_scanner.py`, `src/cli.py`)
3. ✅ Review existing test suite (`tests/test_governance_scanner.py`)
4. ✅ Identify skeletal Phase 1 docs (14-27 lines each — all need rewrite)
5. ✅ Identify README duplication (lines 90-269 repeat lines 1-89)
6. ✅ Identify LICENSE/README contradiction (LICENSE=AGPL-3.0, README header=MIT)
7. ✅ Generate architecture_summary.md with Mermaid diagram + component table + data flow + benchmarks
8. ✅ Generate dependency_report.md with Python deps + ServiceNow plugins + roles + compatibility matrix
9. ✅ Generate risk_report.md with 14 risks across P0-P3 severity + probability + impact + mitigation
10. ✅ Generate execution_plan.md (this file)

---

## Phase 2: Validation Suite (IN PROGRESS)

### Actions
1. ✅ Generate test_suite_SOP.md with 12 scenarios using TXX format
2. ✅ Generate regression_cases.md with 9 regression cases using RXX format
3. ✅ Generate edge_cases.md with 8 edge cases
4. ✅ Generate validation_checklist.md with 12 items

---

## Phase 3: Code Quality & Fixes

### Actions
1. Fix LICENSE: add copyright header "Copyright (C) 2026 Vladimir Kapustin"
2. Fix .gitignore: add `__pycache__/`, `*.pyc`, `reports/` entries
3. Run test suite: `pytest tests/test_governance_scanner.py -v`
4. Verify 10/10 PASS

---

## Phase 4: README Rebuild

### Actions
1. Deduplicate README: collapse repeated sections (Overview, Architecture, Features, etc.) into single instances
2. Expand product-specific sections with Data Fabric governance context
3. Ensure ≥2000 words
4. Verify: `grep '^## ' README.md | sort | uniq -d` must be empty
5. Ensure Mermaid diagram present
6. Ensure ROI analysis table present
7. Ensure Troubleshooting table present

---

## Phase 5: Git Push

### Actions
1. `git add -A`
2. `git commit -m "fix: regenerate Phase 1+2 docs, deduplicate README, fix LICENSE copyright"`
3. Push via `x-access-token` URL
4. Verify push via GitHub API
5. Create DONE.marker

---

## Deliverables Checklist

| Artifact | Status | Gate |
|----------|--------|------|
| architecture_summary.md ≥40 lines | ✅ (110+ lines) | G1 |
| dependency_report.md ≥30 lines | ✅ (80+ lines) | G1 |
| risk_report.md ≥10 risks (P0-P3) | ✅ (14 risks) | G1 |
| execution_plan.md ≥30 lines | ✅ (this file) | G1 |
| test_suite_SOP.md ≥10 TXX scenarios | ✅ (12 scenarios) | G0 |
| regression_cases.md ≥8 RXX cases | ✅ (9 cases) | G2 |
| edge_cases.md ≥6 entries | ✅ (8 entries) | G2 |
| validation_checklist.md ≥8 items | ✅ (12 items) | G2 |
| README.md ≥2000 words, deduplicated | PENDING | G2 |
| LICENSE copyright header | PENDING | G3 |
| .gitignore excludes pycache | PENDING | G6 |
| Tests PASS (10/10) | PENDING | G1 |
| Git push verified | PENDING | G4 |
| DONE.marker created | PENDING | G4 |
