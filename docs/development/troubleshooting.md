# Troubleshooting

## Common Issues

### flutter analyze fails

1. Check for analyzer warnings in changed files
2. Run `flutter analyze` to identify specific issues
3. Fix issues or add appropriate suppressions
4. Re-run `flutter analyze` to verify

### flutter test fails

1. Run `flutter test` to see failure details
2. Check for flaky test patterns
3. Verify test environment consistency
4. Fix test or update expectations
5. Re-run `flutter test` to verify

### flutter build apk --debug fails

1. Check Android SDK setup
2. Verify Gradle configuration
3. Review error output for specific issues
4. Fix identified issues
5. Re-run `flutter build apk --debug`

### Widget not rebuilding

1. Verify `setState()` is called
2. Check for `StatelessWidget` vs `StatefulWidget`
3. Ensure `InheritedWidget` proper usage
4. Review state management approach

## Debug Commands

- `flutter pub get` - Refresh dependencies
- `flutter clean` - Clean build cache
- `flutter pub outdated` - Check for newer versions
- `dart analyze` - Dart analysis standalone
- `flutter doctor` - Environment check