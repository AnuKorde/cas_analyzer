# Code Quality and Linting

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document defines the coding standards, linting rules, and quality expectations for Dart code in the CAS Analyzer project.

## 2. Formatting

All Dart code must be formatted using the standard Dart formatter.
- Run `dart format .` before committing.
- Do not engage in arguments about formatting; defer to the automated tool.

## 3. Linting

The project uses the `flutter_lints` package, augmented with stricter rules in `analysis_options.yaml`.

**Key Strict Rules:**
- `avoid_print`: Use the `logger` package instead.
- `always_declare_return_types`: Ensure method signatures are explicit.
- `avoid_dynamic_calls`: Maintain type safety.
- `require_trailing_commas`: Helps the formatter produce cleaner diffs.

## 4. Null Safety

- **Avoid the Bang Operator (`!`):** Using `!` bypasses null safety and causes runtime crashes. Use `if (value != null)` checks, default values (`??`), or early returns instead.
- **Late Initialization:** Use `late` sparingly. Only use it when you can absolutely guarantee the variable will be initialized before use (e.g., in `initState`).

## 5. Error Handling

- Never swallow exceptions with an empty `catch` block.
- Map infrastructure exceptions (like `DatabaseException`) into custom Domain exceptions (like `StorageFailure`) before they cross the architectural boundary to the UI.

