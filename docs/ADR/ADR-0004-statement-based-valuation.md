# ADR-0004: Statement-Based Portfolio Valuation

**Status:** Accepted  
**Date:** 2026-09-19

## Context

Version 1 works offline and has no market-price or NAV feed. Calling a statement
value "current" would imply freshness and precision the application does not have.

## Decision

The Version 1 dashboard shows **Reported portfolio value as of [statement end
date]**. It is derived only from the latest successfully committed complete CAS
Statement by statement end date, using holding values reported by that statement.
Import time is not a valuation input.

When a holding has no reported value, the application may derive one only from
units and NAV reported in the same statement and must retain that derivation
provenance. If neither source value nor approved same-statement derivation is
available, the value is unavailable. Values from different statement dates are
never combined to fabricate a portfolio total.

Version 1 does not display return, gain/loss, XIRR, CAGR, or a live value. It may
display net cash invested only when transaction coverage is sufficient, and must
label it as such rather than cost basis or performance.

## Consequences

- Holding snapshots must retain statement end date and valuation provenance.
- Dashboard and exported CSVs must include the statement date and value label.
- A future live-value feature requires market-data provenance and a new ADR.
