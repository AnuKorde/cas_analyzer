# Environment Setup

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document outlines the required tools and environment configuration needed to start developing for CAS Analyzer.

## 2. Prerequisites

- **Flutter SDK:** Version 3.x (Ensure you are on the `stable` channel).
- **Dart SDK:** Included with Flutter (Version 3.0 or higher).
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
4. Run the code generator (for Riverpod/Freezed, if utilized): `dart run build_runner build --delete-conflicting-outputs`
5. Run the app: `flutter run`

## 5. Build Configuration

Currently, the primary target is Android. 
- Ensure you have an Android Emulator configured or a physical device connected with USB Debugging enabled.
- Web and iOS targets are disabled or unsupported for Version 1.

