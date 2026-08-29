# Testing Strategy

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document defines the overall testing strategy for CAS Analyzer. It outlines the testing pyramid, tools, and the philosophy behind ensuring financial correctness and application stability.

## 2. Testing Philosophy

- **Test-Driven Development (TDD):** Encouraged for complex parser rules and financial calculations.
- **Financial Correctness First:** The highest priority for testing is the Business Logic (Domain) and the Parser. A UI glitch is acceptable; an incorrect portfolio calculation is not.
- **Privacy in Testing:** Real CAS statements contain highly sensitive Personally Identifiable Information (PII) and financial data. **Real statements must never be committed to the repository.**

## 3. The Testing Pyramid

1. **Unit Tests (Most numerous):** Fast, isolated tests focusing on pure Dart classes (Parsers, Calculations, Use Cases).
2. **Widget Tests (Moderate):** Component-level tests verifying UI rendering and interactions, mocking the business logic layer using Riverpod.
3. **Integration Tests (Fewest):** End-to-end (E2E) tests running on a real device/emulator to verify the entire flow from PDF import to Dashboard rendering.

## 4. Tool Stack

- **Framework:** `flutter_test` (Unit and Widget tests).
- **Mocking:** `mocktail` (for mocking Repositories and Adapters).
- **E2E:** `integration_test` (for full application workflows).

