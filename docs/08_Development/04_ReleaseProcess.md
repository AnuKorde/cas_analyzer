# Release Process

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the steps for versioning and building release artifacts (APKs/AABs) for CAS Analyzer.

## 2. Versioning Scheme

The project follows Semantic Versioning (`MAJOR.MINOR.PATCH`).
- **MAJOR:** Breaking changes to the database schema or core workflow that require explicit user migration actions.
- **MINOR:** New features (e.g., new charts, support for a new CAS format) that are backwards compatible.
- **PATCH:** Bug fixes and performance improvements.

The version is maintained in `app/pubspec.yaml` as `version: 1.0.0+1` (where `+1` is the build number).

## 3. Android Build Steps

When preparing a release for Android:

1. Update the version and build number in `pubspec.yaml`.
2. Ensure the working tree is clean and all tests pass (`flutter test`).
3. Run the release build command:
   ```bash
   flutter build apk --release
   ```
   *Note: For Play Store distribution, use `flutter build appbundle --release`.*

## 4. Keystore Management

- Release builds must be signed with the production keystore.
- **Security:** The `key.jks` file and its passwords (`key.properties`) must **never** be committed to version control. They should be injected via environment variables or a secure CI/CD pipeline secrets manager.

