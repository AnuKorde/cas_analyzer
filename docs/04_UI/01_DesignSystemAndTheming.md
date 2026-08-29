# Design System and Theming

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the visual design language, theming strategy, and shared component guidelines for CAS Analyzer.

## 2. Theming Strategy

CAS Analyzer relies on Flutter's `ThemeData` to provide a consistent look and feel. 

- **Light and Dark Mode:** Full support for both modes is required in Version 1. 
- **Color Palette:** The palette should convey trust and financial stability (e.g., using subtle blues and greens) while maintaining high contrast for readability.
- **Typography:** Standard Material 3 typography scales will be used. Readability of numbers (using tabular figures if possible) is a priority for financial data.

## 3. Shared Component Library

To maintain consistency and speed up development, generic UI elements belong in `app/lib/shared/`:

- **Buttons:** Primary, secondary, and text buttons.
- **Cards:** Elevated containers for portfolio summaries and holding details.
- **Data Tables:** Standardized widgets for displaying transaction histories and paginated data.
- **Dialogs & Snackbars:** Consistent mechanisms for warnings, confirmations, and success messages.
- **Empty States:** Friendly and instructive screens when no data is available (e.g., before the first CAS import).

## 4. Accessibility (A11y)

- All interactive widgets must have adequate tap targets (minimum 48x48 logical pixels).
- Text scaling must be supported without breaking layouts.
- Screen reader semantics (Semantics widget) should be applied to custom visual components like charts.

