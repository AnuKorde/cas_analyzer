# CAS Analyzer Version 1 Implementation Plan

**Document Version:** 1.0  
**Status:** Active  
**Last Updated:** 2026-09-19  
**Owner:** Project Team

## 1. Purpose

This is the executable, end-to-end plan for implementing CAS Analyzer Version 1.
It translates the approved product scope, architecture, and ADRs into small,
testable delivery increments. It is the primary starting point for a developer or
AI coding assistant beginning implementation.

This plan does not replace the authoritative architecture, database, parser,
business-logic, testing, or ADR documents. If this plan conflicts with an
accepted ADR, the ADR takes precedence. If it conflicts with a newer approved
design document, update this plan before continuing.

## 2. Product Boundary

### 2.1 Version 1 Delivers

- A single-user, fully offline Android Flutter application.
- Selection and validation of searchable, text-based CAS PDFs.
- Explicitly supported NSDL/CDSL layouts, introduced and tested one layout at a
  time.
- Local parsing, validation, reconciliation, and atomic SQLite persistence.
- Dashboard, holdings, transaction, import-history, and settings screens.
- Statement-dated, source-reported portfolio value and asset allocation.
- Missing-nominee and concentration warnings that are deterministic and
  explainable.
- Explicit user-initiated CSV export for portfolio, holdings, and transactions.

### 2.2 Version 1 Does Not Deliver

- OCR/scanned-PDF support, live prices/NAVs, network access, cloud sync, trading,
  accounts, tax tools, multi-user workflows, AI advice, or push notifications.
- Backup/restore, PDF reports, sector/category inference, fund-overlap inference,
  XIRR/CAGR/performance analytics, encrypted database, or biometrics.
- A fabricated “current value,” return, gain/loss, or performance measure.

## 3. Mandatory Decisions and Invariants

Every implementation task must preserve the following accepted decisions.

| Topic | Rule | Authority |
| --- | --- | --- |
| Financial values | Use fixed-scale signed integers. Do not use `double`, `num`, or SQLite `REAL` for persisted financial facts or calculations. | ADR-0001 |
| Exact duplicate import | SHA-256 digest of complete source file; a renamed identical file is a duplicate. | ADR-0002 |
| Record reconciliation | Transactions use semantic fingerprints and provenance. Holding snapshots are keyed by account, instrument, and statement end date. Conflicting same-key values reject the import. | ADR-0002 |
| Import acceptance | Material financial data is atomic. Unknown/irrelevant sections may warn only when they cannot change accepted V1 facts. | ADR-0003 |
| Valuation | Display only “Reported portfolio value as of [statement date]”, selected by latest complete statement end date. Import time is never a valuation input. | ADR-0004 |
| Privacy | Never log or commit CAS bytes/text, passwords, investor/account/folio/nominee details, holdings, transactions, values, export contents, or sensitive paths. | `docs/project_context.md` |
| Offline behavior | Core behavior must work without a network request or telemetry. | `docs/project_context.md` |

### 3.1 Stop Conditions

Do not invent an answer or write a placeholder implementation for any of these:

1. Password-protected PDF behavior.
2. Local-data threat model while database encryption is deferred.
3. Recommendation thresholds, wording, disclaimer, and severity.
4. Syncfusion licensing and exact PDF extraction capabilities.

Raise an ADR/design task when one of these decisions blocks work. It is valid to
continue on an independent phase while the decision is unresolved.

## 4. Working Rules for Developers and AI Agents

Before changing code:

1. Read `docs/project_context.md` and this plan.
2. Read only the detailed documents and ADRs relevant to the task.
3. Identify affected Feature IDs, module IDs, data classifications, and tests.
4. Inspect the existing implementation and tests; documentation may be ahead of
   code.
5. State any assumption that could affect financial correctness, privacy,
   compatibility, or Version 1 scope.

When changing code:

- Work in a small, buildable increment. Prefer one task/commit at a time.
- Keep domain rules independent of Flutter, Riverpod, SQLite, File Picker, and
  Syncfusion types.
- Use constructor injection/Riverpod providers; do not introduce service locators
  or mutable global business state.
- Keep SQL in data sources/repositories and parsing in parser adapters/use cases.
- Add focused tests in the same increment as the behavior.
- Update documentation and revision history when behavior or a decision changes.

Before merging:

```text
dart format .
flutter analyze
flutter test
```

Run integration tests and the relevant manual Android check for a feature that
crosses an adapter or platform boundary.

## 5. Target Source Layout

Use feature-oriented ownership. Do not expand `lib/models/` or `lib/repositories/`
into generic cross-feature dumping grounds; remove their placeholder folders only
when their first replacement feature modules are created.

```text
app/lib/
  main.dart
  app.dart
  core/
    errors/
    logging/
    types/
    database/
  shared/
    widgets/
    theme/
    routing/
  features/
    settings/{data,domain,presentation}/
    dashboard/{data,domain,presentation}/
    pdf_import/{data,domain,presentation}/
    cas_parser/{data,domain}/
    portfolio/{data,domain}/
    holdings/{data,domain,presentation}/
    transactions/{data,domain,presentation}/
    analytics/{domain}/
    recommendations/{domain,presentation}/
    reports/{data,domain,presentation}/
```

Create a folder only when it has a concrete owner and at least one focused type.
Avoid generic base classes, global `services/`, and shared business models.

## 6. Delivery Plan

Each phase has a completion gate. Do not begin a dependent phase until its gate
passes; independent documentation or fixture work may proceed in parallel.

### Phase 0 — Repository and Development Baseline

**Goal:** Ensure every contributor can produce the same build and test result.

**Feature IDs:** Foundation for all features.

**Tasks:**

1. From `app/`, run `flutter pub get` and commit the generated `pubspec.lock`.
2. Confirm stable Flutter includes Dart 3.12 or later.
3. Run `flutter analyze` and `flutter test`; record and fix baseline failures.
4. Add a CI workflow that runs `flutter pub get`, `flutter analyze`, and
   `flutter test` from `app/` on pull requests and main-branch pushes.
5. Confirm Android package/application ID, minimum SDK, and release signing
   placeholders are suitable for the intended distribution. Do not commit keys.
6. Add an `.editorconfig` only if it is needed to enforce existing formatting;
   do not introduce formatters or generators without an approved need.

**Done when:** dependency resolution is reproducible, the lockfile is committed,
and CI has a green baseline.

### Phase 1 — Runnable Application Shell

**Goal:** Produce a launchable, navigable application without business data.

**Feature IDs:** FT-043, FT-044, FT-046; foundation for FT-001, FT-018, FT-022,
FT-026.

**Tasks:**

1. Replace the empty `main()` with `ProviderScope` and an application bootstrap.
2. Implement `App` using `MaterialApp.router` and a project-owned GoRouter
   configuration.
3. Add routes and accessible placeholder screens for Dashboard, Import, Holdings,
   Transactions, and Settings.
4. Create a minimal light/dark theme, theme preference abstraction, and settings
   provider backed by SharedPreferences. Store no financial data in preferences.
5. Add a shared empty-state widget and app-level error presentation that expose
   safe messages only.
6. Add widget tests proving boot, default route, navigation, and theme switching.

**Do not:** add database access, parser logic, file handling, or fake portfolio
numbers to widgets.

**Done when:** the app launches on Android, navigation is deterministic, and
widget tests cover the app shell.

### Phase 2 — Core Types, Failures, and Safe Observability

**Goal:** Establish reusable correctness and privacy boundaries before data work.

**Feature IDs:** Foundation for FT-002, FT-014–FT-016, FT-030–FT-038.

**Tasks:**

1. Implement immutable fixed-scale value types for amount, quantity, price/NAV,
   and percentage according to ADR-0001.
2. Add checked parsing, range validation, comparison, arithmetic, and formatting.
   Formatting is presentation-only and must not alter stored facts.
3. Create a typed failure/result model for validation, parsing, persistence,
   unsupported format, duplicate, cancellation, and unexpected failures.
4. Create a safe logger facade that accepts event code, stage, duration, and safe
   aggregate counts only. Do not expose the third-party logger directly to
   feature/domain code.
5. Add unit tests for precision limits, leading/trailing zeros, signs, overflow,
   locale-independent parsing, invalid input, and logger redaction boundaries.

**Done when:** no financial-domain type needs `double`; failures have stable
codes; sensitive fixture values cannot reach a log event.

### Phase 3 — SQLite Version 1 and Repository Contracts

**Goal:** Make committed SQLite data the only source for portfolio views.

**Feature IDs:** FT-014, FT-015, FT-016, FT-017.

**Preconditions:** ADR-0001, ADR-0002, and ADR-0003 are applied to the schema.

**Tasks:**

1. Turn the approved physical schema into executable version-1 SQLite creation
   code. Use app-private storage, enable foreign keys, and expose no database
   until creation/migration checks succeed.
2. Implement ordered migration infrastructure even though the first release
   starts at version 1. Keep the create-latest path and future migrations
   explicit.
3. Implement domain repository interfaces and data-layer implementations for
   import commit/history, portfolio snapshots, holdings, transactions, nominees,
   and read-only dashboard queries.
4. Implement an accepted-import commit boundary. It accepts only validated,
   reconciled data and writes all records, provenance, and safe diagnostics in a
   single SQLite transaction.
5. Enforce uniqueness for committed file digest, transaction semantic
   fingerprint, and holding snapshot key. Equivalent source records add
   provenance; conflicts reject rather than overwrite.
6. Add real SQLite tests: create empty v1, constraints/foreign keys, rollback at
   each write stage, duplicate retry, conflict rejection, read-model queries,
   and future-schema rejection.

**Do not:** persist raw PDFs, extracted text, passwords, arbitrary SQL, or UI
state. Do not make repositories depend on widgets/providers.

**Done when:** a synthetic accepted change set can be committed, queried, and
rolled back safely in real SQLite tests.

### Phase 4 — Synthetic Fixtures and Parser Contracts

**Goal:** Create a safe, repeatable basis for parser development.

**Feature IDs:** FT-002, FT-005–FT-013, FT-016.

**Tasks:**

1. Establish fixture folders under `samples/` and test fixture helpers under
   `app/test/`. Commit only synthetic or demonstrably anonymized data.
2. Create text fixtures plus expected structured outputs for one approved initial
   CAS layout. Include investor/account, mutual-fund/equity holdings,
   transactions, nominee status, statement period, and parser edge cases.
3. Add negative fixtures for non-PDF, corrupt, scanned/no-text, unknown layout,
   missing mandatory financial fields, duplicate transactions, and precision
   overflow.
4. Add a deterministic large synthetic document for memory/performance tests.
5. Define parser contracts that transform source text into candidate records,
   then validated records. Preserve safe page/section/field provenance without
   retaining raw source snippets.
6. Select the first supported layout based on fixture availability and record its
   layout signature/parser version. Do not claim both NSDL and CDSL support until
   each has end-to-end fixtures and tests.

**Decision gate:** confirm Syncfusion licensing/capability and password-PDF
policy before implementing its production adapter. Password support may be
explicitly rejected for V1; it must not be silently ignored.

**Done when:** every parser test runs only against safe fixtures and one layout
has a precise supported/unsupported contract.

### Phase 5 — PDF Import Pipeline for One Layout

**Goal:** Complete the safe path from file selection to committed portfolio data.

**Feature IDs:** FT-001–FT-017 and FT-020.

**Tasks:**

1. Add a File Picker adapter restricted to user-selected PDF files. Validate
   accessibility, file signature/type, resource limits, and local-only behavior.
2. Calculate SHA-256 while reading the selected file. Check duplicates at both
   an early safe boundary and the SQLite commit boundary.
3. Add the Syncfusion extraction adapter after the Phase 4 decision gate. Reject
   scanned, corrupt, encrypted-under-unsupported-policy, empty-text, and unknown
   layouts with typed failures.
4. Implement explicit pipeline stages: validate, identify, extract, detect,
   discover sections, parse candidates, validate, reconcile, persist, cleanup,
   and publish result.
5. Keep heavy extraction/parsing off the UI path. Begin with a profileable,
   bounded implementation; choose a specific isolate/worker model only after
   package behavior and profiling justify it.
6. Add import progress, cancellation at safe boundaries, redacted diagnostics,
   and import history. A terminal successful state is allowed only after SQLite
   commit confirmation.
7. On completion, invalidate/re-query committed read models; never render parser
   candidates as portfolio truth.

**Tests:** stage contract tests, state-transition tests, transaction rollback,
duplicate renamed file, overlapping statement reconciliation, cancellation,
cleanup, no-sensitive-log checks, and one end-to-end synthetic import.

**Done when:** one supported synthetic CAS file reaches dashboard-ready committed
data, while every unsupported or failed input leaves accepted portfolio data
unchanged.

### Phase 6 — Portfolio Read Models and Core Screens

**Goal:** Let users inspect only committed, explainable portfolio data.

**Feature IDs:** FT-018–FT-029.

**Tasks:**

1. Implement repository-backed dashboard read models: latest complete statement
   metadata, reported portfolio value, asset allocation, holding count, and
   recent imports.
2. Display value labels exactly as required by ADR-0004: “Reported portfolio
   value as of [date]”. Show unavailable values instead of estimates.
3. Implement holdings list/detail with source statement date and provenance
   context where useful. Add search/filter only after the core list is correct.
4. Implement transaction list/detail with deterministic sorting, filtering, and
   source/reconciliation context.
5. Replace shell placeholders with loading, empty, error, and populated states.
6. Add widget tests using fake repository providers; add integration coverage for
   import-to-dashboard refresh.

**Done when:** all visible values are traceable to committed data and statement
date, and the UI remains usable with no imports, rejected imports, and a valid
synthetic portfolio.

### Phase 7 — V1 Analytics and Recommendations

**Goal:** Add limited, deterministic insights without implied financial advice.

**Feature IDs:** FT-030–FT-032, FT-035, FT-036, FT-038.

**Precondition:** approve recommendation thresholds, severity, wording, and
financial disclaimer. Create an ADR if the decision changes product behavior.

**Tasks:**

1. Implement statement-dated asset allocation using only explicit supported
   source classifications; otherwise show `Unclassified`.
2. Implement concentration analysis with pure domain functions and exact integer
   arithmetic.
3. Implement missing-nominee detection that distinguishes `not_registered`,
   `not_available`, and `unknown`; never assert missing data is a missing nominee.
4. Implement recommendation results containing rule ID/version, severity,
   explanation, statement date, and safe supporting context.
5. Present recommendations as informational observations with the approved
   disclaimer. Never suggest trades or mutate portfolio data.
6. Add boundary tests for thresholds, unavailable data, classifications, nominee
   states, and deterministic ordering.

**Done when:** every recommendation can explain its rule and input snapshot, and
the app can safely show no recommendation when data is insufficient.

### Phase 8 — CSV Reports and Explicit Export

**Goal:** Export user-selected committed data without hidden copies or unsafe
defaults.

**Feature IDs:** FT-039–FT-042.

**Tasks:**

1. Define CSV schemas and column order for portfolio summary, holdings, and
   transactions. Include statement date and valuation label where relevant.
2. Build exports only from committed repository read models.
3. Require explicit user action and destination selection. Warn that the file
   leaves application-controlled storage.
4. Avoid automatic export, background sharing, remote upload, overwrite without
   confirmation, and sensitive destination-path logging.
5. Add unit tests for CSV escaping, deterministic ordering, unavailable fields,
   date/value labels, and redaction. Add an Android integration/manual test for
   destination cancellation and successful export.

**Done when:** all three CSV outputs are deterministic, clearly dated, and
created only after explicit user intent.

### Phase 9 — Second Layout, Performance, Accessibility, and Release

**Goal:** Broaden verified support and prepare a dependable Android release.

**Feature IDs:** all implemented V1 features.

**Tasks:**

1. Add the second NSDL/CDSL layout only as a separate parser strategy with its
   own signature, fixture suite, supported-section matrix, and end-to-end tests.
2. Benchmark 200–300-page synthetic statements: duration by stage, peak memory,
   temporary storage, UI responsiveness, cancellation latency, and database
   commit duration. Set evidence-based limits; do not guess thresholds.
3. Audit Android permissions, private temporary-file cleanup, logging, export
   warnings, release signing, and offline behavior.
4. Add accessibility labels, keyboard/focus behavior where applicable, text
   scaling checks, color contrast, and screen-reader verification for core flows.
5. Perform regression testing: clean install, upgrade/migration, duplicate,
   failed/cancelled import, supported layouts, no-network operation, and export.
6. Update release notes, app version, documentation, and traceability before a
   release candidate.

**Done when:** all release gates in `docs/08_Development/04_ReleaseProcess.md`
pass on a representative Android device/emulator.

## 7. Test Plan by Layer

| Layer | Required test focus |
| --- | --- |
| Domain | Fixed-scale arithmetic, parsing/validation, valuation selection, reconciliation, analytics, recommendation rules. |
| Data | SQLite schema, migrations, constraints, transactions, repository mapping, duplicate/conflict behavior. |
| Parser | Format detection, section discovery, candidate generation, malformed fields, provenance, precision, supported-layout fixtures. |
| Application | Pipeline state transitions, cancellation, progress throttling, failures, cleanup, post-commit refresh. |
| Presentation | Routes, loading/empty/error states, accessibility labels, user actions, safe messages. |
| Integration | Synthetic file selection through commit and dashboard refresh; export; app restart; offline operation. |
| Privacy/performance | Sensitive-log assertions, temporary-data cleanup, resource-bound and large-fixture benchmarks. |

## 8. Definition of Done for Every Increment

An implementation increment is complete only when:

- The task names its Feature IDs and applicable ADRs.
- The dependency direction and module ownership are preserved.
- Unit/widget/integration tests appropriate to the change pass.
- `dart format .`, `flutter analyze`, and `flutter test` pass from `app/`.
- No sensitive source or portfolio data was added to code, fixtures, logs, or
  test snapshots.
- Unsupported/missing data has an explicit safe outcome; no fallback invents a
  financial fact.
- Documentation/revision history changes are included when behavior or decisions
  changed.
- The commit is small and descriptive, for example
  `feat(FT-014): create version-one SQLite schema`.

## 9. Recommended First Tickets

Implement these tickets in order. Each should be a separate branch/PR unless it
is too small to justify one.

1. **Foundation: CI and reproducible Flutter baseline** — Phase 0.
2. **FT-043/FT-044: app bootstrap, router, theme preference, shell routes** —
   Phase 1.
3. **Foundation: fixed-scale financial value objects and typed failures** —
   Phase 2.
4. **FT-014/FT-015: SQLite v1 creation, migration bootstrap, and real SQLite
   tests** — Phase 3.
5. **FT-005/FT-006: safe synthetic fixture suite and parser contracts for one
   approved layout** — Phase 4.
6. **FT-001–FT-017: one-layout import pipeline through atomic commit** — Phase 5.
7. **FT-018/FT-022/FT-026: committed-data dashboard, holdings, transactions** —
   Phase 6.

Do not start the dashboard with hard-coded financial data, and do not start a
second parser layout before the first has completed Phase 5.

## 10. Authoritative References

- Project context: `docs/project_context.md`
- Scope: `docs/00_Project/02_ProjectScope.md`
- Feature catalog: `docs/00_Project/03_FeatureCatalog.md`
- Roadmap: `docs/00_Project/04_ProductRoadmap.md`
- Module architecture: `docs/01_Architecture/02_ModuleArchitecture.md`
- Import pipeline: `docs/01_Architecture/04_ImportPipelineArchitecture.md`
- Database schema and migrations: `docs/02_Database/02_PhysicalSchema.md` and
  `docs/02_Database/03_MigrationStrategy.md`
- Parser architecture: `docs/03_Parser/`
- Testing strategy: `docs/06_Testing/00_TestingStrategy.md`
- Accepted decisions: `docs/ADR/ADR-0001-financial-value-representation.md`
  through `docs/ADR/ADR-0004-statement-based-valuation.md`

## 11. Revision History

| Version | Date | Author | Description |
| --- | --- | --- | --- |
| 1.0 | 2026-09-19 | Project Team | Initial implementation roadmap for Version 1. |
