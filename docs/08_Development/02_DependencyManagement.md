# Dependency Management

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document establishes rules for introducing and managing third-party Dart/Flutter packages (`pubspec.yaml`). 

## 2. Core Philosophy

**Keep the dependency footprint small.** Every package added is a liability regarding maintenance, security, and app size. Because this is an offline-first financial app, we must be highly scrutinizing of third-party code.

## 3. Adding New Packages

Before adding a package, ask:
1. **Is it necessary?** Can this be solved with a few lines of standard Dart code?
2. **Is it maintained?** Does it have recent updates, a high pub.dev score, and active maintainers?
3. **Does it require network access?** If it includes analytics or phone-home telemetry, it is strictly forbidden.

**Major Additions:** Adding a core package (e.g., changing the database engine from SQLite, adding a new charting library) requires an Architecture Decision Record (ADR) under `docs/ADR/` to document the justification.

## 4. Known Core Dependencies

- `riverpod` / `flutter_riverpod`: State management and DI.
- `go_router`: Navigation.
- `sqflite`: Database.
- `syncfusion_flutter_pdf`: PDF extraction (Subject to licensing review).
- `fl_chart`: Data visualization.
- `path`: Database file-path construction.
- `shared_preferences`: Non-sensitive application preferences only.
- `logger`: Redacted local diagnostics only.
- `mocktail`: Test doubles.

`http` and `flutter_secure_storage` are not approved core dependencies. They must
not be added without a documented need and the applicable ADR or security review.

## 5. Version Pinning

Use caret syntax (`^`) for compatible updates, but avoid blanket `any` versions.
The committed `pubspec.lock` in `app/` is the resolved dependency record for an
application release. Regularly update dependencies and run the test suite to
catch breaking changes early.

