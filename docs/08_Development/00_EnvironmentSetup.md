# Environment Setup

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the required tools and environment configuration needed to start developing for CAS Analyzer.

## 2. Prerequisites

- **Flutter SDK:** Stable channel with Dart 3.12 or later.
- **Dart SDK:** Included with Flutter; the application requires Dart 3.12 or later.
- **IDE:** Visual Studio Code (recommended) or Android Studio.

## 3. Recommended VS Code Extensions

To ensure a consistent development experience, install the following extensions:
- **Flutter** & **Dart** (Official extensions)
- **Error Lens** (For inline analyzer warnings)
- **Riverpod Snippets** (Optional, for faster provider creation)
- **Markdown All in One** (For maintaining documentation)

## 4. Bootstrapping the Project

1. Clone the repository: `git clone <repository_url>`
2. Navigate to the app directory: `cd app`
3. Fetch dependencies: `flutter pub get`
4. Run the app: `flutter run`

Code generation is not part of the initial foundation. Do not add `build_runner`,
Riverpod code generation, or Freezed until an approved implementation decision
establishes a concrete need.

## 5. Build Configuration

Currently, the primary target is Android. 
- Ensure you have an Android Emulator configured or a physical device connected with USB Debugging enabled.
- Version 1 is released for Android only. Keep the shared Dart code and selected
  dependencies portable to iOS; do not remove generated platform support solely
  to enforce the release scope.

