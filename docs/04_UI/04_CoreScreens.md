# Core Screens

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document catalogs the primary user-facing screens for Version 1 of CAS Analyzer and their core responsibilities.

## 2. Main Dashboard

The entry point after data is loaded.
- **Summary Cards:** Total portfolio value, day's change (if applicable), and total invested amount.
- **Asset Allocation:** A pie or donut chart (`fl_chart`) showing the split between Equity, Debt, Hybrid, etc.
- **Recent Activity:** A preview of the most recent transactions.
- **Actionable Insights:** Alerts for missing nominees or portfolio concentration risks.

## 3. Import Orchestrator

The workflow for adding new statements.
- **Selection:** A file picker interface to select a CAS PDF.
- **Progress View:** A stepped indicator showing validation, extraction, parsing, and database insertion.
- **Success/Failure Summary:** Displays how many folios/transactions were imported, or clear diagnostics if the import failed.

## 4. Holdings Details

- **Holdings List:** A searchable, sortable list of all active investments, displaying current balance and units.
- **Holding Detail Screen:** Tapping a specific fund shows its history, a localized chart of accumulation, and its specific transactions.

## 5. Global Transactions

- A unified chronological ledger of all actions (buys, sells, dividends) across all folios.
- Includes filtering by date range, transaction type, and folio name.

## 6. Settings & Data Management

- Toggle Light/Dark mode.
- View import history and logs.
- Provide options to delete all local data (factory reset) or trigger manual database backups.

