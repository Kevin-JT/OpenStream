# Git Workflow

## Repository Structure

- main: Production-ready code only
- feature branches: Development work
- PRs: Required for all changes

## Workflow Steps

1. `git checkout main` && `git pull`
2. `git checkout -b feature/description`
3. Development work in feature branch
4. `git push -u origin feature/description`
5. Create PR against main
6. Address review comments
7. Maintainer merges PR to main
8. `git checkout main` && `git pull`

## Commit Guidelines

- Conventional commits required
- Descriptive subject lines (50 chars max)
- Body: motivation and context
- Footer: breaking changes, issues

## Merge Requirements

- All gates pass (analyze, test, build)
- At least one reviewer approval
- No merge conflicts
- CHANGELOG.md updated for user-visible changes

## Conflict Resolution

- Rebase feature branch against main
- Resolve conflicts in feature branch
- Force push only after team consensus
- Document conflict resolution in PR