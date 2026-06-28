# 06_CodingStandards.md

# CAS Analyzer

## Coding Standards

**Document Version:** 1.0

**Status:** Approved

---

# 1. Purpose

This document defines the coding standards for the CAS Analyzer project.

The objectives are to:

* Maintain a consistent coding style.
* Improve readability and maintainability.
* Simplify code reviews.
* Support AI-assisted development.
* Reduce defects through predictable design patterns.

All source code committed to the project should comply with these standards.

---

# 2. Guiding Principles

The project follows these principles:

* Readability over cleverness
* Simplicity over complexity
* Composition over inheritance
* Small classes
* Small methods
* Explicit over implicit behavior
* Testability first
* SOLID principles
* Clean Architecture

---

# 3. General Coding Rules

Every class should have a single responsibility.

Every method should perform one logical operation.

Avoid deeply nested code.

Avoid duplicated logic.

Prefer immutable data structures where practical.

Write self-documenting code.

Do not optimize prematurely.

---

# 4. Naming Conventions

## Classes

PascalCase

```dart
PortfolioRepository
PdfParser
HoldingService
```

---

## Methods

camelCase

```dart
parsePdf()

loadTransactions()

calculatePortfolio()
```

---

## Variables

camelCase

```dart
totalInvestment

currentHolding

transactionCount
```

---

## Constants

camelCase for `const` values.

```dart
const defaultPageSize = 25;
```

Use `SCREAMING_SNAKE_CASE` only when required for interoperability with external APIs or generated code.

---

## Files

snake_case

```text
portfolio_repository.dart

pdf_parser.dart

dashboard_screen.dart
```

---

## Folders

snake_case

```text
presentation

repository

analytics
```

---

# 5. Class Design

Each class should:

* Have one responsibility.
* Remain small and focused.
* Avoid unnecessary state.
* Be easy to test.
* Avoid direct UI dependencies.

Target guidelines:

* Maximum ~300 lines per class (exceptions allowed with justification).
* Maximum ~20 public methods.

If a class grows beyond these guidelines, consider refactoring.

---

# 6. Method Design

Methods should:

* Perform one operation.
* Return early when possible.
* Avoid deeply nested logic.
* Have descriptive names.

Guidelines:

* Aim for fewer than 40 lines.
* Limit parameters (prefer 4 or fewer; use parameter objects if needed).
* Avoid boolean flags that change method behavior.

---

# 7. Widget Design

Flutter widgets should:

* Be small.
* Be reusable.
* Avoid business logic.
* Receive data through constructors or providers.

Large screens should be composed of multiple widgets.

---

# 8. State Management

Business state belongs in Riverpod providers.

Widgets should:

* Render UI.
* Handle user interaction.

Business rules belong in services or use cases, not in widgets.

---

# 9. Architecture Rules

The dependency flow is strictly one direction:

```text
Presentation
      ↓
Use Cases
      ↓
Repositories
      ↓
Data Sources
      ↓
Database
```

The Presentation layer must never directly access SQLite.

---

# 10. Error Handling

Never ignore exceptions.

Use custom exception classes where appropriate.

Display user-friendly error messages.

Log technical details.

Do not expose stack traces to users.

---

# 11. Logging

Use structured logging.

Avoid:

* `print()`
* Logging sensitive financial information.
* Logging personally identifiable information.

Use appropriate log levels:

* Debug
* Info
* Warning
* Error

---

# 12. Null Safety

Enable and embrace Dart null safety.

Avoid unnecessary null assertions (`!`).

Prefer explicit null handling.

---

# 13. Asynchronous Code

Use `async` / `await`.

Avoid deeply nested Futures.

Handle timeouts where appropriate.

Avoid blocking the UI thread.

---

# 14. Dependency Injection

Dependencies should be injected through constructors or Riverpod providers.

Avoid service locators and global mutable state.

---

# 15. Database Access

Database access must:

* Go through repositories.
* Use transactions where appropriate.
* Be isolated from UI.

No widget should execute SQL.

---

# 16. Testing Standards

Every business rule should be testable.

Recommended priorities:

1. Unit tests
2. Widget tests
3. Integration tests

Critical parsing and calculation logic should have high test coverage.

---

# 17. Documentation Standards

Public classes and complex methods should include documentation comments.

Explain *why* when the intent is not obvious.

Avoid redundant comments that merely restate the code.

---

# 18. Code Review Checklist

Before merging:

* Builds successfully.
* Passes tests.
* Follows architecture.
* No duplicated logic.
* Appropriate naming.
* Documentation updated (if applicable).
* No unnecessary dependencies.
* No debugging code left behind.

---

# 19. AI Coding Standards

AI-generated code must:

* Follow project architecture.
* Use approved technologies.
* Follow naming conventions.
* Avoid introducing unnecessary packages.
* Include tests where appropriate.
* Be reviewed before merging.

AI-generated code is treated the same as manually written code.

---

# 20. Performance Guidelines

Avoid:

* Unnecessary widget rebuilds.
* Expensive operations in `build()`.
* Large synchronous computations on the UI thread.

Prefer lazy loading and efficient collection handling where appropriate.

---

# 21. Security Guidelines

Never:

* Hardcode secrets.
* Log sensitive user data.
* Store credentials in plain text.

Validate all external input before processing.

---

# 22. Linting & Formatting

Use:

* `dart format`
* Flutter recommended lints

The codebase should remain consistently formatted.

---

# 23. Technical Debt

Technical debt should be:

* Identified.
* Documented.
* Tracked.
* Addressed in future iterations.

Avoid accumulating undocumented shortcuts.

---

# 24. Relationship to Other Documents

This document complements:

* 00_DocumentationStandards.md
* 05_TechnologyStack.md
* 07_GitWorkflow.md
* 09_DevelopmentWorkflow.md

---

# 25. AI Development Notes

When prompting AI:

* Reference the relevant architecture document.
* Specify the target layer (Presentation, Domain, Data).
* Request tests for business logic.
* Ask AI to explain non-obvious design choices.

Review every generated change before committing.

---

# 26. Future Revisions

Future versions may include:

* Dart analyzer configuration
* Package-specific conventions
* Performance benchmarking rules
* Accessibility coding standards
* Localization guidelines

---

# Revision History

| Version | Date       | Author       | Description              |
| ------- | ---------- | ------------ | ------------------------ |
| 1.0     | 2026-06-28 | Project Team | Initial coding standards |
