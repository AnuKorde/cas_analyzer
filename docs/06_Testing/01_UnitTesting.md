# Unit Testing Guidelines

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the guidelines for writing Unit Tests in CAS Analyzer. Unit tests form the foundation of the application's reliability.

## 2. Core Focus Areas

### 2.1 Parser Rules
Every regex and state machine in the `MOD-PARSER` must have exhaustive unit tests. 
- Input: Snippets of raw text (anonymized fixtures).
- Output: Validated `Candidate` models or expected `DiagnosticWarnings`.

### 2.2 Financial Calculations
Functions determining Current Value, Asset Allocation, and balance math must be tested against edge cases (e.g., zero balances, negative units from sells, missing prices).

### 2.3 Reconciliation Logic
Tests must verify that the duplicate detection and state merging logic (IP-09) correctly prevents double-counting transactions.

## 3. Mocking Strategy

Use `mocktail` to isolate the system under test.
- **Do not** talk to a real SQLite database in unit tests; mock the Repository interfaces.
- **Do not** use the real Syncfusion PDF extractor in parser unit tests; feed text fixtures directly.

## 4. Test Structure

Follow the `Arrange-Act-Assert` (or `Given-When-Then`) pattern for readability.

```dart
test('calculates total portfolio value correctly', () {
  // Arrange
  final mockHoldings = [ /* ... */ ];
  
  // Act
  final totalValue = calculatePortfolioValue(mockHoldings);
  
  // Assert
  expect(totalValue, 150000.50);
});
```

