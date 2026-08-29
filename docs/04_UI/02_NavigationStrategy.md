# Navigation Strategy

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document details the navigation architecture using the `go_router` package. It covers route definitions, nested navigation, and transition animations.

## 2. Route Configuration

The application uses declarative routing based on URLs. This makes deep linking (if needed later) and route state management simpler.

### 2.1 Main Routes

- `/`: The root path, resolving to the Dashboard if data exists, or the Onboarding/Import screen if empty.
- `/import`: The CAS PDF selection and progress screen.
- `/holdings`: List of all mutual fund and equity holdings.
- `/holdings/:id`: Detailed view of a specific holding.
- `/transactions`: Global chronological transaction ledger.
- `/settings`: Application preferences and data management.

## 3. Nested Navigation

A ShellRoute is used to implement a persistent Bottom Navigation Bar (or Navigation Rail on larger screens). The ShellRoute wraps the main features (`/`, `/holdings`, `/transactions`, `/settings`), keeping the navigation UI stable while switching tabs.

## 4. Route Guarding

Since CAS Analyzer is an offline-first app without user authentication (in V1), route guarding is minimal. However, a guard may be used to redirect users from the Dashboard (`/`) to the Import screen (`/import`) if the database is completely empty upon initial app launch.

