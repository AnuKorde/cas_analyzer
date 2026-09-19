# Calculations and Analytics

**Document Version:** 1.0

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the business logic for calculating portfolio metrics. Since CAS Analyzer V1 operates offline without live market feeds, calculations rely strictly on the data present in the imported CAS statements.

## 2. Core Calculations

### 2.1 Reported Portfolio Value

Version 1 has no live prices or NAVs. A displayed portfolio value is therefore a
**reported portfolio value**, not a live or current market value.

- The dashboard selects the latest successfully committed, complete CAS
  Statement by statement end date; import time never determines valuation.
- It uses the holding values reported by that statement. `units * NAV` may be
  used only when both are reported by that same statement and the parser marks
  the value as derived from those source fields.
- The UI must display the statement end date and label the value "Reported as of
  [date]". It must not call it current value, live value, return, or performance.
- If a complete statement does not report a value and no approved same-statement
  derivation is available, the value is unavailable. The application must not
  fill it with a value from a different statement date.
- A later import with the same statement end date but conflicting holding values
  is a reconciliation conflict, not a replacement chosen by import time.

### 2.2 Asset Allocation
Holdings are categorized only by an explicit, supported asset classification in
the imported CAS. Holdings without one are shown as `Unclassified`; the
application must not infer a class from an instrument name or external data in
Version 1. Allocation is calculated as a percentage of the reported portfolio
value for the selected statement date, with unclassified value displayed
separately.

### 2.3 Net Cash Invested

Version 1 may display **net cash invested** only when the imported transaction
history is complete enough for that account and statement coverage. It is the
sum of source-reported purchase/SIP cash outflows minus source-reported
redemption cash inflows. It is not cost basis, realized gain, unrealized gain,
or performance. If coverage is incomplete or a transaction amount is missing,
the metric is unavailable rather than estimated.

## 3. Advanced Analytics (Future / Deferred)

- **XIRR (Extended Internal Rate of Return):** Requires an accurate history of all cash flows (transactions) and the current value. If transaction history is incomplete (e.g., CAS only covers the last month), XIRR cannot be accurately calculated and should be disabled or explicitly marked as an estimate.

## 4. Revision History

| Version | Date | Description |
| --- | --- | --- |
| 1.0 | 2026-09-19 | Defined Version 1 statement-based valuation and net-cash-invested semantics. |
| 0.1 | 2026-08-29 | Initial draft. |

