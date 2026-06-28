# 10_Glossary.md

# CAS Analyzer

## Project Glossary

**Document Version:** 1.0

**Status:** Approved

---

# 1. Purpose

This document defines the terminology used throughout the CAS Analyzer project.

Its objectives are to:

* Ensure consistent terminology.
* Reduce ambiguity.
* Support documentation.
* Improve communication.
* Assist AI-generated code and documentation.

Unless otherwise specified, the definitions in this glossary take precedence over informal usage.

---

# 2. Usage Guidelines

When writing documentation or code:

* Use the preferred terms defined here.
* Avoid introducing new terminology without updating this glossary.
* Use abbreviations consistently.
* Keep business terms distinct from technical terms.

---

# 3. Business Terms

| Term             | Definition                                                                             |
| ---------------- | -------------------------------------------------------------------------------------- |
| Asset            | A financial instrument owned by the investor.                                          |
| Asset Allocation | Distribution of investments across asset classes.                                      |
| CAS              | Consolidated Account Statement issued by NSDL/CDSL containing investment information.  |
| Demat Account    | Electronic account used to hold securities.                                            |
| Equity           | Ownership in a listed company.                                                         |
| Folio            | Unique identifier assigned by an Asset Management Company for mutual fund investments. |
| Holding          | Current ownership position in a financial instrument.                                  |
| Investment       | Money allocated to a financial product.                                                |
| Mutual Fund      | A professionally managed pooled investment vehicle.                                    |
| Nominee          | Person designated to receive assets upon the investor's death.                         |
| Portfolio        | Collection of all investments owned by the user.                                       |
| Redemption       | Sale or withdrawal of mutual fund units.                                               |
| SIP              | Systematic Investment Plan involving regular investments.                              |
| Transaction      | A recorded investment activity such as purchase, redemption, switch, or transfer.      |
| Units            | Quantity of mutual fund units held.                                                    |

---

# 4. Technical Terms

| Term               | Definition                                                              |
| ------------------ | ----------------------------------------------------------------------- |
| Clean Architecture | Layered architecture separating business logic from infrastructure.     |
| Data Source        | Component responsible for interacting with storage or external systems. |
| Domain             | Layer containing business rules and use cases.                          |
| Entity             | Core business object independent of frameworks.                         |
| Provider           | Riverpod object that exposes state or services.                         |
| Repository         | Abstraction over data access used by the domain layer.                  |
| Service            | Class implementing business operations or orchestration.                |
| SQLite             | Embedded relational database used for local storage.                    |
| Use Case           | A single business operation performed by the application.               |
| Widget             | Reusable Flutter UI component.                                          |

---

# 5. Flutter Terms

| Term             | Definition                                                  |
| ---------------- | ----------------------------------------------------------- |
| Build Method     | Flutter method that creates a widget tree.                  |
| Context          | `BuildContext` used to access widget hierarchy information. |
| Material Design  | Google's UI design system used by Flutter.                  |
| Provider Scope   | Root Riverpod container for dependency management.          |
| State            | Data that affects the UI and may change over time.          |
| Stateless Widget | Widget without mutable state.                               |
| Stateful Widget  | Widget with mutable state managed by Flutter.               |

---

# 6. Project Terms

| Term                | Definition                                                           |
| ------------------- | -------------------------------------------------------------------- |
| ADR                 | Architecture Decision Record documenting important design decisions. |
| Feature             | A user-visible capability defined in the Feature Catalog.            |
| Feature ID          | Unique identifier such as `FT-018`.                                  |
| Goal ID             | Identifier for a documented project goal (e.g., `BG-01`, `UG-03`).   |
| Implementation Task | A granular unit of development work.                                 |
| Module              | A logical grouping of related functionality.                         |
| Phase               | Major stage of the project lifecycle.                                |
| Roadmap             | Planned sequence of releases and milestones.                         |

---

# 7. Testing Terms

| Term                | Definition                                                                 |
| ------------------- | -------------------------------------------------------------------------- |
| Acceptance Criteria | Conditions that must be satisfied for a feature to be considered complete. |
| Integration Test    | Test validating interaction between multiple components.                   |
| Regression Test     | Test ensuring previously working functionality remains intact.             |
| Unit Test           | Test validating an individual class or function in isolation.              |
| Widget Test         | Flutter test verifying widget behavior.                                    |

---

# 8. Git Terms

| Term                | Definition                                              |
| ------------------- | ------------------------------------------------------- |
| Branch              | Independent line of development.                        |
| Commit              | Snapshot of changes in version control.                 |
| Conventional Commit | Standardized commit message format used by the project. |
| Main Branch         | Primary stable branch.                                  |
| Pull Request        | Proposed change submitted for review before merging.    |
| Tag                 | Named version marking a release.                        |

---

# 9. AI Terms

| Term                    | Definition                                                                      |
| ----------------------- | ------------------------------------------------------------------------------- |
| AI-Assisted Development | Using AI tools to accelerate software development while retaining human review. |
| Prompt                  | Instruction provided to an AI assistant.                                        |
| Prompt Library          | Collection of reusable prompts maintained by the project.                       |
| Human Review            | Manual verification of AI-generated output before acceptance.                   |

---

# 10. Abbreviations

| Abbreviation | Meaning                                        |
| ------------ | ---------------------------------------------- |
| ADR          | Architecture Decision Record                   |
| AMC          | Asset Management Company                       |
| API          | Application Programming Interface              |
| CAS          | Consolidated Account Statement                 |
| CI           | Continuous Integration                         |
| CRUD         | Create, Read, Update, Delete                   |
| DI           | Dependency Injection                           |
| ISIN         | International Securities Identification Number |
| MVP          | Minimum Viable Product                         |
| NAV          | Net Asset Value                                |
| PDF          | Portable Document Format                       |
| PR           | Pull Request                                   |
| SIP          | Systematic Investment Plan                     |
| SQL          | Structured Query Language                      |
| UI           | User Interface                                 |
| UX           | User Experience                                |

---

# 11. Preferred Terminology

Use the preferred term consistently throughout the project.

| Preferred     | Avoid                                                       |
| ------------- | ----------------------------------------------------------- |
| CAS Statement | Statement PDF                                               |
| Holding       | Stock Entry                                                 |
| Portfolio     | Investment Data                                             |
| Repository    | DAO (unless specifically referring to a Data Access Object) |
| Transaction   | Record                                                      |
| Use Case      | Business Method                                             |
| Widget        | Screen Component                                            |
| Feature       | Functionality Item                                          |

---

# 12. Naming Standards

Identifiers used across the documentation:

| Prefix | Description                  | Example  |
| ------ | ---------------------------- | -------- |
| BG     | Business Goal                | BG-01    |
| UG     | User Goal                    | UG-03    |
| FG     | Functional Goal              | FG-05    |
| TG     | Technical Goal               | TG-02    |
| FT     | Feature                      | FT-018   |
| ADR    | Architecture Decision Record | ADR-001  |
| TASK   | Implementation Task          | TASK-042 |
| TEST   | Test Case                    | TEST-017 |

These identifiers should remain stable throughout the project.

---

# 13. Relationship to Other Documents

This glossary supports all project documentation, including:

* Project Vision
* Project Goals
* Feature Catalog
* Architecture Documents
* Database Design
* Implementation Plans
* Test Plans

New terminology introduced in these documents should be added here.

---

# 14. AI Development Notes

When generating documentation or code:

* Use the preferred terminology from this glossary.
* Avoid creating alternate names for existing concepts.
* Reference Feature IDs and Goal IDs consistently.
* Keep business and technical terminology distinct.

---

# 15. Future Revisions

Future versions may include:

* Financial calculation terminology
* Database terminology
* Analytics terminology
* Security terminology
* Performance terminology
* Internationalization terms
* User interface terminology

The glossary should evolve alongside the project and remain the authoritative source for project vocabulary.

---

# Revision History

| Version | Date       | Author       | Description              |
| ------- | ---------- | ------------ | ------------------------ |
| 1.0     | 2026-06-28 | Project Team | Initial project glossary |
