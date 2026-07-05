# CAS Analyzer Project Context

## Purpose

This document is the concise onboarding entry point for developers and AI coding assistants. Read it before designing or changing the project. It summarizes the current product direction and engineering guardrails; detailed documents under `docs/` remain the authoritative sources.

Update this file whenever scope, architecture, technology, or major business rules change.

## Product Summary

CAS Analyzer is a privacy-first, offline Android application for Indian retail investors. It imports text-based NSDL/CDSL Consolidated Account Statement (CAS) PDFs and converts them into understandable portfolio holdings, transactions, analytics, recommendations, and reports.

The product must favor financial correctness over speed. Calculations and recommendations must be deterministic, reproducible, and explainable.

## Primary Users and Outcomes

Primary users are retail investors with mutual funds and demat holdings who value privacy and offline access. The application should help them:

- Import CAS statements with minimal effort.
- Understand their complete portfolio and allocation.
- Review holdings and historical transactions.
- Identify concentration, duplication, and missing nominees.
- Export useful portfolio reports.

## Version 1 Scope

Version 1 is a single-user, fully offline Android application.

### Included

- Select and validate one or more CAS PDFs.
- Support searchable, text-based NSDL and CDSL statements.
- Show import progress and import history.
- Detect duplicate imports.
- Extract investor details, demat accounts, mutual funds, equities, transactions, nominees, and available corporate actions.
- Validate extracted data and persist it in normalized SQLite tables.
- Preserve historical transactions and support schema migrations.
- Display dashboard summaries, holdings, transactions, and asset allocation.
- Provide basic diversification and concentration analysis.
- Provide explainable, rule-based recommendations.
- Detect missing nominees and duplicate investments.
- Generate and export portfolio, holdings, and transaction reports.
- Provide application settings and light/dark themes.

Backup and restore is optional for Version 1. Advanced analytics such as XIRR may be deferred if they are not ready for the initial release.

### Excluded from Version 1

- Scanned PDFs and OCR.
- Live prices, NAV updates, or market feeds.
- Trading or brokerage integration.
- Mandatory registration or internet access.
- Cloud backup, synchronization, and multi-device support.
- Multi-user and advisor workflows.
- Tax, retirement, insurance, loan, or goal planning.
- Push notifications and market alerts.
- AI-generated investment advice or natural-language portfolio assistance.
- Encrypted database and biometric authentication; these are future security enhancements.

## Core Product Flow

```text
Select CAS PDF
  -> validate file and supported format
  -> identify duplicate/import identity
  -> extract text locally
  -> detect issuer and statement sections
  -> parse structured records
  -> validate and reconcile records
  -> persist atomically in SQLite
  -> calculate portfolio views and insights
  -> present dashboard, details, and exports
```

Long-running extraction, parsing, and calculation must not block the UI. Large statements of roughly 200-300 pages should be handled without exhausting device memory.

## Architecture Baseline

- Framework and language: Flutter and Dart.
- Primary platform: Android; avoid choices that unnecessarily prevent future iOS support.
- Style: feature-based Clean Architecture.
- State management and dependency injection: Riverpod.
- Navigation: GoRouter.
- Persistence: SQLite.
- PDF processing: Syncfusion Flutter PDF.
- File selection: File Picker.
- Charts: `fl_chart`.
- Preferences: SharedPreferences for lightweight settings only.
- Logging: `logger`, with no sensitive or personally identifiable financial data.
- Testing: `flutter_test`, `mocktail`, and `integration_test`.

### Dependency Rules

- Presentation depends on domain abstractions and use cases.
- Domain entities and business rules must remain independent of Flutter and SQLite.
- Repository interfaces belong at the domain boundary; data-layer implementations satisfy them.
- Data sources own SQLite and other infrastructure interaction.
- Widgets must never execute SQL or contain parsing, calculation, or recommendation rules.
- Features communicate through intentional public interfaces, not internal implementation details.
- All persistent access goes through repositories.
- Dependencies are injected through constructors or Riverpod providers; avoid service locators and mutable global state.
- Significant architecture decisions require an ADR under `docs/ADR/`.

### Intended Source Organization

```text
app/lib/
  core/                 shared infrastructure without feature business logic
  shared/               generic reusable UI and helpers
  features/
    pdf_import/
    cas_parser/
    dashboard/
    holdings/
    transactions/
    portfolio/
    analytics/
    recommendations/
    reports/
    nominee/
    settings/
```

Each feature normally contains `data/`, `domain/`, and `presentation/`. Add abstractions only when they provide a clear boundary or testability benefit; Clean Architecture should not become boilerplate for its own sake.

## Feature and Delivery Order

The main dependency chain is:

```text
Project foundation
  -> architecture
  -> database and migrations
  -> PDF extraction and CAS parsing
  -> import orchestration and persistence
  -> dashboard, holdings, and transactions
  -> analytics and recommendations
  -> reports and export
  -> stabilization and release
```

Feature IDs are defined in `docs/00_Project/03_FeatureCatalog.md` (`FT-001` through `FT-050`). Reference applicable Feature IDs and Goal IDs in implementation plans, tests, commits, and architecture documents.

## Quality, Privacy, and Security Guardrails

- All core behavior must work without a network connection.
- Investment data must remain on-device unless the user explicitly exports or backs it up.
- Request only minimum Android permissions.
- Never log CAS content, investor identity, account numbers, holdings, transactions, or report contents.
- Validate every imported file and every parsed record before persistence.
- Do not silently discard parse failures or calculation errors.
- Show user-friendly errors while retaining safe diagnostic context.
- Preserve provenance so important displayed values can be traced to their source statement and calculation.
- Design imports for idempotency and transactional consistency.
- Keep parser variants modular because NSDL/CDSL layouts can change.
- Critical parsers, reconciliation rules, calculations, and recommendation rules require focused unit tests.
- New third-party packages require a clear need, maintenance and licensing review, and documentation; significant additions require an ADR.

## Coding and Documentation Conventions

- Prefer readability, explicit behavior, immutability, composition, small focused types, and SOLID principles.
- Dart files and folders use `snake_case`; types use `PascalCase`; members use `camelCase`.
- Use null safety and avoid unnecessary `!` assertions.
- Use `async`/`await`; keep expensive work off the UI thread.
- Avoid business logic in widgets and expensive work in `build()`.
- Use custom, meaningful failures/exceptions where appropriate; never ignore exceptions.
- Format with `dart format` and use Flutter recommended lints.
- Documentation is Markdown, uses Mermaid where helpful, contains cross-references and revision history, and changes alongside implementation.
- Use preferred project terms: CAS Statement, Portfolio, Holding, Transaction, Repository, Use Case, and Widget.

## Known Open Decisions

Do not silently invent answers to these questions. Resolve them in the appropriate design document or ADR before the affected implementation:

1. How "current portfolio value" is defined without live prices or NAV, including valuation date and source.
2. Whether and how password-protected CAS PDFs are supported.
3. The canonical import identity and duplicate rule: file hash, content fingerprint, statement period, accounts, or a combination.
4. Reconciliation behavior for overlapping statements and repeated transactions.
5. Whether a partially parsed statement is rejected atomically or imported with explicit warnings.
6. V1 protection for sensitive local data while database encryption is deferred.
7. Exact recommendation boundaries, thresholds, explanations, and financial disclaimers.
8. How sector/category reference data is obtained when it is absent from CAS and networking is excluded.
9. Supported report/export formats and safeguards for sensitive exported files.
10. Syncfusion licensing and the exact supported CAS/PDF capabilities.

## AI Assistant Working Instructions

Before making a change:

1. Read this file.
2. Read only the detailed documents relevant to the requested change.
3. Inspect the existing implementation and tests; documentation may describe intent ahead of implementation.
4. Identify applicable Feature IDs, Goal IDs, constraints, and ADRs.
5. State any assumption that could affect financial correctness, privacy, data compatibility, or scope.

When making a change:

- Stay within Version 1 unless the request explicitly changes scope.
- Keep domain logic independent and explainable.
- Preserve user data and backward compatibility.
- Add or update tests in proportion to risk.
- Update affected documentation and revision histories.
- Do not introduce a package, architectural pattern, online dependency, or sensitive logging without explicit justification.

## Authoritative References

Use these when more detail is required:

- Product vision: `docs/00_Project/ProjectVision.md`
- Goals: `docs/00_Project/01_ProjectGoals.md`
- Scope: `docs/00_Project/02_ProjectScope.md`
- Feature IDs: `docs/00_Project/03_FeatureCatalog.md`
- Roadmap: `docs/00_Project/04_ProductRoadmap.md`
- Constraints: `docs/00_Project/04_ProjectConstraints.md`
- Technology: `docs/00_Project/05_TechnologyStack.md`
- Coding standards: `docs/00_Project/06_CodingStandards.md`
- Project structure: `docs/00_Project/08_ProjectStructure.md`
- Development workflow: `docs/00_Project/09_DevelopmentWorkflow.md`
- Terminology: `docs/00_Project/10_Glossary.md`
- Architecture: `docs/01_Architecture/`
- Database: `docs/02_Database/`
- Parser: `docs/03_Parser/`
- UI: `docs/04_UI/`
- Business logic: `docs/05_BusinessLogic/`
- Testing: `docs/06_Testing/`
- ADRs: `docs/ADR/`

If this summary conflicts with a newer approved detailed document or ADR, the newer approved decision takes precedence and this file must be updated.

## Context Maintenance Checklist

Update this document when any of the following changes:

- Version scope or roadmap.
- Feature catalog or important acceptance criteria.
- Architecture boundaries, module ownership, or data flow.
- Approved technologies or package policy.
- Database, parser, reconciliation, valuation, or recommendation rules.
- Privacy, security, platform, or performance constraints.
- A known open decision is resolved by an ADR.

Keep the document concise: summarize decisions and link to their authoritative detail rather than copying full specifications.

## Revision History

| Version | Date       | Author       | Description                              |
| ------- | ---------- | ------------ | ---------------------------------------- |
| 1.0     | 2026-07-05 | Project Team | Initial consolidated project context. |
