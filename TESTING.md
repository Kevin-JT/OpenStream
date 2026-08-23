# Testing

## Philosophy
- Test-driven development where possible
- Comprehensive coverage of critical paths
- Maintain cross-platform compatibility

## Required Validation
- flutter analyze must pass
- flutter test must pass
- flutter build apk --debug must pass

## Quality Gates
- Minimum test coverage for critical paths
- No analyzer warnings on new code
- All existing tests must continue to pass

## Test Matrix
- Unit tests: Widget and logic units
- Widget tests: UI component behavior
- Integration tests: Cross-platform flows (TBD)

## Regression Strategy
- All PRs must run full test suite
- Failing tests block merge
- Maintain test files alongside source code