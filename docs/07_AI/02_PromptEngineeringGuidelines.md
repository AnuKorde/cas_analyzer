# Prompt Engineering Guidelines

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document provides best practices for prompting AI assistants to generate high-quality code specific to this project's architecture.

## 2. General Prompting Rules

1. **Provide Context:** Always reference or paste the relevant architecture documents (e.g., `project_context.md`) before asking for implementation.
2. **Specify Constraints:** Explicitly tell the AI what it cannot do. (e.g., "Do not use the `get_it` package, we use Riverpod. Do not put business logic in the widget.")
3. **Ask for Tests First:** When implementing complex logic (like a parser), ask the AI to generate the unit tests *before* generating the implementation.

## 3. Example: Prompting for a Parser

**Bad Prompt:**
> "Write a dart function to parse this CAS transaction: 01-Jan-24  Purchase  1000.00  10.0  100.0"

**Good Prompt:**
> "I am working on the CAS Analyzer project. Based on `04_ParsingAndCandidateGeneration.md`, we use pure Dart to generate `Candidate` models. 
> 
> Write a Dart class `TransactionLineParser` with a method that takes this synthetic string: '01-Jan-24  Purchase  1000.00  10.0  100.0' and returns a `TransactionCandidate`. 
> - Use named regex groups. 
> - Handle potential format exceptions by returning a `DiagnosticWarning` instead of throwing.
> - Ensure the candidate includes a `Provenance` object."

