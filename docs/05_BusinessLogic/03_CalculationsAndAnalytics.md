# Calculations and Analytics

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the business logic for calculating portfolio metrics. Since CAS Analyzer V1 operates offline without live market feeds, calculations rely strictly on the data present in the imported CAS statements.

## 2. Core Calculations

### 2.1 Current Value
Because live NAVs (Net Asset Values) are not fetched, the "Current Value" of a holding is defined as the `Units * NAV` on the closing date of the most recently imported CAS. The UI must clearly indicate the "As of [Date]" to avoid misleading the user.

### 2.2 Asset Allocation
Holdings are categorized into broader asset classes (Equity, Debt, Hybrid, Gold, etc.). 
- The analyzer will use pattern matching on the fund name or RTA classification present in the CAS to determine the asset class.
- Allocation is calculated as a percentage of the total portfolio value.

### 2.3 Invested Amount
Calculated by summing all `Buy` and `SIP` transactions, minus the cost basis of `Sell` transactions.

## 3. Advanced Analytics (Future / Deferred)

- **XIRR (Extended Internal Rate of Return):** Requires an accurate history of all cash flows (transactions) and the current value. If transaction history is incomplete (e.g., CAS only covers the last month), XIRR cannot be accurately calculated and should be disabled or explicitly marked as an estimate.

