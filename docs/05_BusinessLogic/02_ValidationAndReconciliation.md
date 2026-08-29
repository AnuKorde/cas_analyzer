# Validation and Reconciliation

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document explains stage **IP-08 Validate Domain** and **IP-09 Reconcile** of the import pipeline. It dictates how raw parsed data (`Candidates`) are checked for financial correctness and merged with existing database records.

## 2. Domain Validation (IP-08)

Before a parsed candidate becomes an entity, it must pass domain rules:
- **Balance Math:** Does the previous transaction balance plus the current transaction units equal the new transaction balance?
- **Completeness:** Does a folio have at least one valid holding or transaction? Are mandatory fields (like Folio Number) present?

If validation fails, the business logic generates a `DomainIssue` (e.g., `MathMismatchException`). The pipeline policy dictates whether to reject the entire file or skip the malformed record with a warning.

## 3. Reconciliation (IP-09)

When importing a new CAS statement, it may overlap chronologically with previously imported statements.

### 3.1 Duplicate Detection
- **File Level:** Detect if the exact same PDF (by hash or period) has been imported.
- **Transaction Level:** Match incoming transactions against existing ones using (Date, Folio, Amount, Units) to prevent double-counting SIPs or purchases.

### 3.2 State Merging
- Update the "Latest Balance" of a holding only if the incoming statement date is newer than the last recorded date for that folio.
- Merge nominee information if a newer statement provides updated details.

