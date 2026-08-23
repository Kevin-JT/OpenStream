# Regression Strategy

## Regression Prevention

All changes must pass the following gates before merge:

1. `flutter analyze` - No new warnings or errors
2. `flutter test` - 100% of existing tests pass
3. `flutter build apk --debug` - Build succeeds

## Regression Process

1. **Pre-commit**: Developer runs all three gates locally
2. **CI Pipeline**: All gates run on CI server
3. **Blocker**: Any failing gate blocks PR merge
4. **Fix Cycle**: Developer fixes and re-runs gates
5. **Verification**: Maintainer verifies fix before merge

## Known Regression Areas (Future)

- State management changes - verify widget tree unchanged
- Architecture selections - ensure cross-platform compatibility
- Dependency upgrades - run full test suite after update
- UI changes - review widget tests for relevance

## Rollback Criteria

If any gate fails after merge:
- Immediate investigation required
- Revert if critical issue confirmed
- Post-mortem documentation in CHANGELOG.md
- Process improvement if pattern emerges