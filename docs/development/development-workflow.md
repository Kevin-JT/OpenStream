# Development Workflow

## Daily Development

1. Start with latest main branch
2. Create feature branch: `git checkout -b feature/description`
3. Implement feature following PROJECT_RULES.md
4. Run `flutter analyze` - fix all issues
5. Run `flutter test` - ensure all pass
6. Run `flutter build apk --debug` - verify build
7. Commit with conventional commits
8. Push branch and create PR

## Conventional Commits

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types: feat, fix, docs, style, refactor, test, chore

## Branch Naming

- `feature/description` - New features
- `bug/description` - Bug fixes
- `hotfix/description` - Production fixes
- `docs/description` - Documentation changes

## PR Process

1. Fill PR template completely
2. Link related issues
3. All checklist items complete
4. Address review feedback
5. Maintainer approval required

## Code Review Checklist

- [ ] flutter analyze passes
- [ ] flutter test passes
- [ ] flutter build apk --debug passes
- [ ] PROJECT_RULES.md compliance
- [ ] Documentation updated if needed
- [ ] No secrets introduced
- [ ] Cross-platform compatibility verified