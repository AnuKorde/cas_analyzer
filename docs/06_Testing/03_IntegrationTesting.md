# Integration Testing

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the strategy for End-to-End (E2E) integration tests using the `integration_test` package. These tests run on real emulators/devices to verify that all layers of the application work together.

## 2. Core Workflows to Test

Because E2E tests are slow, we only test the most critical "Golden Paths":
1. **The Import Pipeline:** Selecting a dummy CAS, validating progress indicators, parsing, saving to SQLite, and navigating to the populated Dashboard.
2. **Navigation Flow:** Moving between the Dashboard, Holdings, and Transactions tabs.

## 3. Dealing with File Pickers

Integration tests cannot easily interact with the native Android file picker dialog. 
- **Solution:** The `FilePicker` service must be mocked at the application boundary for integration tests. Instead of opening the native picker, the mock will return the path to a bundled dummy PDF asset.

## 4. Database State

- Integration tests must run against an in-memory SQLite database or a temporary file that is completely wiped before each test run.
- This ensures test isolation and prevents cross-test contamination.

