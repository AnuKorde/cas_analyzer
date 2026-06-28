# CAS Analyzer

**Version:** 1.0.0 (Planning Phase)

**Project Status:** Design & Architecture

---

# Overview

CAS Analyzer is an offline-first Android application built using Flutter that enables investors to import and analyze NSDL/CDSL Consolidated Account Statement (CAS) PDF files.

The application extracts investment information from CAS statements, stores it in a local SQLite database, and provides portfolio analytics, investment insights, and actionable recommendations.

The primary goal of the project is to provide users with complete visibility into their investments without requiring any cloud storage or third-party services.

---

# Project Objectives

The application aims to:

* Import one or more CAS PDF files.
* Parse standard NSDL/CDSL Consolidated Account Statements.
* Store all extracted data locally.
* Analyze portfolios across multiple asset classes.
* Generate investment insights.
* Detect missing nominee information.
* Identify underperforming investments.
* Highlight portfolio concentration risks.
* Produce portfolio reports.
* Operate completely offline.

---

# Key Design Principles

The project follows the following principles:

* Offline First
* Privacy by Design
* Clean Architecture
* Feature-Based Development
* SOLID Principles
* Modular Design
* Test Driven Development (where practical)
* AI Assisted Development
* Incremental Delivery

---

# Technology Stack

| Area                 | Technology                                 |
| -------------------- | ------------------------------------------ |
| Framework            | Flutter                                    |
| Language             | Dart                                       |
| State Management     | Riverpod                                   |
| Database             | SQLite                                     |
| PDF Processing       | Syncfusion PDF                             |
| Charts               | fl_chart                                   |
| Navigation           | GoRouter                                   |
| Dependency Injection | Riverpod Providers                         |
| Version Control      | Git + GitHub                               |
| IDE                  | VS Code                                    |
| AI Assistance        | ChatGPT, Cursor, GitHub Copilot (optional) |

---

# High Level Architecture

```text
                   Flutter UI
                        │
                        ▼
             Presentation Layer
                        │
                        ▼
             Business Services
                        │
                        ▼
                Repository Layer
                        │
                        ▼
                    SQLite
                        ▲
                        │
                  PDF Parser
                        ▲
                        │
                     PDF File
```

---

# Repository Structure

```text
CAS-Analyzer/
│
├── app/
│
├── docs/
│
├── prompts/
│
├── diagrams/
│
├── scripts/
│
├── assets/
│
├── .github/
│
└── README.md
```

---

# Documentation Structure

```text
docs/

00_Project/

01_Architecture/

02_Database/

03_Parser/

04_UI/

05_BusinessLogic/

06_Testing/

07_AI/

08_Development/

ADR/
```

Every design decision should be documented before implementation begins.

---

# Development Strategy

The project will be developed in phases.

## Phase 1

Project Foundation

## Phase 2

Architecture

## Phase 3

Database

## Phase 4

PDF Parser

## Phase 5

Business Logic

## Phase 6

UI

## Phase 7

Testing

## Phase 8

AI Recommendations

## Phase 9

Release

Each phase will produce complete documentation before implementation starts.

---

# Development Workflow

For every feature:

1. Update architecture documentation (if needed).
2. Create or refine the implementation plan.
3. Implement the feature.
4. Write tests.
5. Perform code review.
6. Update documentation.
7. Commit to Git.

This keeps the documentation synchronized with the codebase.

---

# Coding Philosophy

The project emphasizes:

* Readable code over clever code.
* Small classes with a single responsibility.
* Composition over inheritance.
* Immutable models where appropriate.
* Repository pattern for data access.
* Separation of UI and business logic.

---

# Project Goals

The long-term vision is to build a portfolio management application that can:

* Import multiple CAS statements.
* Track historical investments.
* Provide portfolio analytics.
* Generate recommendations.
* Support secure local backups.
* Offer optional AI-assisted insights.

---

# Out of Scope (Version 1)

The first release will **not** include:

* Online synchronization.
* Real-time market prices.
* Brokerage integrations.
* Cloud storage.
* Multi-device synchronization.
* Direct trading functionality.

These capabilities may be considered in future releases.

---

# Documentation Standards

All project documentation should:

* Be written in Markdown.
* Use consistent headings.
* Include revision history.
* Cross-reference related documents.
* Explain design decisions and trade-offs.
* Avoid duplication where possible.

---

# AI Assisted Development

AI tools may be used to:

* Generate boilerplate code.
* Create unit tests.
* Draft documentation.
* Suggest refactoring.
* Generate diagrams.
* Explain implementation options.

All AI-generated code must be reviewed and tested before being merged.

---

# Contributing Guidelines

This project follows:

* Feature-based development.
* Small pull requests.
* Descriptive commit messages.
* Mandatory code reviews.
* Updated documentation with every significant change.

---

# Planned Documentation

The documentation repository will include:

* Project Vision
* Goals
* Scope
* Feature Roadmap
* Architecture
* Database Design
* Parser Design
* UI Specification
* Business Logic
* Testing Strategy
* AI Development Guide
* Implementation Plan
* Architecture Decision Records (ADR)

---

# Revision History

| Version | Date             | Description            |
| ------- | ---------------- | ---------------------- |
| 1.0.0   | Initial Planning | Initial README created |

---

# Next Document

The next document to be completed is:

**00_ProjectVision.md**

This document defines the business vision, target users, success criteria, and product direction for the CAS Analyzer application.
