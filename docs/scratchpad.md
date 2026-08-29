# Build architecture context

> Can you read the files from docs/00_project and build the context so that we can start create the architecture files?

<details><summary>2 previous messages</summary>

> I’ll inventory `docs/00_Project`, read its contents, and turn it into a concise architecture-ready context: goals, scope, constraints, actors, workflows, and unresolved decisions.
>
> I found 13 project documents. I’m reading them in related groups so I can distinguish product intent from engineering conventions and catch contradictions before we encode them into architecture.
</details>

I’ve read all 13 files in `docs/00_Project` and built the architecture context.

## Architecture baseline

CAS Analyzer is a privacy-first, offline Android application that:

- Imports text-based NSDL/CDSL CAS PDFs.
- Extracts investor, account, holding, transaction, nominee, and corporate-action data.
- Validates and deduplicates imports.
- Persists normalized historical data in SQLite.
- Presents dashboards, holdings, transactions, analytics, recommendations, and reports.
- Performs all core processing locally.

The primary quality priority is financial correctness, followed by privacy, responsiveness, maintainability, and extensibility.

## Required architecture

- Flutter and Dart
- Feature-based Clean Architecture
- Riverpod for state and dependency management
- GoRouter for navigation
- SQLite behind repository abstractions
- Syncfusion Flutter PDF for extraction
- Background processing for large 200–300-page statements
- Modular parsers for NSDL/CDSL format variations
- Testable business logic independent of Flutter
- Architecture decisions recorded as ADRs

Expected processing flow:

```text
PDF selection
→ validation and fingerprinting
→ text extraction
→ statement-format detection
→ section parsing
→ domain validation
→ transactional persistence
→ portfolio projections
→ analytics and recommendations
→ UI and exports
```

## V1 boundaries

Included:

- Single local user
- Multiple CAS imports
- Import history and duplicate prevention
- Equity and mutual-fund holdings
- Transactions and nominee information
- Basic offline analytics and rule-based recommendations
- Local reports and exports
- Light/dark themes

Excluded:

- Scanned/OCR PDFs
- Live prices or NAV
- Trading and brokerage integration
- User accounts and mandatory connectivity
- Cloud synchronization
- Tax and financial planning
- Advanced AI features
- Multi-user/advisor mode

## Important architecture uncertainties

These should be resolved or explicitly documented while creating the architecture:

1. “Current portfolio value” is requested, but live market prices and NAV are excluded. Its valuation date and source must be defined.
2. Password-protected CAS handling is not specified.
3. Import identity and duplicate semantics need definition—file hash, statement period, account, or content fingerprint.
4. Re-import and overlapping-statement reconciliation rules are missing.
5. Parser failure policy needs definition: atomic rejection versus partial import with warnings.
6. Sensitive investor data is local but database encryption is deferred; the V1 security posture needs an explicit ADR.
7. “Investment suggestions” may imply financial advice. V1 recommendations should likely remain deterministic, explainable observations with appropriate disclaimers.
8. Syncfusion licensing and supported platforms need confirmation.
9. Export formats and whether exports contain sensitive data are undecided.
10. Sector allocation requires reference metadata that may not exist in a CAS, conflicting with the offline/no-market-data boundary.

## Documentation issues noticed

- Several documents reference `00_ProjectVision.md`, but the actual file is `ProjectVision.md`.
- Both constraints and roadmap use the `04_` prefix.
- Some diagrams describe the domain as depending on a repository implementation. Clean Architecture should instead place repository interfaces in the domain and implementations in the data layer.
- The project structure has both global `models/repositories` and feature-owned models/repositories, which could create unclear ownership.
- Many features have only summary-level acceptance criteria; parser and persistence architecture will need stronger invariants.

The context is now sufficient to start `docs/01_Architecture`. I recommend beginning with:

1. `SolutionArchitecture.md`
2. `ArchitecturePrinciples.md`
3. `ModuleArchitecture.md`
4. `DataFlowArchitecture.md`
5. `ImportPipelineArchitecture.md`
6. `ErrorHandlingArchitecture.md`
7. `SecurityArchitecture.md`
8. Initial ADRs for Clean Architecture, SQLite, PDF extraction, background processing, and import reconciliation.

> Based on your understanding can you create project_context.md file? I want you to create this file so that you do not need to go through all the documentation folder. This file should be concise and you or any other AI coding assistant should be able to build the context of the project with effective way.
> I will keep updating this file as and when i add more fetures or architecture files.
> This file should be present at /docs/project_context.md

<details><summary>4 previous messages</summary>

> I’ll create a compact “front door” document at `docs/project_context.md`, optimized for AI onboarding while keeping the source documents authoritative. I’ll include scope, constraints, architecture guardrails, feature map, known ambiguities, and an update checklist.
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/project_context.md` (+248 -0)
>
> </details>
>
> The context file is drafted. I’ve deliberately separated settled guardrails from unresolved decisions, so future assistants won’t mistake gaps—especially valuation and import reconciliation—for established business rules. I’m doing a quick integrity check now.
>
> <details><summary>Edited a file</summary>
>
> - Wrote `./docs/project_context.md` (+3 -3)
>
> </details>
</details>

Created [project_context.md](./docs/project_context.md).

It includes:

- Product purpose and V1 scope
- Architecture and dependency rules
- Technology stack
- Privacy, security, and quality guardrails
- Feature delivery order
- Known unresolved decisions
- AI assistant working instructions
- Authoritative documentation references
- Maintenance checklist and revision history

Validation passed with no formatting or encoding issues.

> Now lets create architecture specific MD files as you mentioned above. Can you start with "SolutionArchitecture.md"

<details><summary>5 previous messages</summary>

> I’ll draft `SolutionArchitecture.md` as the top-level architectural map—system context, containers/components, dependency boundaries, key runtime flows, quality tactics, and traceability—while leaving unresolved business choices clearly marked for ADRs rather than smuggling them in as facts.
>
> The architecture templates are empty, so I’m using the project’s documentation standard directly. The document will be authoritative at the solution level but intentionally stop short of database schemas, parser grammars, and UI details—those belong in their dedicated design files.
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/01_Architecture/SolutionArchitecture.md` (+556 -0)
>
> </details>
>
> The first draft is in place. I’m checking Markdown hygiene, heading structure, Mermaid blocks, and whether the document accidentally contradicts the current context or claims unresolved decisions as settled.
>
> <details><summary>Edited a file</summary>
>
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
</details>

Created [SolutionArchitecture.md](./docs/01_Architecture/SolutionArchitecture.md).

It defines:

- System context and trust boundaries
- Clean Architecture layers and dependency rules
- Major feature modules
- Import and query flows
- Data, state, concurrency, and performance principles
- Reliability, security, and explainability requirements
- Testing and architecture enforcement
- Open decisions requiring ADRs
- Goal, constraint, and feature traceability

Also updated [project_context.md](./docs/project_context.md) to reference it. Markdown validation passed.

> Now lets create 2nd file from the list "ArchitecturePrinciples.md"

<details><summary>4 previous messages</summary>

> I’ll turn the solution’s guiding ideas into enforceable principles with stable IDs, rationale, concrete rules, and review checks. This file will answer “what must remain true?” while `SolutionArchitecture.md` continues to answer “how is the system arranged?”
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/01_Architecture/ArchitecturePrinciples.md` (+536 -0)
>
> </details>
>
> The principles now have stable IDs (`AP-01` through `AP-17`) and explicit compliance evidence. I’m updating the document graph so future assistants can discover them from both the solution overview and the compact project context.
>
> <details><summary>Edited 2 files</summary>
>
> - Wrote `./docs/01_Architecture/SolutionArchitecture.md` (+1 -1)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
</details>

Created [ArchitecturePrinciples.md](./docs/01_Architecture/ArchitecturePrinciples.md).

It defines 17 traceable principles (`AP-01`–`AP-17`) covering correctness, privacy, boundaries, imports, parsing, explainability, performance, testing, dependencies, and governance.

I also cross-linked it from:

- [SolutionArchitecture.md](./docs/01_Architecture/SolutionArchitecture.md)
- [project_context.md](./docs/project_context.md)

Formatting and structural validation passed.

> Lets complete 4th file from the list

<details><summary>4 previous messages</summary>

> I’m treating the fourth item in the original list as `DataFlowArchitecture.md` (with `ModuleArchitecture.md` intentionally left for later). I’ll define data classifications, lifecycle states, trust boundaries, command/query flows, provenance, and prohibited flows without settling the still-open reconciliation and valuation policies.
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/01_Architecture/DataFlowArchitecture.md` (+772 -0)
>
> </details>
>
> The data-flow design is drafted with stable flow IDs (`DF-01`–`DF-11`) and invariants (`DFI-01`–`DFI-12`). I’ve kept the important distinction between describing a flow and deciding its unresolved business policy—particularly partial imports, valuation, and overlap reconciliation. Now I’m wiring cross-references and validating the diagrams/document structure.
>
> <details><summary>Edited 4 files</summary>
>
> - Wrote `./docs/01_Architecture/ArchitecturePrinciples.md` (+1 -0)
> - Wrote `./docs/01_Architecture/SolutionArchitecture.md` (+1 -1)
> - Wrote `./docs/project_context.md` (+2 -0)
> - Wrote `./docs/01_Architecture/DataFlowArchitecture.md` (+1 -1)
>
> </details>
</details>

Created the fourth listed file: [DataFlowArchitecture.md](./docs/01_Architecture/DataFlowArchitecture.md).

It defines:

- Sensitive-data classifications and trust boundaries
- Canonical data lifecycle
- Flows `DF-01` through `DF-11`
- Import, query, analytics, recommendation, export, and deletion flows
- Provenance and transaction boundaries
- Concurrency, cancellation, caching, and failure propagation
- Prohibited flows and invariants `DFI-01` through `DFI-12`
- Open decisions and verification strategy

Cross-references and [project_context.md](./docs/project_context.md) were updated. Validation passed.

> I forgot to ask you to create 3rd file from the list. Can you create it?

<details><summary>5 previous messages</summary>

> Absolutely—the missing third file is `ModuleArchitecture.md`. I’ll define module ownership, allowed dependencies, public contracts, internal feature structure, and the dependency matrix so the feature-based Clean Architecture has concrete boundaries instead of becoming a folder-labeling exercise.
>
> One useful ambiguity surfaced in the approved project-structure document: it lists global `models/` and `repositories/` alongside feature-owned equivalents, and places use cases under `domain/` while the solution architecture distinguishes an application layer. I’ll make the module design explicit: feature-owned models/repositories, use cases in `application/`, and no global business-model dumping grounds. I’ll flag this as a documentation alignment item rather than quietly pretending the conflict does not exist.
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/01_Architecture/ModuleArchitecture.md` (+884 -0)
>
> </details>
>
> The module draft now resolves ownership down to public contracts and includes an allowed-dependency matrix plus invariants (`MI-01`–`MI-12`). I’m cross-linking it and checking the catalog/matrix for omissions or accidental cycles before handing it back.
>
> <details><summary>Edited 5 files</summary>
>
> - Wrote `./docs/01_Architecture/ArchitecturePrinciples.md` (+1 -0)
> - Wrote `./docs/01_Architecture/DataFlowArchitecture.md` (+1 -1)
> - Wrote `./docs/01_Architecture/ModuleArchitecture.md` (+1 -0)
> - Wrote `./docs/01_Architecture/SolutionArchitecture.md` (+1 -1)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
</details>

Created the missing third file: [ModuleArchitecture.md](./docs/01_Architecture/ModuleArchitecture.md).

It defines:

- 15 modules with stable `MOD-*` identifiers
- Standard feature-layer structure
- Module ownership and public contracts
- Allowed dependency matrix
- Repository, Riverpod, and navigation ownership
- Module invariants `MI-01`–`MI-12`
- Prohibited dependencies and enforcement rules
- Open architectural decisions

All architecture documents and [project_context.md](./docs/project_context.md) were cross-linked. Validation passed.

> Please create file number 5 from the list

<details><summary>4 previous messages</summary>

> File 5 is `ImportPipelineArchitecture.md`. I’ll expand `DF-01` into an implementation-ready pipeline: stage contracts, state machine, orchestration, background-work boundaries, transaction handoff, cancellation/retry semantics, diagnostics, and test gates—while preserving the open ADR decisions around identity, overlap, passwords, and partial imports.
>
> <details><summary>Created a file</summary>
>
> - Wrote `./docs/01_Architecture/ImportPipelineArchitecture.md` (+992 -0)
>
> </details>
>
> The pipeline design is in place with stages `IP-01`–`IP-12` and invariants `IPI-01`–`IPI-14`. The key safety boundary is now explicit in the document model: parser candidates cannot reach persistence; only a validated, reconciled, immutable accepted change set can cross the commit port. I’m updating the architecture index and validating consistency.
>
> <details><summary>Edited 5 files</summary>
>
> - Wrote `./docs/01_Architecture/ArchitecturePrinciples.md` (+1 -0)
> - Wrote `./docs/01_Architecture/DataFlowArchitecture.md` (+1 -1)
> - Wrote `./docs/01_Architecture/ModuleArchitecture.md` (+1 -0)
> - Wrote `./docs/01_Architecture/SolutionArchitecture.md` (+1 -1)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
</details>

Created file 5: [ImportPipelineArchitecture.md](./docs/01_Architecture/ImportPipelineArchitecture.md).

It defines:

- Pipeline stages `IP-01`–`IP-12`
- Import lifecycle and terminal outcomes
- Candidate, validation, reconciliation, and commit boundaries
- Progress, cancellation, retry, and restart semantics
- Background execution and bounded-resource strategy
- Multi-file import behavior
- Invariants `IPI-01`–`IPI-14`
- Testing, observability, and ADR requirements

All architecture documents and [project_context.md](./docs/project_context.md) were cross-linked. Validation passed.

> Please create 6th file from the list.

> I think architecture is complete. What should i start with?

<details><summary>3 previous messages</summary>

> Context compaction
>
> Source: automatic
> Status: completed
>
> I’ll do a quick read of the current docs layout and project context so the recommendation is grounded in what’s actually in your repo, not just my memory of our doc plan.
>
> I see the architecture files are now numbered (`00_...` through `06_...`). I’m going to skim the tail of the context plus architecture directory names to catch any mismatch before I suggest the next step.
</details>

Yes — if architecture is complete, I’d start with the database layer next.

My recommended next step:

1. First, fix a small documentation mismatch

Your architecture files are now numbered:

- `00_SolutionArchitecture.md`
- `01_ArchitecturePrinciples.md`
- `02_ModuleArchitecture.md`
- `03_DataFlowArchitecture.md`
- `04_ImportPipelineArchitecture.md`
- `05_ErrorHandlingArchitecture.md`
- `06_SecurityArchitecture.md`

But [project_context.md](./docs/project_context.md) still references the older unnumbered architecture filenames. I’d update that first so future AI/context loading doesn’t drift.

2. Then create database design docs

Start with:

[docs/02_Database/00_DatabaseArchitecture.md](./docs/02_Database/00_DatabaseArchitecture.md)

This should define:

- SQLite role in the app
- database ownership boundaries
- tables/modules mapping
- migration strategy
- transaction rules
- import atomicity
- provenance storage
- indexing strategy
- backup/restore implications
- what must not be stored/logged

Then follow with:

- `01_ConceptualDataModel.md`
- `02_PhysicalSchema.md`
- `03_MigrationStrategy.md`
- `04_DataRetentionAndCleanup.md`

Why database first? Because the parser, import pipeline, reconciliation, dashboard, holdings, transactions, analytics, recommendations, and reports all depend on the shape and guarantees of persisted data. It’s the spine of the app.

After database docs, I’d move in this order:

```text
Database design
  -> Parser design
  -> Business logic/calculation rules
  -> UI information architecture
  -> Testing strategy
  -> ADRs for major decisions
  -> Flutter project skeleton / implementation
```

So my pick: let’s fix `project_context.md` references, then create `docs/02_Database/00_DatabaseArchitecture.md`.

> let’s fix project_context.md references

<details><summary>3 previous messages</summary>

> Good catch to do this now — context files are the map other assistants will trust. I’ll update only the stale architecture references in `project_context.md`, then run a quick check for remaining old filenames.
>
> <details><summary>Edited a file</summary>
>
> - Wrote `./docs/project_context.md` (+8 -5)
>
> </details>
>
> The references are updated, including the two architecture files that existed but weren’t listed yet. I’m doing one pass for stale unnumbered paths and Markdown whitespace issues.
</details>

Done. I updated [project_context.md](./docs/project_context.md) to use the numbered architecture filenames:

- `00_SolutionArchitecture.md`
- `01_ArchitecturePrinciples.md`
- `02_ModuleArchitecture.md`
- `03_DataFlowArchitecture.md`
- `04_ImportPipelineArchitecture.md`
- `05_ErrorHandlingArchitecture.md`
- `06_SecurityArchitecture.md`

I also added the missing Error Handling and Security architecture references, plus revision history entry `1.6`.

Validation passed: no stale unnumbered architecture paths remain in `project_context.md`. Git only reported the usual LF/CRLF warning.

> Lets create  create docs/02_Database/00_DatabaseArchitecture.md

<details><summary>5 previous messages</summary>

> Perfect — database architecture is the right “spine” document next. I’ll ground it in the project context and architecture docs, then create `docs/02_Database/00_DatabaseArchitecture.md` with cross-links and revision history.
>
> I’ve got enough of the architecture spine. One useful thing popped out: some architecture cross-references still use old unnumbered names internally, but I’ll keep this task focused on the database doc and use the numbered paths correctly in the new file.
>
> I’m drafting it as the database-level contract, not a physical schema yet. So it will define responsibilities, persistence boundaries, transaction rules, identity/provenance expectations, migrations, and the open ADRs that block table-level details.
>
> <details><summary>Created a file, edited a file</summary>
>
> - Wrote `./docs/02_Database/00_DatabaseArchitecture.md` (+511 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> The database architecture doc is created and the context index now points to it. I’m doing the usual doc hygiene pass: Markdown shape, ASCII check, and diff check.
</details>

Created [00_DatabaseArchitecture.md](./docs/02_Database/00_DatabaseArchitecture.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Database architecture reference
- Revision history entry `1.7`

The new database architecture covers:

- SQLite’s role and ownership
- repository/data-source boundaries
- import transaction rules
- provenance and idempotency expectations
- duplicate/reconciliation concerns
- migration architecture
- privacy/security guardrails
- backup/restore/delete expectations
- database testing strategy
- open ADR decisions blocking physical schema work

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters found
- no stale unnumbered architecture references in the new database doc or project context

Next best file: `docs/02_Database/01_ConceptualDataModel.md`.

> Lets create docs/02_Database/01_ConceptualDataModel.md

<details><summary>4 previous messages</summary>

> Great — this is the “what are the business things we store?” layer before columns and SQL. I’ll base it on the database architecture plus the feature/catalog language, then add the context reference when the file is created.
>
> I’m going to keep this conceptual: entities and relationships, not table names/columns. That means it can guide the future physical schema without prematurely deciding import identity, precision, or reconciliation policy.
>
> <details><summary>Created a file, edited a file</summary>
>
> - Wrote `./docs/02_Database/01_ConceptualDataModel.md` (+549 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> The conceptual model is in place and linked from the project context. I’m running a final hygiene check for stale paths, non-ASCII, Mermaid/section structure, and diff status.
</details>

Created [01_ConceptualDataModel.md](./docs/02_Database/01_ConceptualDataModel.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Conceptual data model reference
- Revision history entry `1.8`

The new conceptual model defines:

- import batch / import attempt / CAS Statement
- provenance
- investor, investment account, demat/folio concepts
- instruments, mutual fund schemes, equities
- holdings, transactions, nominees, corporate actions
- diagnostics and schema metadata
- conceptual relationships with a Mermaid ER diagram
- source-reported vs derived data rules
- data classification and invariants
- query support expectations
- open ADR decisions before physical schema work

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters in the new file
- no stale unnumbered architecture references in the new file or project context

Next natural file: `docs/02_Database/02_PhysicalSchema.md`.

> Lets create docs/02_Database/02_PhysicalSchema.md

<details><summary>4 previous messages</summary>

> Nice, now we’re at the “blueprint starts becoming concrete” layer. I’ll create a draft physical schema doc that is table-oriented, but I’ll keep the unresolved decisions explicit so we don’t accidentally lock in identity, precision, or reconciliation rules too early.
>
> I’ve got the constraints loaded. The file will include a draft table catalog, naming/type conventions, foreign keys, indexes, views, transaction notes, and “blocked until ADR” markers where physical details would otherwise become guesswork.
>
> <details><summary>Created a file, edited a file</summary>
>
> - Wrote `./docs/02_Database/02_PhysicalSchema.md` (+744 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> Physical schema draft is created and linked. I’m doing the cleanup pass now — checking formatting, non-ASCII, stale links, schema invariant markers, and repo status.
</details>

Created [02_PhysicalSchema.md](./docs/02_Database/02_PhysicalSchema.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Physical schema reference
- Revision history entry `1.9`

The new schema blueprint includes:

- naming conventions
- standard columns
- SQLite type policy
- table groups
- draft tables for:
  - `schema_migrations`
  - `database_metadata`
  - `import_batches`
  - `import_attempts`
  - `cas_statements`
  - `source_provenance`
  - `investors`
  - `investment_accounts`
  - `account_statement_links`
  - `instruments`
  - `mutual_fund_schemes`
  - `equity_securities`
  - `holdings`
  - `transactions`
  - `nominations`
  - `corporate_actions`
  - `diagnostic_issues`
  - optional `backup_restore_events`
- relationship Mermaid diagram
- constraint/index strategy
- view/read-model strategy
- import commit order
- deletion/cleanup notes
- migration/privacy notes
- physical schema invariants `PHSI-01` through `PHSI-12`

I intentionally left identity, reconciliation, decimal precision, partial import, and valuation rules marked as pending ADR/design decisions, so we don’t quietly bake in financial assumptions.

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters in the new file
- no stale unnumbered architecture references in the new file or project context

Next natural file: `docs/02_Database/03_MigrationStrategy.md`.

> Lets create docs/02_Database/03_MigrationStrategy.md file

<details><summary>4 previous messages</summary>

> Absolutely — migration strategy is the “future-you does not hate present-you” document. I’ll tie it to the database architecture and physical schema, then add it to `project_context.md` so assistants know it exists.
>
> I’ve got the migration-related constraints. I’ll make this practical: versioning rules, startup flow, transaction/rollback expectations, test matrix, destructive-change policy, backup/restore interactions, and invariant checks.
>
> <details><summary>Created a file, edited a file</summary>
>
> - Wrote `./docs/02_Database/03_MigrationStrategy.md` (+443 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> Migration strategy is created and linked. I’ll run the doc hygiene checks now — same little broom pass as before.
</details>

Created [03_MigrationStrategy.md](./docs/02_Database/03_MigrationStrategy.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Migration strategy reference
- Revision history entry `1.10`

The new migration strategy covers:

- schema versioning model
- migration metadata
- startup database flow with Mermaid diagram
- initial database creation
- ordered migration rules
- transaction and rollback strategy
- migration types and destructive-change policy
- pre/post migration checks
- failed migration behavior
- unsupported future database versions
- downgrade policy
- backup/restore interaction
- privacy-safe migration diagnostics
- real SQLite migration test strategy
- migration review checklist
- invariants `MIGI-01` through `MIGI-12`

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters in the new file
- no stale unnumbered architecture references in the new file or project context

Next natural file: `docs/02_Database/04_RepositoryDesign.md`.

> Lets create docs/02_Database/04_RepositoryDesign.md

<details><summary>4 previous messages</summary>

> Great — this one becomes the contract between the clean architecture world and SQLite’s gritty little machinery. I’ll create the repository design doc, keeping domain ports separate from SQLite implementations and linking it back into `project_context.md`.
>
> I’ve got the repository/module boundary constraints. I’ll make the doc focus on meaningful repository ports, query contracts, transaction ownership, mapping rules, failure behavior, and testing — avoiding generic CRUD soup.
>
> <details><summary>Created a file, edited a file</summary>
>
> - Wrote `./docs/02_Database/04_RepositoryDesign.md` (+505 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> Repository design is created and indexed. I’ll run the same guardrail checks now: formatting, non-ASCII, stale links, invariant markers, and status.
</details>

Created [04_RepositoryDesign.md](./docs/02_Database/04_RepositoryDesign.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Repository design reference
- Revision history entry `1.11`

The new repository design covers:

- repository ownership rules
- command/query/metadata/maintenance repository categories
- planned repository ports
- query and command contract guidelines
- accepted import commit repository behavior
- transaction/unit-of-work boundaries
- domain-to-row mapping rules
- read-model strategy
- pagination/filtering expectations
- error handling and privacy-safe diagnostics
- dependency injection guidance
- data source responsibilities
- repository implementation layout
- repository testing strategy
- invariants `RDR-01` through `RDR-08` and `RDI-01` through `RDI-12`

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters in the new file
- no stale unnumbered architecture references in the new file or project context

Next natural file: `docs/02_Database/05_BackupRestoreAndCleanup.md`.

> Can you please create empty files under docs/02_Database/ directory so that i know which all files we will be creating

<details><summary>2 previous messages</summary>

> Yep — I’ll add placeholder files for the remaining database docs, without linking them as authoritative yet since they’ll be empty. First I’ll check what already exists so we don’t stomp on the real docs we’ve created.
>
> Based on the planned database set in `00_DatabaseArchitecture.md`, only one remaining database document is still missing: `05_BackupRestoreAndCleanup.md`. I’ll create it as a truly empty placeholder.
</details>

Created the empty placeholder:

- [05_BackupRestoreAndCleanup.md](./docs/02_Database/05_BackupRestoreAndCleanup.md)

Current `docs/02_Database/` plan is now visible:

- `00_DatabaseArchitecture.md`
- `01_ConceptualDataModel.md`
- `02_PhysicalSchema.md`
- `03_MigrationStrategy.md`
- `04_RepositoryDesign.md`
- `05_BackupRestoreAndCleanup.md`

I did not add the empty placeholder to `project_context.md` yet, since it has no content. Once we fill it, I’ll link it there properly.

> Letts create file: docs/02_Database/05_BackupRestoreAndCleanup.md

<details><summary>4 previous messages</summary>

> Absolutely — let’s turn that placeholder into the real backup/restore/cleanup design. I’ll keep backup/restore clearly marked optional for V1, and avoid inventing encryption/archive decisions that still need ADRs.
>
> I’ve got the relevant backup/security/migration constraints. I’ll make this doc cover three related but distinct things: backup, restore, and cleanup/deletion — with strong user-intent and rollback rules.
>
> <details><summary>Edited 2 files</summary>
>
> - Wrote `./docs/02_Database/05_BackupRestoreAndCleanup.md` (+400 -0)
> - Wrote `./docs/project_context.md` (+2 -0)
>
> </details>
>
> The file is now filled and linked from project context. I’ll run the final hygiene checks — formatting, ASCII, stale links, invariant markers, and status.
</details>

Created [05_BackupRestoreAndCleanup.md](./docs/02_Database/05_BackupRestoreAndCleanup.md).

I also updated [project_context.md](./docs/project_context.md) with:

- Backup, restore, and cleanup reference
- Revision history entry `1.12`

The new doc covers:

- V1 status for backup/restore vs required cleanup
- security boundary for backup/restore
- data classification
- backup architecture and manifest expectations
- backup protection open decisions
- restore stages and safety rules
- restore mode options
- cleanup categories and triggers
- deletion rules
- transaction/replacement strategy
- error handling and metadata rules
- user intent requirements
- testing strategy
- invariants `BRCI-01` through `BRCI-12`

Validation passed:

- no Markdown whitespace issues from `git diff --check`
- no non-ASCII characters in the new file
- no stale unnumbered architecture references in the new file or project context

Database docs now have a complete first pass.
