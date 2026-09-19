# ADR-0001: Fixed-Scale Integer Financial Values

**Status:** Accepted  
**Date:** 2026-09-19

## Context

CAS data includes money, units, prices, NAVs, and percentages. Binary floating
point values and SQLite `REAL` can introduce rounding errors that are unacceptable
for persisted financial facts and reconciliation.

## Decision

Version 1 stores normalized financial values as signed SQLite `INTEGER` values
with an explicit, type-defined scale:

| Value | Scale | Example |
| --- | ---: | --- |
| INR amount | 2 | `₹123.45` → `12345` |
| Units/quantity | 6 | `1.234567` → `1234567` |
| Price or NAV | 6 | `12.345678` → `12345678` |
| Percentage | 4 | `12.3456%` → `123456` |

The source text and source scale are retained only in provenance where needed for
audit and parser diagnostics; normalized values are used for queries and
calculations. Parsing must reject or flag a value with greater precision rather
than silently round it. Arithmetic must use checked integer operations and an
explicit rounding rule only where a displayed result requires one.

`double`, `num`, and SQLite `REAL` are prohibited for persisted financial values,
identity inputs, reconciliation, and business calculations.

## Consequences

- Database columns and domain value objects must encode the applicable scale.
- Migration and parser tests need precision, overflow, sign, and locale fixtures.
- A later need for greater precision requires a schema/data migration and a new ADR.
