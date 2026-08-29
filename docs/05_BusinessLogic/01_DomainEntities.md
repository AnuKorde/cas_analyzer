# Domain Entities

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document catalogs the core Domain Entities used in CAS Analyzer. Entities encapsulate the most fundamental business rules and represent the accepted, validated truth.

## 2. Core Entities

### 2.1 Investor
Represents the individual or entity owning the investments.
- **Key Attributes:** Name, PAN, Address.

### 2.2 Folio / Demat Account
Represents the account holding the investments under a specific RTA or Depository.
- **Key Attributes:** Folio Number / DP ID, Investor reference, Nominee status.

### 2.3 Holding
Represents a specific asset owned by the investor at a point in time.
- **Key Attributes:** Asset Name (e.g., Mutual Fund Scheme), Units, Current Balance, ISIN, Asset Class (Equity, Debt, etc.).
- **Rules:** Units cannot be negative.

### 2.4 Transaction
Represents a historical action on a holding.
- **Key Attributes:** Date, Type (Buy, Sell, Dividend, SIP), Amount, Units, Running Balance.
- **Rules:** Must chronologically align with the holding's balance.

## 3. Value Objects

Value objects do not have a distinct identity; they are defined by their attributes.
- **Money:** Encapsulates currency amounts to prevent floating-point rounding errors.
- **Percentage:** Used for asset allocation calculations.

## 4. Entity Lifecycle

Entities are created exclusively by:
1. The Data Layer mapper (when reading from SQLite).
2. The Validation Layer (when upgrading parsed `Candidate` models into accepted `Entities` during import).

