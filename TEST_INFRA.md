# E2E Test Infra: Lawfirm i18n

## Test Philosophy
- Opaque-box, requirement-driven testing. Derived from user specifications (R1 through R5), independent of internal implementation artifacts.
- Methodology: Category-Partition, Boundary Value Analysis (BVA), Pairwise Feature Interaction Testing, and Real-World Application Workload Scenarios.

## Feature Inventory
| # | Feature | Source (requirement) | Tier 1 | Tier 2 | Tier 3 |
|---|---|---|:---:|:---:|:---:|
| 1 | Core Translation Engine (`__()`) | ORIGINAL_REQUEST R1 | 5 | 5 | Yes |
| 2 | Runtime Locale Switching (`setLocale()`) | ORIGINAL_REQUEST R1 | 5 | 5 | Yes |
| 3 | Multi-Tier Fallback Hierarchy | ORIGINAL_REQUEST R1 | 5 | 5 | Yes |
| 4 | Pluralization (`_n()`) and Interpolation | ORIGINAL_REQUEST R1 | 5 | 5 | Yes |
| 5 | Context Translation (`_x()`) | ORIGINAL_REQUEST R1 | 5 | 5 | Yes |
| 6 | Directives (`i18n`, `i18n-placeholder`, `i18n-title`, `i18n-value`) | ORIGINAL_REQUEST R1, R2 | 5 | 5 | Yes |
| 7 | Complete en_US Dictionary | ORIGINAL_REQUEST R3 | 5 | 5 | Yes |
| 8 | Complete pt_BR Dictionary | ORIGINAL_REQUEST R3 | 5 | 5 | Yes |
| 9 | 100% Dictionary Key Parity | ORIGINAL_REQUEST R3 | 5 | 5 | Yes |
| 10 | Template Semantic Key Integration | ORIGINAL_REQUEST R2 | 5 | 5 | Yes |
| 11 | Controller Semantic Key Integration | ORIGINAL_REQUEST R2 | 5 | 5 | Yes |
| 12 | Locale Generator CLI (`i18n-generate.js`) | ORIGINAL_REQUEST R4 | 5 | 5 | Yes |
| 13 | Automated Translation Audit CLI (`i18n-audit.js`) | ORIGINAL_REQUEST R5 | 5 | 5 | Yes |

## Test Architecture
- Test Runner: `apps/lawfirm/scripts/test-i18n.js` executed via `node apps/lawfirm/scripts/test-i18n.js` or `pnpm test`.
- Assertion Framework: Built-in `node:test` and `node:assert`.
- VM Sandbox: Isolated execution of dictionary modules and i18n engine runtime simulation via `node:vm`.
- Exit Code: 0 on all tests passing; non-zero on any failure.

## Real-World Application Scenarios (Tier 4)
| # | Scenario | Features Exercised | Complexity |
|---|---|---|---|
| 1 | Client Management Flow (Create, Edit, Validate, Feedback) | F1, F2, F6, F7, F8, F10, F11 | High |
| 2 | Process & Case Workflow (Status, Modals, Confirmations) | F1, F2, F4, F6, F7, F8, F10, F11 | High |
| 3 | Financial Accounts Payable/Receivable Translation | F1, F2, F6, F7, F8, F10, F11 | Medium |
| 4 | Settings & User Permissions (RECURSOS) Multi-Language | F1, F2, F7, F8, F10, F11 | Medium |
| 5 | Dynamic Locale Switching Across Open Views Without Reload | F2, F3, F6, F7, F8, F10, F11 | High |
| 6 | New Locale Generation and Validation Workflow (e.g. es_ES) | F7, F8, F9, F12, F13 | Medium |

## Coverage Thresholds
- Tier 1 (Feature Coverage): >= 65 tests (>= 5 per feature across 13 features)
- Tier 2 (Boundary & Corner Cases): >= 65 tests (empty strings, missing keys, special chars, null/undefined, numbers, malformed inputs)
- Tier 3 (Cross-Feature Combinations): >= 15 pairwise interaction tests
- Tier 4 (Real-World Application Scenarios): >= 6 end-to-end integration workflows
- Total Minimum Test Cases: >= 151 test cases
