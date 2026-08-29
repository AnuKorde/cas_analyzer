# Text Extraction Strategy

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document details stage **IP-04 Extract** of the import pipeline. It explains how raw text is safely pulled from CAS PDF files using the Syncfusion PDF package, bounded in memory, and formatted for downstream parsing.

## 2. Scope

- PDF package adapter design.
- Paged extraction to prevent memory exhaustion.
- Cross-isolate communication for background extraction.
- Password handling strategy (if supported in V1).

## 3. Extraction Adapter

To prevent the core parser domain from tightly coupling to Syncfusion Flutter PDF, an adapter interface is defined:

```dart
abstract class PdfExtractor {
  Stream<String> extractTextLines(String filePath, {String? password});
  Future<bool> isPasswordProtected(String filePath);
}
```

This ensures we can mock the extractor during tests or swap PDF libraries in the future without changing parser logic.

## 4. Resource Bounding

Large CAS statements can span hundreds of pages. The extraction strategy must:
1. Extract text page-by-page or chunk-by-chunk.
2. Yield chunks using Dart `Stream` or `Isolate` messages to keep memory usage flat.
3. Allow cancellation mid-extraction if the user aborts the import.

