# Parsing and Candidate Generation

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document details stage **IP-07 Parse Candidates** of the import pipeline. It explains how individual `SectionBlock`s are analyzed line-by-line using format-specific rules (regex, state machines) to generate structured domain models (Candidates).

## 2. Parsing Strategy

Parsing in CAS Analyzer favors correctness and explicit failure over silent data corruption.

### 2.1 Regex and Tokenization
- Regular expressions are used to match known line patterns (e.g., a transaction row with date, description, amount, units, balance).
- Named capture groups are preferred for readability.

### 2.2 State Machines
- Some sections span multiple lines (e.g., a multi-line transaction description). A state machine accumulates context until a complete record can be emitted.

### 2.3 Candidate Models
Parsers produce "Candidate" models rather than final Domain entities. Candidates represent "what the parser thinks it saw" and are decoupled from the database schema.

```dart
class TransactionCandidate {
  final DateTime date;
  final String description;
  final double amount;
  final double units;
  final double balance;
  final Provenance provenance;
}
```

## 3. Provenance and Diagnostics

To ensure explainability, every generated candidate must include a `Provenance` object indicating exactly where it came from (file, page, line number, and raw text).

```dart
class Provenance {
  final String sourceFile;
  final int pageNumber;
  final int lineNumber;
  final String rawText;
}
```

When a line cannot be parsed, instead of crashing or ignoring it silently, the parser emits a `DiagnosticWarning`. This allows the validation layer (IP-08) to decide if the failure is fatal or acceptable.

