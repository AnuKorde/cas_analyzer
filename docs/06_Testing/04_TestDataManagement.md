# Test Data Management

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document dictates how test data (fixtures, mocks, and PDFs) is managed, with a strict emphasis on preventing the accidental commit of Personally Identifiable Information (PII) or real financial data to the version control system.

## 2. Strict Privacy Rule

**Under no circumstances should a real, unedited CAS statement be committed to the repository.**
Doing so compromises investor privacy and violates the core tenets of this offline-first application.

## 3. Anonymization Strategy

To create test fixtures:
1. **Synthetic PDFs:** Create completely fake CAS PDFs using dummy data (e.g., "John Doe", "Folio 1234567") that structurally match NSDL/CDSL formats.
2. **Sanitized Text:** If extracting text snippets for unit testing parsers, manually replace all names, PANs, Folio numbers, and amounts with synthetic values before saving the `.txt` fixture.

## 4. Fixture Organization

Test data should be stored in the `test/fixtures/` directory:
- `test/fixtures/pdfs/`: Synthetic CAS PDFs for integration testing.
- `test/fixtures/text/`: Sanitized raw text chunks for parser unit tests.
- `test/fixtures/json/`: Expected output models for asserting parser correctness.

## 5. Mock Generators

For unit and widget testing, use factory packages or helper functions to generate varied domain entities (`Holding`, `Transaction`) quickly, reducing boilerplate.

