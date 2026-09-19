# ADR-0003: Atomic Import Policy

**Status:** Accepted  
**Date:** 2026-09-19

## Context

Partially parsed financial data can create misleading holdings, transactions, and
recommendations. The product prioritizes financial correctness over permissive
import completion.

## Decision

An import is atomic for all material financial data. If the parser cannot
validate the statement identity, account data, holdings, or transaction records
needed by the supported layout, the entire import is rejected and no accepted
portfolio data is written.

Unknown, irrelevant, or unsupported sections may be tolerated only when they
cannot change a Version 1 holding, transaction, nominee status, valuation, or
recommendation. Such sections produce safe, user-visible warnings and retained
redacted diagnostics. They never silently become accepted financial records.

SQLite persistence occurs in one transaction after validation and reconciliation.
Cancellation or failure before commit leaves no accepted mutation.

## Consequences

- The initial parser must declare exactly which layouts and sections it supports.
- Import UX distinguishes `completed with warnings`, `duplicate`, and `rejected`.
- Tolerated-warning rules require unit tests; a future partial-financial import
  policy requires a superseding ADR.
