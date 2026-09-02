# Project: Lawfirm i18n Refactor

## Architecture
- Client Application: AngularJS 1.2+ Single Page Application located in `apps/lawfirm/app/`.
- i18n Engine: `apps/lawfirm/app/js/i18n.js` providing global `window.I18n`, `window.__`, `window._n`, `window._x`, and AngularJS module `xophz.i18n` with directives (`i18n`, `i18n-placeholder`, `i18n-title`, `i18n-value`, `i18n-aria-label`, `i18n-switcher`), filters, and 4-locale runtime switching (`en_US`, `pt_BR`, `es_ES`, `fr_FR`).
- Dictionaries: `apps/lawfirm/app/js/dictionaries/` (`en_US.js`, `pt_BR.js`, `es_ES.js`, `fr_FR.js`, `index.js`) using Universal Module Definition (UMD) attached to `window.I18N_DICTIONARIES`.
- Templates & Views: 45 HTML templates (`apps/lawfirm/app/index.html` + `apps/lawfirm/app/view/**/*.html`) using directives and filters.
- Controllers & Services: 35 JS controllers in `apps/lawfirm/app/js/controllers/` and feedback services in `apps/lawfirm/app/js/services.js`.
- Automation & Verification Suite: Native Node.js CLI scripts in `apps/lawfirm/scripts/` (`i18n-audit.js`, `i18n-generate.js`, `test-i18n.js`).

## Feature Inventory
| # | Feature | Description | Milestone | Source |
|---|---|---|---|---|
| 1 | Semantic Key Convention | Standardize all translation keys to UPPER_SNAKE_CASE organized by domain prefix | M1, M2, M3 | Survey & Request R1, R2, R3 |
| 2 | Runtime Locale Switching Without Reload | Update dictionaries, DOM text, placeholders, tooltips, attributes for 4 locales (EN, PT, ES, FR) | M1, M3 | Survey & Request R1, 4-Locale Directive |
| 3 | Multi-Tier Fallback Mechanism | Fall back gracefully: currentLocale -> wp.i18n -> en_US -> pt_BR -> rawKey | M1 | Survey & Request R1 |
| 4 | Pluralization and Parameter Interpolation | Support singular/plural variants and `{param}` / `%s` / `%d` substitution | M1 | Survey & Request R1 |
| 5 | Context Translation | Support contextual disambiguation via `_x(text, context)` | M1 | Survey & Request R1 |
| 6 | DOM Directive Enhancements | Safe text replacement for `i18n`, plus `i18n-placeholder`, `i18n-title`, `i18n-value`, `i18n-aria-label`, `i18n-html`, `i18n-switcher` | M1 | Survey & Request R1, R2 |
| 7 | Complete en_US Dictionary | Comprehensive English catalog with pure semantic keys | M2 | Survey & Request R3 |
| 8 | Complete pt_BR Dictionary | Comprehensive Portuguese catalog with 100% key parity | M2 | Survey & Request R3 |
| 9 | Complete es_ES Dictionary | Comprehensive Spanish catalog with 100% key parity | M2B | 4-Locale Directive |
| 10 | Complete fr_FR Dictionary | Comprehensive French catalog with 100% key parity | M2B | 4-Locale Directive |
| 11 | HTML View Templates Key Migration | Replace hardcoded text in all 45 HTML templates with semantic keys | M3 | Survey & Request R2 |
| 12 | JS Controllers & Services Migration | Replace raw messages, alerts, confirms in controllers with semantic keys | M3 | Survey & Request R2 |
| 13 | App Constants Migration | Translate `MONTHS` and `RECURSOS` permission descriptors in `app.js` | M3 | Survey & Request R2 |
| 14 | Extensible Locale Generator CLI | CLI utility `i18n-generate.js` to scaffold new dictionaries from base catalog | M4 | Survey & Request R4 |
| 15 | Automated Translation Audit Suite | Verification CLI `i18n-audit.js` checking missing keys, 4-locale parity discrepancies, and scan reports | M5 | Survey & Request R5 |
| 16 | E2E Testing Infrastructure | Native test suite in `scripts/test-i18n.js` verifying Tiers 1-4 and Tier 5 hardening across all locales | E2E, M6 | Survey & Request R5 |

## Milestones
| # | Name | Scope | Dependencies | Status |
|---|---|---|---|---|
| E2E | E2E Testing Infrastructure | Build test runner and comprehensive test suite across Tiers 1-4 | none | DONE |
| M1 | Core i18n Engine Upgrade | Upgrade `js/i18n.js` with fallback, switching, interpolation, directives | none | DONE |
| M2 | Comprehensive Dictionaries (en_US, pt_BR) | Build pure semantic `en_US.js` and `pt_BR.js` with 100% parity | M1 | DONE |
| M3 | Templates & Controllers Migration | Migrate 45 HTML templates and 35 JS controllers to semantic keys | M1, M2 | IN_PROGRESS |
| M2B | Multi-Locale Expansion (es_ES, fr_FR) | Build `es_ES.js` and `fr_FR.js` with 100% parity, update index and engine switcher | M2 | PLANNED |
| M4 | Locale Generator CLI | Build `scripts/i18n-generate.js` scaffold utility and validation | M2 | PLANNED |
| M5 | Automated Audit & Verification Suite | Build `scripts/i18n-audit.js` scanner and verification suite | M2, M3, M2B | PLANNED |
| M6 | Acceptance & Hardening | Run 100% E2E tests, Tier 5 adversarial tests, verify zero defects | All | PLANNED |

## Code Layout
- `apps/lawfirm/app/js/i18n.js`: Core i18n runtime engine and AngularJS directives/filters.
- `apps/lawfirm/app/js/dictionaries/en_US.js`: English dictionary catalog.
- `apps/lawfirm/app/js/dictionaries/pt_BR.js`: Portuguese dictionary catalog.
- `apps/lawfirm/app/js/dictionaries/es_ES.js`: Spanish dictionary catalog.
- `apps/lawfirm/app/js/dictionaries/fr_FR.js`: French dictionary catalog.
- `apps/lawfirm/app/js/dictionaries/index.js`: Dictionary index exporting all 4 locales.
- `apps/lawfirm/app/js/app.js`: Application setup, routes, constants.
- `apps/lawfirm/app/js/services.js`: Feedback services (`MyMessage`, `MyLoading`).
- `apps/lawfirm/app/js/controllers/*.js`: View controllers.
- `apps/lawfirm/app/index.html`: Main shell template.
- `apps/lawfirm/app/view/**/*.html`: View templates, partials, and modals.
- `apps/lawfirm/scripts/i18n-audit.js`: Audit and parity verification CLI.
- `apps/lawfirm/scripts/i18n-generate.js`: Locale generator CLI.
- `apps/lawfirm/scripts/test-i18n.js`: Automated E2E and unit test runner.
