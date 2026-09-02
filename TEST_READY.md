# TEST_READY: Lawfirm i18n E2E Test Suite

## Test Execution Command
```bash
node apps/lawfirm/scripts/test-i18n.js
```
Alternative commands:
```bash
node --test apps/lawfirm/scripts/test-i18n.js
```

## Test Runner Architecture
- **Framework**: Native Node.js `node:test` and `node:assert/strict` (zero external npm test dependencies).
- **Target File**: `apps/lawfirm/scripts/test-i18n.js`
- **Execution Mode**: ESM module runtime (`apps/lawfirm/package.json` `"type": "module"`).
- **Execution Time**: ~100ms.
- **Exit Code**: `0` on all passing tests, `1` on any failure.

## Tier Coverage Summary

| Tier | Category | Minimum Required | Actual Tests | Status |
|---|---|:---:|:---:|:---:|
| **Tier 1** | Feature Coverage (13 Features) | 65 | 65 | PASSED (65/65) |
| **Tier 2** | Boundary & Corner Cases (13 Features) | 65 | 65 | PASSED (65/65) |
| **Tier 3** | Pairwise Cross-Feature Interactions | 15 | 16 | PASSED (16/16) |
| **Tier 4** | Real-World Application Workflows | 6 | 6 | PASSED (6/6) |
| **Total** | **Complete Suite** | **151** | **152** | **PASSED (152/152)** |

---

## Detailed Tier Breakdown

### Tier 1: Feature Coverage (65 Tests)
- **Feature 1: Core Translation Engine (`__()`)**: 5 tests (basic keys, named interpolation, sprintf positional params, raw key fallback, whitespace trimming).
- **Feature 2: Runtime Locale Switching (`setLocale()`)**: 5 tests (state updates, `toggleLocale`, `localStorage` persistence, `i18n:localeChanged` broadcast, `lang` attribute sync).
- **Feature 3: Multi-Tier Fallback Hierarchy**: 5 tests (Tier 1 active locale, Tier 2 WordPress i18n, Tier 3 base `en_US`, Tier 4 raw key, backward-compatible raw string aliases).
- **Feature 4: Pluralization (`_n()`) and Interpolation**: 5 tests (singular count 1, plural count >1, plural count 0, named `{count}` interpolation, sprintf `%d` interpolation).
- **Feature 5: Context Translation (`_x()`)**: 5 tests (noun context, verb context, WordPress `_x` delegation, default fallback, context-aware parameter interpolation).
- **Feature 6: Directives**: 5 tests (`i18n` text, `i18n-placeholder`, `i18n-title`, `i18n-value`, `i18n-aria-label`).
- **Feature 7: Complete en_US Dictionary**: 5 tests (file presence, uppercase semantic keys, non-empty values, core domain prefixes, UMD export).
- **Feature 8: Complete pt_BR Dictionary**: 5 tests (file presence, uppercase semantic keys, non-empty Portuguese strings, domain prefixes, Portuguese diacritics).
- **Feature 9: 100% Dictionary Key Parity**: 5 tests (1-to-1 key parity, zero orphaned keys, matching `{param}` variables, matching `%s`/`%d` specifiers, dictionary index export).
- **Feature 10: Template Semantic Key Integration**: 5 tests (`index.html` shell, `clientes.html`, `processos.html`, `cadastro-cliente.html`, modal templates).
- **Feature 11: Controller Semantic Key Integration**: 5 tests (`MyMessage.show()`, feedback messages, confirm dialogs, `MONTHS` constants, `RECURSOS` permissions).
- **Feature 12: Locale Generator CLI (`i18n-generate.js`)**: 5 tests (scaffold structure, 100% key coverage, base catalog selection, Node vm execution, alphabetical key sorting).
- **Feature 13: Automated Translation Audit CLI (`i18n-audit.js`)**: 5 tests (template key extraction, controller key extraction, missing key detection, clean status reporting, JSON reporting).

### Tier 2: Boundary & Corner Cases (65 Tests)
- **Boundary Cases: Feature 1 (Core Engine)**: 5 tests (null/undefined keys, empty/whitespace strings, numeric/boolean types, special characters `#@!`, missing/extra interpolation parameters).
- **Boundary Cases: Feature 2 (Runtime Switching)**: 5 tests (unknown locale code, mixed case locale codes, rapid sequential switching, restricted localStorage, duplicate locale set).
- **Boundary Cases: Feature 3 (Fallback Hierarchy)**: 5 tests (empty locale dictionary, missing base dictionary, recursive WP key prevention, fallback parameter interpolation, prototype pollution protection).
- **Boundary Cases: Feature 4 (Pluralization & Interpolation)**: 5 tests (negative counts, floating point counts, non-numeric strings, missing plural key fallback, multi-variable templates).
- **Boundary Cases: Feature 5 (Context Translation)**: 5 tests (empty/null context, slashes and symbols in context, unknown context fallback, WP error resilience, context key casing variations).
- **Boundary Cases: Feature 6 (Directives)**: 5 tests (child icon tag preservation, empty element text, multiple simultaneous directives, unmounted element safety, input button values).
- **Boundary Cases: Feature 7 (en_US Dictionary)**: 5 tests (duplicate key detection, keys with colons, empty value rejection, balanced brace validation, high-volume performance).
- **Boundary Cases: Feature 8 (pt_BR Dictionary)**: 5 tests (UTF-8 multi-byte encoding, rejection of [TODO] markers, empty value rejection, balanced quotation marks, Portuguese legal terminology).
- **Boundary Cases: Feature 9 (Parity)**: 5 tests (missing key detection, orphan key detection, variable name mismatch detection, colon mismatch detection, empty dictionary handling).
- **Boundary Cases: Feature 10 (Template Migration)**: 5 tests (ternary filter expressions, dynamic ng-attr bindings, comments/whitespace around tags, inline icons inside buttons, hardcoded raw text detection).
- **Boundary Cases: Feature 11 (Controller Migration)**: 5 tests (undefined message handling, interpolation objects, pre-initialization calls, dynamic confirm prompts, async concurrent translations).
- **Boundary Cases: Feature 12 (Locale Generator CLI)**: 5 tests (invalid locale code rejection, overwrite protection without `--force`, non-existent base rejection, `--help`/`--version` flags, vm script compilation).
- **Boundary Cases: Feature 13 (Audit CLI)**: 5 tests (file type filtering, malformed script detection, empty template handling, unmapped key detection, strict mode empty value rejection).

### Tier 3: Pairwise Cross-Feature Interactions (16 Tests)
- **T3.1**: Dynamic locale switching + multiple open DOM directives.
- **T3.2**: Fallback cascading + parameter interpolation.
- **T3.3**: Pluralization combined with contextual translation (`_x` + `_n`).
- **T3.4**: Dynamic locale switching + pluralization reactivity.
- **T3.5**: Runtime dictionary extension (`addTranslations`) + immediate locale switch + directive update.
- **T3.6**: Multi-tier fallback + HTML entity preservation.
- **T3.7**: Locale switcher directive interaction + document language attribute synchronization.
- **T3.8**: Controller feedback service + semantic key translation + parameter interpolation across locale switch.
- **T3.9**: Generator CLI scaffolds new locale -> Engine loads it -> Engine translates new locale.
- **T3.10**: Generator CLI scaffolds new locale -> Audit CLI verifies parity with base catalog.
- **T3.11**: Template semantic key parsing + dictionary parity verification.
- **T3.12**: Controller message key extraction + dictionary parity verification.
- **T3.13**: Fallback hierarchy with legacy Portuguese string aliases during transition.
- **T3.14**: Multiple simultaneous directives on input element reacting to locale switch.
- **T3.15**: Pluralization with boundary counts (0, 1, many) across en_US and pt_BR.
- **T3.16**: Context translation with fallback to base key when context key missing in active locale.

### Tier 4: Real-World Application Workflows (6 Tests)
- **T4.1 (Scenario 1)**: Client Management End-to-End Workflow (Create, Edit, Validate, Feedback, Locale Switch).
- **T4.2 (Scenario 2)**: Process & Case Management End-to-End Workflow (Listing, Plural Counter Badge, Delete Confirmation, Locale Switch).
- **T4.3 (Scenario 3)**: Financial Accounts Payable & Receivable Workflow (Tabs, Status Badges, Dynamic Updates).
- **T4.4 (Scenario 4)**: Settings and RECURSOS Permissions Multi-Language Workflow (Sub-modules, 17 Resource Descriptors, 12 Months).
- **T4.5 (Scenario 5)**: Dynamic Locale Switching Across Multiple Open Views Without Reload (SPA Shell, Nav, Search, Modals, Badges).
- **T4.6 (Scenario 6)**: New Locale Generation, Loading, and Audit Verification Workflow (es_ES Scaffold -> Parity Check -> Runtime Load -> Translate).

---

## Verification Result
- **Total Tests**: 152
- **Passing Tests**: 152
- **Failing Tests**: 0
- **Duration**: < 120ms
- **Status**: READY FOR MILESTONE GATES (M1 through M6)
