# Section Discovery

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document details stage **IP-06 Discover Sections** of the import pipeline. CAS Statements are logically divided into sections (e.g., Summary, Folios, Transactions). This stage segments the continuous text stream into distinct, manageable blocks before detailed parsing occurs.

## 2. Section Types

Typical sections found in a CAS include:
1. **Header/Metadata:** Statement period, investor name, address, PAN.
2. **Summary of Holdings:** Aggregate values and allocations.
3. **Detailed Folios/Accounts:** Specific mutual fund or equity details.
4. **Transaction History:** Chronological ledger of investments, redemptions, and corporate actions.
5. **Footer/Disclaimers:** Legal text and explanatory notes (often ignored).

## 3. Discovery Strategy

The `SectionDiscoverer` uses known section delimiters and headers specific to the detected format (e.g., NSDL or CDSL) to mark the start and end of blocks.

- **State Machine:** A simple state machine tracks the current section based on matched headers.
- **Line Buffering:** Lines are grouped into a `SectionBlock` object containing the section type and the raw text lines.
- **Graceful Skipping:** Unknown or irrelevant sections (like detailed disclaimers) are marked as `ignored` to save processing time.

```dart
class SectionBlock {
  final SectionType type;
  final List<String> textLines;
  final int startPage;
  // ...
}
```

## 4. Error Handling

If a mandatory section (e.g., Investor Details) is completely missing, the discoverer may yield a warning or failure diagnostic, as downstream parsing might not be able to establish account ownership.

