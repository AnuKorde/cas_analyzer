# State Management

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document explains how CAS Analyzer uses `Riverpod` for state management, dependency injection, and asynchronous data handling within the Presentation Layer.

## 2. Riverpod Overview

Riverpod is used to expose domain use cases, repositories, and UI state controllers to the widget tree in a safe, compile-time verified manner.

## 3. Asynchronous Data Handling

Because the app relies heavily on a local SQLite database, most data fetching is asynchronous. 

- `FutureProvider` and `AsyncNotifierProvider` are the primary tools for loading data.
- Widgets observe these providers and use Riverpod's `.when()` method to map the state into three distinct UI branches:
  1. `data`: Render the main content.
  2. `loading`: Render shimmer effects or circular progress indicators.
  3. `error`: Render a localized error message with a retry action.

## 4. UI State Controllers

Complex screens (like the Import Orchestrator or filtering the Holdings list) use `Notifier` or `AsyncNotifier` classes to encapsulate presentation logic. 

- **Inputs:** The controller exposes methods (e.g., `startImport(file)`, `applyFilter(query)`).
- **Outputs:** The controller mutates its internal state, which Riverpod then pushes to listening widgets.
- **Dependencies:** Controllers read Application Use Cases or Repositories via the `ref` object.

## 5. Global vs. Local State

- **Global State:** Themes, User Settings, and active portfolio summaries are provided globally.
- **Local State:** Tab indices, scroll positions, and temporary text field inputs use standard Flutter `StatefulWidget`s, as they do not need to outlive the screen or be shared across features.

