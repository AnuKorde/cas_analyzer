# Widget Testing Guidelines

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

This document describes how to test Flutter UI components (Widgets) in isolation to ensure they render correctly and respond to user inputs as expected.

## 2. Testing with Riverpod

Because CAS Analyzer uses Riverpod, widget tests must wrap the subject in a `ProviderScope`. This allows us to override the real business logic providers with mocked versions.

```dart
testWidgets('Dashboard shows loading state', (WidgetTester tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        portfolioProvider.overrideWith((ref) => AsyncValue.loading()),
      ],
      child: const MaterialApp(home: DashboardScreen()),
    ),
  );

  expect(find.byType(CircularProgressIndicator), findsOneWidget);
});
```

## 3. What to Test

- **State Handling:** Verify that the widget correctly renders data, loading indicators, and error boundaries based on the `AsyncValue` state.
- **Interactions:** Verify that tapping buttons or entering text triggers the expected methods on the mocked UI State Controllers.
- **Graceful Degradation:** Ensure that a failure in a child widget (e.g., a chart throwing an error) does not crash the parent screen.

## 4. What NOT to Test

- Do not use widget tests to verify financial math; that belongs in Unit Tests.
- Do not test actual GoRouter navigation paths deeply; test that the routing intent was fired.

