# Rule-Based Recommendations

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document describes the engine that generates actionable insights and warnings based on the user's portfolio. These are deterministic, rule-based checks, not AI-generated financial advice.

## 2. Recommendation Engine

The recommendation engine runs a series of isolated `InsightRule` classes against the `Portfolio` aggregate. Each rule returns a list of `Insight` objects.

```dart
abstract class InsightRule {
  List<Insight> evaluate(Portfolio portfolio);
}
```

## 3. V1 Supported Rules

### 3.1 Missing Nominee Rule
- **Trigger:** A `Folio` or `Demat Account` has the nominee status marked as "Not Registered" or empty.
- **Action:** Generates a High-Priority alert advising the user to update their nomination with the specific AMC/RTA.

### 3.2 Portfolio Concentration Rule
- **Trigger:** A single holding or single asset class (e.g., Equity) exceeds a predefined threshold (e.g., 40% of the total portfolio).
- **Action:** Generates a Medium-Priority warning highlighting potential lack of diversification.

### 3.3 Overlapping Funds (Basic)
- **Trigger:** User holds multiple funds in the exact same category (e.g., three different "Large Cap" funds).
- **Action:** Generates a Low-Priority note suggesting consolidation. (Note: Relies on accurate categorization).

## 4. Disclaimers

All insights must be accompanied by a clear UI disclaimer stating that the application provides algorithmic analysis based on limited data, not professional financial advice.

