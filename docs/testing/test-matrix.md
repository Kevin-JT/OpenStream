# Test Matrix

## Platform Coverage

| Test Level | Android | iOS | Web | Desktop |
|------------|---------|-----|-----|---------|
| Unit Tests | ✓ | ✓ | ✓ | ✓ |
| Widget Tests | ✓ | ✓ | ✓ | ✓ |
| Integration Tests | ✓ | TBD | TBD | TBD |

## Test Type Coverage

| Test Type | Scope | Platforms |
|-----------|-------|-----------|
| Unit | Business logic, data models | All |
| Widget | UI components, navigation, forms | All |
| Integration | Full user flows | Android primarily |

## CI/CD Matrix

- `flutter analyze` on every PR
- `flutter test` on every PR
- `flutter build apk --debug` on merge
- Weekly: `flutter build ipa --debug` (iOS sim)
- Monthly: Manual cross-platform verification

## Test Prioritization

1. Critical paths (TBD)
2. Core UI: screens, forms, lists
3. Edge cases: error states, empty data, network failure
4. Polish: animations, transitions, accessibility