# Quick Start Guide: Code Quality and Security

## For Developers

### Before Committing Code

Run these commands locally to ensure your code meets quality standards:

```bash
# 1. Format your code
npm run format

# 2. Check for linting issues
npm run lint

# 3. Check TypeScript types
npm run typecheck

# 4. Run tests
npm test

# 5. Build the project
npm run build
```

### Fixing Issues

```bash
# Auto-fix ESLint issues
npm run lint:fix

# Auto-format all files
npm run format
```

### Understanding PR Feedback

When you create a pull request, the following workflows will automatically run:

1. **Code Quality Workflow** - Checks ESLint, Prettier, and TypeScript
   - If it fails, check the workflow logs or PR comments
   - Run the failing commands locally to see detailed errors
   - Fix issues and push again

2. **Security Workflow** - Checks for dependency vulnerabilities
   - If vulnerabilities are found, a comment will appear on your PR
   - Follow the suggested `npm audit fix` commands
   - Review breaking changes before running `npm audit fix --force`

## For Maintainers

### Reviewing Dependabot PRs

Dependabot will automatically create PRs for dependency updates every Monday.

**Steps to Review:**
1. Check the PR description for changelog information
2. Review any security advisories mentioned
3. Ensure all CI checks pass
4. Merge if safe, or close if the update is not needed

### Handling Security Alerts

The security workflow runs:
- On all PRs
- On push to main
- Weekly on Mondays at 9 AM UTC

**When vulnerabilities are detected:**
1. Download the `npm-audit-report` artifact from the workflow run
2. Review the JSON report for details
3. Check the fix preview for suggested changes
4. Test fixes locally before merging

### Manual Security Commands

```bash
# View all vulnerabilities
npm audit

# Fix non-breaking vulnerabilities
npm audit fix

# Fix all vulnerabilities (may include breaking changes)
npm audit fix --force

# View what would be fixed without applying changes
npm audit fix --dry-run
```

## Workflow Schedule

| Workflow | Trigger | Frequency |
|----------|---------|-----------|
| Code Quality | PR/Push to main | Every PR and push |
| Security - Dependency Review | PR only | Every PR |
| Security - NPM Audit | PR/Push/Schedule | Every PR, push, and weekly |
| Dependabot - NPM | Scheduled | Weekly (Mondays) |
| Dependabot - Actions | Scheduled | Weekly (Mondays) |

## Troubleshooting

### "Prettier check failed"
Run `npm run format` to automatically fix formatting.

### "ESLint found errors"
Run `npm run lint:fix` to automatically fix many issues. Some may require manual fixes.

### "TypeScript errors found"
Review the errors in the workflow logs or run `npm run typecheck` locally.

### "Cypress installation failed"
This is handled automatically in workflows with `CYPRESS_INSTALL_BINARY=0`.

### "npm audit found vulnerabilities"
Review the PR comment with suggested fixes. Run `npm audit fix` locally and test before committing.

## Configuration Files

| File | Purpose |
|------|---------|
| `.eslintrc.json` | ESLint configuration |
| `.prettierrc.json` | Prettier formatting rules |
| `.prettierignore` | Files excluded from Prettier |
| `.github/dependabot.yml` | Dependabot automation config |
| `.github/workflows/code-quality.yml` | Code quality checks workflow |
| `.github/workflows/security.yml` | Security and dependency workflow |

## Getting Help

For detailed information, see:
- [Complete Documentation](./code-quality-and-security.md)
- [ESLint Documentation](https://eslint.org/)
- [Prettier Documentation](https://prettier.io/)
- [Dependabot Documentation](https://docs.github.com/en/code-security/dependabot)

## Quick Reference

```bash
# Code Quality Commands
npm run lint           # Check code with ESLint
npm run lint:fix       # Fix ESLint issues
npm run format         # Format all files
npm run format:check   # Check formatting
npm run typecheck      # Check TypeScript types

# Testing
npm test               # Run tests with coverage
npm run build          # Build the project

# Security
npm audit              # Check for vulnerabilities
npm audit fix          # Fix vulnerabilities
```
