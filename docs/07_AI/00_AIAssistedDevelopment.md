# AI Assisted Development Strategy

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the strategy for utilizing AI coding assistants (such as GitHub Copilot, Cursor, Gemini, or ChatGPT) during the development of CAS Analyzer.

## 2. AI Philosophy

CAS Analyzer embraces AI as an accelerator for development, not a replacement for engineering rigor. 

**Acceptable Uses of AI:**
- Generating boilerplate code (e.g., Riverpod providers, DTOs, mappers).
- Writing repetitive Unit and Widget tests.
- Drafting initial Markdown documentation and Mermaid diagrams.
- Explaining complex Flutter/Dart concepts or suggesting refactoring options.
- Writing regular expressions for the CAS parser based on anonymized text snippets.

## 3. Mandatory Review

**All AI-generated code must be treated as untrusted.** 
- It must be thoroughly reviewed by a human developer.
- It must be covered by automated tests.
- It must adhere strictly to the project's Clean Architecture principles and coding standards. AI tools often suggest shortcuts (like putting SQL directly in a UI widget) which must be explicitly rejected.

