# Project Rules

## Coding Rules
- Follow Flutter/Dart conventions
- Use meaningful names and clear code structure
- No hardcoded secrets or credentials
- Keep widgets focused and single-purpose

## Architecture Rules
- Cross-platform ready architecture
- No platform-specific implementation unless required
- TBD items must remain as TBD until approved

## Dependency Rules
- No new dependencies without evaluation
- Only use packages from pub.dev
- Keep dependency count minimal for Phase 01

## Git Rules
- Conventional commits
- Feature branches off main
- PRs require review
- Keep commit history clean

## AI Policy Compliance
- Follow AI_POLICY.md strictly
- Do not auto-generate implementation without approval
- Verify all code changes manually

## Stability Rules
- flutter analyze must pass
- flutter test must pass
- flutter build apk --debug must pass before merge