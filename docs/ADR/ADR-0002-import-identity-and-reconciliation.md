# ADR-0002: Import Identity and Overlap Reconciliation

**Status:** Accepted  
**Date:** 2026-09-19

## Context

Users can import renamed copies of the same CAS Statement and statements whose
periods overlap. The application must prevent duplicate data without treating
import time as a financial fact.

## Decision

1. An exact duplicate import is a SHA-256 digest of the complete selected file.
   The digest is calculated while reading the file; a matching committed digest
   returns `duplicate` and creates no portfolio mutation.
2. A committed import is immutable and records issuer, parser version, statement
   period, and a non-sensitive import identifier. File names and paths are not
   identity inputs.
3. Transactions use a deterministic semantic fingerprint over issuer, account
   identity, instrument identity, transaction type, effective date, normalized
   amount, normalized quantity, and the source reference when present. Matching
   records are one canonical transaction with provenance links to every source
   statement that reported them.
4. Holding snapshots are keyed by account, instrument, and statement end date.
   Equivalent snapshots merge provenance. Conflicting values for the same key
   are a reconciliation conflict and reject the import; import time never chooses
   a winner.
5. A newer statement end date can provide a newer snapshot, but it never deletes
   historical transactions or snapshots.

## Consequences

- Renamed copies are safely rejected; semantically similar but byte-different
  statements are imported and reconciled at record level.
- Tables require unique constraints for file digest, transaction fingerprint, and
  holding-snapshot key plus provenance join tables.
- Account identity components must be normalized and protected from logs.
