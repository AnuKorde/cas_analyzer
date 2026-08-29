# Feature Workflow

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document details the standard workflow for adding new features, fixing bugs, and integrating changes into the CAS Analyzer codebase.

## 2. Branching Strategy

We follow a lightweight feature-branch workflow based off `main`.
- **Main Branch:** `main` represents the stable, deployable state of the application.
- **Feature Branches:** Create a branch for every new task: `feature/FT-001-import-ui` or `bugfix/fix-parser-crash`.
  - Always include the Feature ID or Goal ID if applicable.

## 3. The Development Cycle

1. **Document First:** If the feature requires architectural changes, update the relevant Markdown files in `docs/` or create an ADR *before* coding.
2. **Implement:** Write the code, adhering to Clean Architecture principles.
3. **Test:** Write Unit/Widget tests for the new logic. Run `flutter test` to ensure no regressions.
4. **Format:** Run `dart format .` and ensure the `flutter analyze` linter reports zero issues.
5. **Commit:** Use descriptive commit messages.

## 4. Pull Requests (PRs)

- Keep PRs small and focused on a single responsibility.
- A PR must pass all CI checks (Formatting, Linting, Tests) before it can be merged.
- At least one code review is required. Reviewers must check for architectural violations (e.g., UI directly calling the database).

