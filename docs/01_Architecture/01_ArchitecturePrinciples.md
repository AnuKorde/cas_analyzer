# CAS Analyzer Architecture Principles

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-07-05

## 1. Purpose

This document defines the durable principles used to design, implement, and review CAS Analyzer. The principles turn product goals and constraints into rules that can be applied consistently across features and architecture documents.

`SolutionArchitecture.md` describes the overall structure of the solution. This document defines the properties that structure and its implementation must preserve.

## 2. Scope

These principles apply to:

- Production Dart and Flutter code.
- Database schemas, migrations, and repositories.
- CAS extraction, parsing, validation, and reconciliation.
- Portfolio calculations, analytics, recommendations, and reports.
- Tests, fixtures, logging, exports, and local backups.
- Architecture decisions and third-party dependencies.
- Code or documentation produced with AI assistance.

They apply to Version 1 and remain in force for later versions unless deliberately revised through architecture governance.

## 3. How to Use This Document

Each principle has a stable ID for traceability. Feature designs, ADRs, implementation plans, and reviews should reference the applicable IDs.

The terms below are normative:

- **Must / must not:** mandatory for architecture compliance.
- **Should / should not:** expected unless a documented exception is justified.
- **May:** optional and context-dependent.

A principle describes direction and constraints, not a substitute for a detailed design. When a business rule is unresolved, the principle must guide the decision without inventing the missing rule.

## 4. Decision Priority

When principles compete, use this order:

1. Financial correctness and data integrity.
2. Privacy and explicit user control.
3. Reliability and recoverability.
4. Responsiveness and bounded resource use.
5. Maintainability and testability.
6. Extensibility and portability.
7. Delivery convenience.

Higher priority does not automatically justify an unbounded design. Record material trade-offs in an ADR.

## 5. Principle Summary

| ID | Principle | Core Requirement |
| --- | --- | --- |
| AP-01 | Correctness before convenience | Never guess or conceal uncertainty in financial data. |
| AP-02 | Offline and private by default | Core processing and storage remain on-device. |
| AP-03 | Explicit user control | External disclosure or destructive action requires clear intent. |
| AP-04 | Explicit architectural boundaries | Keep presentation, application, domain, and infrastructure responsibilities distinct. |
| AP-05 | Dependencies point inward | Domain policy depends on abstractions, not frameworks or storage. |
| AP-06 | Feature ownership with narrow contracts | Features own behavior and expose intentional public interfaces. |
| AP-07 | Traceable source and derived data | Important outputs can be explained from their inputs and rules. |
| AP-08 | Idempotent and consistent import | Reprocessing must not duplicate data or corrupt portfolio state. |
| AP-09 | Validate at trust boundaries | Untrusted input and state transitions are validated explicitly. |
| AP-10 | Modular and version-aware parsing | Parser variations are isolated, testable, and replaceable. |
| AP-11 | Deterministic and explainable insight | Calculations and recommendations are reproducible and qualified. |
| AP-12 | Responsive, bounded work | Heavy operations do not block the UI or consume unbounded resources. |
| AP-13 | Explicit failures and safe recovery | Failures are visible, classified, and leave consistent state. |
| AP-14 | Testability by design | Critical policy and boundaries are independently verifiable. |
| AP-15 | Evolution without speculative complexity | Extend stable boundaries only for demonstrated needs. |
| AP-16 | Dependencies are liabilities | Third-party packages require necessity and lifecycle review. |
| AP-17 | Documentation and decisions evolve together | Architecture intent and significant decisions remain current. |

## 6. Foundational Principles

### AP-01: Correctness Before Convenience

**Statement:** CAS Analyzer must prefer an unavailable, rejected, or explicitly qualified result over a plausible but unsupported financial result.

**Rationale:** Incorrect financial information can lead users to harmful conclusions. A smooth workflow cannot compensate for silently incorrect holdings, values, or recommendations.

**Required implications:**

- Parsers must not invent missing values or silently coerce malformed financial fields.
- Calculations must define their inputs, precision, date semantics, and missing-data behavior.
- Unsupported formats must fail explicitly rather than falling through to a loosely matching parser.
- Warnings and assumptions that materially affect interpretation must reach the user or consuming use case.
- A performance optimization must not change a financial result without an approved, tested rationale.

**Verification:**

- Boundary and malformed-input tests exist.
- Golden parser fixtures have explicit expected output.
- Financial rules have deterministic unit tests, including missing and edge values.
- Reviews check that fallback values do not masquerade as real data.

### AP-02: Offline and Private by Default

**Statement:** Core functionality must execute locally and must not require or initiate network communication.

**Rationale:** Privacy and offline availability are primary product value propositions.

**Required implications:**

- PDF extraction, parsing, storage, analytics, recommendations, and core reporting occur on-device.
- Investment data must not be sent to telemetry, crash-reporting, analytics, AI, or other remote services.
- A package that can communicate over a network must not do so implicitly.
- Features that require network access are outside Version 1 unless scope and architecture are explicitly changed.
- Tests should demonstrate that core workflows do not depend on network availability.

**Verification:**

- Dependency and platform-permission review.
- Runtime/network inspection before release.
- Integration tests with networking unavailable.
- Security review of telemetry and crash-reporting configuration.

### AP-03: Explicit User Control

**Statement:** The user must initiate and understand actions that disclose, export, back up, replace, or delete sensitive data.

**Rationale:** Local ownership is meaningful only when sensitive state changes and boundary crossings are visible and intentional.

**Required implications:**

- Exports and backups require a direct user action and an explicit destination.
- Destructive operations require appropriate confirmation and clearly state their scope.
- Permission requests occur in context and request only what is needed.
- The application must not silently overwrite an existing export or replace portfolio data.
- Cancellation semantics must be honest; the UI must not claim cancellation after an irreversible commit.

**Verification:**

- Widget and integration tests cover confirmations, cancellation, and error recovery.
- UX review identifies every data disclosure and destructive operation.
- Platform permission review confirms least privilege.

## 7. Structure and Dependency Principles

### AP-04: Explicit Architectural Boundaries

**Statement:** Presentation, application orchestration, domain policy, and infrastructure must have distinct responsibilities.

**Rationale:** Clear boundaries protect financial rules from framework churn and prevent UI or storage concerns from spreading through the system.

**Required implications:**

- Presentation renders state and captures intent; it does not execute SQL, parse CAS text, or define financial policy.
- Application use cases coordinate workflows and transactional intent.
- Domain types enforce business invariants and contain deterministic rules.
- Infrastructure adapts SQLite, PDF, file, preferences, export, and platform APIs.
- Mapping between domain types and persistence or presentation models is explicit where representations differ.

**Verification:**

- Code review checks imports and responsibilities.
- Automated dependency rules should be introduced when the source tree exists.
- Business-rule tests run without Flutter bindings or a database unless the boundary itself is under test.

### AP-05: Dependencies Point Inward

**Statement:** Source-code dependencies must point toward stable domain policy; infrastructure implements interfaces owned at the boundary it serves.

**Rationale:** Dependency inversion allows business behavior to remain testable and independent of Flutter plugins, SQLite, and third-party packages.

**Required implications:**

- Domain code must not import Flutter, Riverpod, SQLite, PDF, file-picker, or platform libraries.
- Repository and service interfaces used by domain/application logic belong with the consumer-facing boundary.
- Concrete adapters are selected only in the composition root.
- Riverpod may wire dependencies but must not become a domain dependency.
- Static/global access to databases or services is prohibited.

**Verification:**

- Import/dependency linting.
- Tests construct use cases with fakes or mocks through interfaces.
- Review of provider definitions and composition-root ownership.

### AP-06: Feature Ownership With Narrow Contracts

**Statement:** A feature owns its behavior and internal representation; other features consume only intentional public contracts.

**Rationale:** Feature-based organization becomes ineffective when features reach into one another's data sources, providers, or internal models.

**Required implications:**

- Cross-feature access must use a documented use case, repository abstraction, query contract, or shared domain concept.
- Features must not import another feature's internal data source or presentation state.
- `core/` contains generic infrastructure, not displaced business logic.
- `shared/` contains genuinely reusable presentation/helpers, not a miscellaneous dumping ground.
- Shared abstractions should emerge from proven use, not anticipated reuse.

**Verification:**

- Module dependency review.
- Public contracts have focused tests.
- Review rejects `common`, `misc`, or broad helper modules without clear ownership.

## 8. Data and Import Principles

### AP-07: Traceable Source and Derived Data

**Statement:** Important stored and derived financial information must retain enough provenance to explain where it came from and how it was produced.

**Rationale:** Users and developers must be able to investigate discrepancies without relying on sensitive logs or reparsing a statement blindly.

**Required implications:**

- Imported records reference their source import and relevant source location or section when practical.
- Derived results identify relevant source records, statement/valuation date, and calculation or rule version.
- Parser warnings and material normalization decisions remain associated with the affected import or record.
- Provenance is application data with privacy protections; it must not expose raw sensitive content unnecessarily.
- Deletion and re-import rules account for provenance relationships.

**Verification:**

- Schema and mapping review.
- Integration tests trace representative dashboard/report values to source records.
- Diagnostics work without logging raw CAS content.

### AP-08: Idempotent and Consistent Import

**Statement:** Reprocessing the same logical input must not duplicate portfolio data, and every durable write must leave the database internally consistent.

**Rationale:** Duplicate or partially related financial records can corrupt every downstream result.

**Required implications:**

- Import identity and record identity must be explicitly designed and database-enforced where possible.
- Duplicate detection occurs early for efficiency and at persistence for integrity.
- Related durable writes use a transaction with rollback on failure.
- Re-import and overlapping-statement behavior must follow an approved reconciliation policy.
- A future approved partial-import policy may commit an explicitly accepted subset, but each commit must remain consistent and auditable.
- Cancellation before durable persistence leaves no portfolio mutation.

**Verification:**

- Tests cover exact duplicates, renamed files, overlapping periods, retry after failure, and concurrent attempts.
- Repository integration tests verify uniqueness constraints and rollback.
- Import history agrees with committed portfolio state.

### AP-09: Validate at Trust Boundaries

**Statement:** Data must be validated when it crosses a trust or representation boundary, with domain invariants enforced again at the domain boundary.

**Rationale:** File extensions, extracted text, parsed fields, and persisted rows each have different failure modes. One validation step cannot protect every boundary.

**Required implications:**

- File selection validates access, supported type/content, and safe resource limits.
- Extraction distinguishes corrupt, encrypted, scanned, and unsupported documents.
- Parsing validates grammar and field shape without claiming business validity.
- Domain construction validates financial invariants and relationships.
- Persistence relies on database constraints as defense in depth.
- Exports validate destination and serialization success.

**Verification:**

- Negative tests at each boundary.
- Invalid domain states are difficult or impossible to construct.
- Database constraints mirror critical invariants that can be represented relationally.

### AP-10: Modular and Version-Aware Parsing

**Statement:** CAS extraction, format detection, section identification, and domain parsing must be separate, composable stages with format-specific behavior isolated behind contracts.

**Rationale:** NSDL/CDSL formats and layouts can change independently; a monolithic parser would be fragile and difficult to validate.

**Required implications:**

- PDF-library output is adapted into a project-owned extraction representation.
- Format detection is explicit and precedes format-specific parsing.
- NSDL/CDSL and future layout variants have isolated strategies.
- Shared parsing utilities remain deterministic and format-neutral.
- Unsupported versions return a clear outcome rather than using an arbitrary fallback.
- Parser fixtures identify issuer/layout variants and expected diagnostics.

**Verification:**

- Parser stages have independent unit tests.
- Adding a supported layout does not require editing unrelated feature logic.
- Golden fixtures protect known statement variants from regression.

## 9. Insight and Runtime Principles

### AP-11: Deterministic and Explainable Insight

**Statement:** Portfolio calculations, analytics, and recommendations must produce reproducible results and explain the basis of those results.

**Rationale:** Users must be able to understand important conclusions, especially when data is incomplete or stale.

**Required implications:**

- Identical inputs and rule versions produce identical outputs.
- Money, units, rates, rounding, and date semantics are explicitly defined.
- Calculations do not use current time implicitly; an effective date is supplied where time matters.
- Missing valuation or classification data yields an unavailable or qualified result, never a fabricated value.
- Recommendation outputs include a stable rule ID, version, relevant inputs/thresholds, and user-facing explanation.
- Recommendations are observations, not guaranteed outcomes or personalized regulated advice.

**Verification:**

- Unit tests include boundary thresholds, rounding, date edges, and incomplete data.
- Snapshot/golden tests may verify explanations without replacing rule tests.
- Reports and UI agree because they consume shared domain results.

### AP-12: Responsive, Bounded Work

**Statement:** Work must be structured so input size cannot freeze the interface or cause avoidable unbounded memory consumption.

**Rationale:** CAS Statements may contain hundreds of pages and run on resource-constrained consumer devices.

**Required implications:**

- Heavy extraction, parsing, and calculation must not block the main isolate.
- Processing should be incremental where supported.
- Isolate boundaries use bounded, serializable messages and avoid repeated full-document copies.
- Database mutations are batched appropriately; large reads are paged or bounded.
- Progress reports real stages or measured work, not invented precision.
- Resources are released on success, cancellation, and failure.
- Optimization decisions follow profiling on representative devices.

**Verification:**

- Performance tests use representative small and large synthetic statements.
- UI responsiveness is measured during import.
- Memory and duration regressions are tracked once baselines exist.

### AP-13: Explicit Failures and Safe Recovery

**Statement:** Expected failures must be modeled and communicated explicitly, while unexpected failures must be contained without corrupting durable state.

**Rationale:** Silent failure produces untrustworthy results; uncontrolled exceptions produce poor recovery and may expose sensitive details.

**Required implications:**

- Failure categories distinguish input, extraction, parsing, validation, persistence, calculation, and export problems.
- Exceptions must not be ignored or converted into successful empty results.
- User messages are actionable but exclude stack traces, SQL, raw CAS content, and personal data.
- Retriable operations preserve enough safe state to retry without duplication.
- Database failures roll back the current unit of work.
- Logs use structured, redacted metadata only.

**Verification:**

- Tests exercise failure at each stage and verify resulting state.
- Review inspects broad catches, fallback values, and logging payloads.
- Integration tests cover restart/retry after interrupted imports.

## 10. Evolution and Governance Principles

### AP-14: Testability by Design

**Statement:** Critical business policy and external boundaries must be independently testable without relying primarily on end-to-end testing.

**Rationale:** Parser and financial defects require fast, precise feedback; slow broad tests alone make failures difficult to locate.

**Required implications:**

- Domain rules are pure where practical and receive dependencies explicitly.
- Time, files, databases, PDF extraction, and exports are accessed through controllable boundaries.
- Repositories are tested against a real test SQLite database in addition to mocked use-case tests.
- Synthetic or irreversibly anonymized fixtures replace real user data.
- Tests assert observable contracts, not private implementation details.

**Verification:**

- Critical paths have unit plus boundary/integration coverage.
- A rule can be tested without rendering a widget.
- A repository can be tested without importing a real personal CAS Statement.

### AP-15: Evolution Without Speculative Complexity

**Statement:** Architecture must support foreseeable extension through stable seams, but must not implement future product features or generalized frameworks prematurely.

**Rationale:** Extensibility is valuable only when current behavior remains understandable and deliverable.

**Required implications:**

- Create an abstraction when it protects a real boundary, enables testing, or supports a known variation.
- Do not implement cloud, AI, OCR, multi-user, or future asset behavior as hidden Version 1 complexity.
- Prefer composition and small contracts over inheritance hierarchies.
- New asset or statement types should extend parser and domain strategies without destabilizing unrelated features.
- Refactoring is preferred when evidence reveals a better abstraction; speculative compatibility is not required.

**Verification:**

- ADRs identify the present need, not only hypothetical benefits.
- Reviews challenge unused interfaces, base classes, flags, and generic frameworks.
- Feature work remains traceable to current goals and scope.

### AP-16: Dependencies Are Liabilities

**Statement:** Every third-party dependency must provide clear value that outweighs its maintenance, security, privacy, licensing, size, and portability costs.

**Rationale:** Packages become part of the product's operational and legal surface, even when used for a small convenience.

**Required implications:**

- Prefer approved stack capabilities before adding a package.
- Record purpose, alternatives, maintenance status, license, platform support, and data behavior.
- Wrap significant infrastructure packages behind project-owned interfaces.
- Avoid overlapping packages that solve the same problem.
- Pin and upgrade dependencies deliberately, with regression testing.
- A significant dependency addition or replacement requires an ADR.

**Verification:**

- Dependency review accompanies changes to `pubspec.yaml`.
- Release review includes license and vulnerability checks.
- Removing or replacing an adapter does not require rewriting domain policy.

### AP-17: Documentation and Decisions Evolve Together

**Statement:** Architecture documentation, ADRs, implementation, and tests must describe the same system state.

**Rationale:** Stale guidance is especially hazardous for a documentation-first, AI-assisted project because it can reproduce obsolete decisions quickly.

**Required implications:**

- Significant design choices are recorded in ADRs before or alongside implementation.
- A feature change updates affected architecture, database, parser, UI, business-rule, and test documents.
- `docs/project_context.md` remains a concise entry point and links to current authoritative documents.
- Superseded decisions are marked rather than silently rewritten.
- Feature IDs, Goal IDs, principle IDs, and ADR IDs are used for traceability where applicable.
- AI-generated output is reviewed under the same standards as human-authored output.

**Verification:**

- Definition of Done includes documentation impact review.
- Pull requests reference affected principles and ADRs.
- Periodic documentation review checks links, status, and implementation alignment.

## 11. Principle Interaction Examples

### 11.1 Fast Import Versus Correct Import

AP-01 takes precedence over AP-12. Parallel or incremental processing is acceptable only if it preserves deterministic parsing, record ordering where relevant, validation, and transaction integrity.

### 11.2 Detailed Diagnostics Versus Privacy

AP-07 and AP-13 require traceability, while AP-02 protects sensitive content. Preserve structured source references, rule IDs, and redacted diagnostics in application data; do not solve auditability by logging raw statements.

### 11.3 Reuse Versus Feature Ownership

AP-06 and AP-15 favor local feature ownership until repeated, stable behavior demonstrates a shared abstraction. Similar-looking code is not automatically the same business concept.

### 11.4 Partial Import Versus Consistency

AP-08 does not decide whether partial import is allowed. That policy requires an ADR. If allowed, the accepted subset, rejected subset, warnings, provenance, and transaction boundary must remain explicit and consistent.

### 11.5 Extensibility Versus Minimal Dependencies

AP-15 and AP-16 favor project-owned boundaries around significant packages, not preemptive plugin frameworks or multiple competing implementations.

## 12. Compliance Review Checklist

Use the applicable parts of this checklist during design and review:

- [ ] Financial results reject or qualify missing and uncertain inputs (AP-01).
- [ ] Core behavior has no network dependency or hidden data transfer (AP-02).
- [ ] Export, deletion, replacement, and backup reflect explicit user intent (AP-03).
- [ ] Layer responsibilities and imports follow the approved direction (AP-04, AP-05).
- [ ] Cross-feature access uses a narrow public contract (AP-06).
- [ ] Stored and derived results have appropriate provenance (AP-07).
- [ ] Import/retry behavior is idempotent and transactionally consistent (AP-08).
- [ ] Validation occurs at each relevant trust boundary (AP-09).
- [ ] Parser variants remain isolated and regression-tested (AP-10).
- [ ] Calculations and recommendations are deterministic and explainable (AP-11).
- [ ] Heavy work is bounded and does not block the UI (AP-12).
- [ ] Failures are classified, visible, redacted, and recoverable (AP-13).
- [ ] Critical logic and adapters have proportional automated tests (AP-14).
- [ ] Abstractions and future seams solve demonstrated needs (AP-15).
- [ ] New dependencies have documented justification and review (AP-16).
- [ ] Documentation, ADRs, tests, and implementation are updated together (AP-17).

## 13. Exceptions and Governance

A principle exception is permitted only when all of the following are documented:

1. The affected principle IDs.
2. The concrete need and why compliant alternatives are insufficient.
3. Impact on correctness, privacy, reliability, performance, and maintainability.
4. Mitigations, tests, and monitoring or review conditions.
5. Whether the exception is temporary or permanent.
6. The approving ADR and any planned removal date.

An exception must not silently weaken a product constraint. If the proposed change alters offline operation, user data ownership, supported input, platform, or Version 1 scope, update the governing project documents before treating it as an architecture exception.

## 14. Traceability

| Principle Group | Primary Goals and Constraints |
| --- | --- |
| AP-01, AP-07-AP-11, AP-13 | UG-01, UG-03-UG-08; FG-02-13; DEV-03; reliability and transparency goals |
| AP-02, AP-03 | BG-01, BG-05; UG-09; BC-01, BC-02; FC-03; SEC-01-03 |
| AP-04-AP-06 | BG-02-04; TG-02, TG-03, TG-07; TC-04, TC-06 |
| AP-12 | PERF-01-04; PC-02, PC-03 |
| AP-14 | TG-09; DEV-03; release readiness criteria |
| AP-15, AP-16 | TG-08; TC-07; BC-03, BC-04; future growth constraints |
| AP-17 | DEV-01, DEV-02, DEV-04, DEV-05; documentation constraints |

Feature-specific documents should reference only the principles that materially affect their design.

## 15. Cross References

- `docs/project_context.md`
- `docs/01_Architecture/SolutionArchitecture.md`
- `docs/01_Architecture/ModuleArchitecture.md`
- `docs/01_Architecture/DataFlowArchitecture.md`
- `docs/01_Architecture/ImportPipelineArchitecture.md`
- `docs/00_Project/ProjectVision.md`
- `docs/00_Project/01_ProjectGoals.md`
- `docs/00_Project/02_ProjectScope.md`
- `docs/00_Project/04_ProjectConstraints.md`
- `docs/00_Project/05_TechnologyStack.md`
- `docs/00_Project/06_CodingStandards.md`
- `docs/00_Project/09_DevelopmentWorkflow.md`
- `docs/ADR/`

## 16. AI Development Notes

When using this document with an AI coding assistant:

- Include applicable principle IDs in the task or implementation plan.
- Ask the assistant to identify principle conflicts and open decisions before generating affected code.
- Do not let the assistant convert an unresolved business decision into an implementation default.
- Require tests that demonstrate compliance, especially for AP-01, AP-02, AP-08, AP-09, AP-11, and AP-13.
- Reject generated abstractions or packages justified only by generic best practices; require project-specific evidence under AP-15 and AP-16.
- Treat the compliance checklist as review input, not as proof that behavior is correct.

## 17. Revision History

| Version | Date | Author | Description |
| --- | --- | --- | --- |
| 0.1 | 2026-07-05 | Project Team | Initial draft of the architecture principles. |
