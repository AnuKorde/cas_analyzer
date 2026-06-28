# 02_ProjectScope.md

# CAS Analyzer

## Project Scope

**Document Version:** 1.0

**Status:** Draft

**Related Documents**

* README.md
* 00_ProjectVision.md
* 01_ProjectGoals.md

---

# 1. Purpose

This document defines the functional and non-functional scope of the CAS Analyzer project.

Its objectives are to:

* Clearly define what Version 1 will deliver.
* Identify functionality that is intentionally excluded.
* Establish boundaries for implementation.
* Prevent uncontrolled scope expansion.
* Provide guidance for future releases.

This document is the primary reference when deciding whether a proposed feature belongs in the current release.

---

# 2. Scope Definition

The scope of Version 1 is to deliver a fully offline Android application that enables users to import NSDL/CDSL Consolidated Account Statements (CAS), extract investment information, store it locally, analyze the portfolio, and present meaningful insights through a modern mobile interface.

No cloud services are required for the core functionality.

---

# 3. In Scope (Version 1)

## 3.1 PDF Import

The application shall allow users to:

* Select one or more CAS PDF files.
* Validate supported PDF formats.
* Display import progress.
* Prevent duplicate imports.
* Display import history.

---

## 3.2 CAS Parsing

The application shall:

* Read text-based NSDL/CDSL CAS PDFs.
* Identify statement sections.
* Extract structured investment data.
* Validate parsed information.
* Handle common statement variations.

---

## 3.3 Data Storage

The application shall:

* Store all extracted data in SQLite.
* Preserve historical transaction data.
* Maintain normalized database tables.
* Support future database migrations.

---

## 3.4 Holdings

Display:

* Equity holdings
* Mutual fund holdings
* ISIN
* Folio numbers
* Units
* Current balances
* Investment summaries

---

## 3.5 Transactions

Display:

* Purchase transactions
* Redemption transactions
* Switches
* Transfers
* Corporate actions (where available)

Support:

* Filtering
* Searching
* Sorting

---

## 3.6 Dashboard

The dashboard shall provide:

* Total portfolio value
* Total invested amount
* Number of holdings
* Asset allocation
* Portfolio summary
* Recent imports

---

## 3.7 Portfolio Analytics

Provide:

* Asset allocation
* Category allocation
* Sector allocation (where data is available)
* Investment summary
* Portfolio diversification overview

Advanced analytics (e.g., XIRR) may be introduced in later iterations if not ready for the initial release.

---

## 3.8 Recommendations

Version 1 recommendations include:

* Missing nominee detection
* Duplicate holdings
* Concentration warnings
* Portfolio diversification suggestions
* Inactive folios (if identifiable)

---

## 3.9 Reports

Allow users to:

* View summary reports.
* Export portfolio summaries.
* Export holdings.
* Export transaction history.

The export format (PDF, CSV, etc.) will be finalized during implementation.

---

## 3.10 Settings

Provide settings for:

* Theme (Light/Dark)
* Database management
* Import history
* Backup & restore (optional)
* About page

---

# 4. Non-Functional Scope

Version 1 shall:

* Operate completely offline.
* Store data locally.
* Avoid mandatory user registration.
* Avoid mandatory internet access.
* Be responsive on modern Android devices.
* Support large CAS statements.

---

# 5. Out of Scope (Version 1)

The following features are intentionally excluded:

## Market Data

* Live stock prices
* Live NAV updates
* Real-time market feeds

---

## Trading

* Buy/Sell orders
* Brokerage integration
* Order management

---

## Online Features

* Cloud synchronization
* Online backup
* Multi-device sync
* User accounts

---

## Financial Planning

* Retirement planning
* Goal planning
* Insurance planning
* Loan analysis

---

## Taxation

* Capital gains computation
* Income tax reports
* Tax filing integration

---

## Notifications

* Push notifications
* Market alerts
* Price alerts

---

## Collaboration

* Portfolio sharing
* Advisor collaboration
* Multi-user accounts

---

# 6. Future Scope

Potential future enhancements include:

## Portfolio Intelligence

* XIRR
* CAGR
* Risk analysis
* Performance benchmarking
* Goal tracking

---

## AI Features

* Portfolio assistant
* Natural language queries
* Investment recommendations
* Portfolio summaries
* Personalized insights

---

## Security

* Encrypted database
* Biometric authentication
* Secure backups

---

## Cloud Features

* Optional cloud backup
* Device synchronization
* Secure account login

---

## Additional Asset Classes

* Bonds
* ETFs
* Sovereign Gold Bonds
* REITs
* InvITs
* NPS
* International investments

---

# 7. Assumptions

This project assumes:

* Users possess standard text-based NSDL/CDSL CAS PDFs.
* The device has sufficient storage.
* SQLite is adequate for local persistence.
* Internet connectivity is not required for core functionality.

---

# 8. Constraints

The project is constrained by:

* Flutter mobile platform.
* Local device resources.
* Variations in CAS statement formats.
* Availability of data within the CAS statement.

---

# 9. Risks

Potential risks include:

* Unsupported CAS layout changes.
* Large PDF performance.
* Incomplete or inconsistent statement data.
* Incorrect parsing of uncommon statement formats.

Mitigation strategies will be defined in the parser design documentation.

---

# 10. Scope Management

Any proposed feature should be evaluated against the following questions:

1. Does it support the project vision?
2. Does it align with the project goals?
3. Does it fit within Version 1?
4. Does it increase architectural complexity?
5. Can it be deferred without reducing core value?

Features failing these checks should be scheduled for a later release.

---

# 11. Scope Prioritization

| Priority        | Description                                |
| --------------- | ------------------------------------------ |
| Must Have       | Essential for Version 1                    |
| Should Have     | Important but can be deferred if necessary |
| Could Have      | Valuable future enhancement                |
| Won't Have (V1) | Explicitly excluded from Version 1         |

This prioritization should guide implementation planning.

---

# 12. Acceptance Criteria

Version 1 is considered complete when users can:

* Import supported CAS statements.
* View parsed holdings.
* View transactions.
* Explore portfolio summaries.
* Receive basic portfolio recommendations.
* Export reports.
* Use the application entirely offline.

---

# 13. Traceability

Every implementation task should reference:

* Project Goals
* Project Scope
* Architecture
* Feature Specification

This ensures that implementation remains aligned with documented scope.

---

# 14. AI Development Notes

When implementing a feature:

* Verify that it is within the current scope.
* Avoid introducing functionality that belongs to future releases.
* Document any scope-related assumptions.
* Raise a design review if implementation reveals a gap in the documented scope.

---

# 15. Future Revisions

This document should evolve as the product roadmap changes.

Version 1.1 may include:

* Refined export capabilities
* Additional analytics
* Enhanced recommendation rules

Major scope expansions should be reflected in new release planning documents rather than by continuously expanding Version 1.

---

# Revision History

| Version | Date       | Author       | Description                    |
| ------- | ---------- | ------------ | ------------------------------ |
| 1.0     | 2026-06-28 | Project Team | Initial project scope document |
