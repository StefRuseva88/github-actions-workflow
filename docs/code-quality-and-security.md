# Code Quality and Dependency Automation

This document describes the code analysis and dependency automation improvements implemented in this repository.

## Code Quality Checks

### ESLint Configuration

ESLint is configured to enforce code quality standards across the TypeScript/React codebase.

**Configuration:** `.eslintrc.json`

**Scripts:**
- `npm run lint` - Run ESLint to check code
- `npm run lint:fix` - Run ESLint and auto-fix issues

### Prettier Configuration

Prettier is configured to enforce consistent code formatting across all source files.

**Configuration:** `.prettierrc.json`

**Scripts:**
- `npm run format` - Format all source files
- `npm run format:check` - Check if files are properly formatted

**Prettier Rules:**
- Single quotes for strings
- 2-space indentation
- 80 character line width
- Semicolons required
- ES5 trailing commas
- Arrow function parentheses avoided when possible

### Code Quality Workflow

**Workflow:** `.github/workflows/code-quality.yml`

**Triggers:**
- Pull requests to `main`
- Push to `main` branch

**Checks:**
1. **ESLint** - Static code analysis
2. **Prettier** - Code formatting validation
3. **TypeScript** - Type checking

**Features:**
- Automatic PR comments with detailed issue reports
- Artifact uploads for quality reports (retained for 30 days)
- Fails the build if any checks fail

## Dependency Automation

### Dependabot Configuration

Dependabot is configured to automatically create pull requests for dependency updates.

**Configuration:** `.github/dependabot.yml`

**Update Schedule:**
- **NPM Dependencies:** Weekly on Mondays
  - Groups minor and patch updates for development dependencies
  - Groups patch updates for production dependencies
  - Maximum 10 open PRs
- **GitHub Actions:** Weekly on Mondays
  - Maximum 5 open PRs

**PR Labels:**
- `dependencies` - For npm dependency updates
- `github-actions` - For GitHub Actions updates
- `automated` - All Dependabot PRs

### Security and Dependency Review Workflow

**Workflow:** `.github/workflows/security.yml`

**Triggers:**
- Pull requests (dependency review)
- Push to `main` branch (npm audit)
- Scheduled weekly on Mondays at 9 AM UTC (npm audit)

**Jobs:**

#### 1. Dependency Review
- Runs on pull requests only
- Reviews new dependencies for known vulnerabilities
- Fails if moderate or higher severity issues found
- Comments summary in PR

#### 2. NPM Security Audit
- Runs npm audit to check for vulnerabilities
- Counts and categorizes vulnerabilities by severity
- Generates audit reports and fix previews
- Comments on PRs with:
  - Vulnerability counts by severity
  - Suggested automatic fixes
  - Commands to run for remediation
- Uploads audit reports as artifacts (retained for 30 days)

**Manual Remediation:**
```bash
# Fix vulnerabilities that don't require breaking changes
npm audit fix

# Force fix all vulnerabilities (may include breaking changes)
npm audit fix --force
```

## Integration with Existing Workflows

### Node.js CI Workflow

Updated `.github/workflows/node.js.yml` to include:
- Prettier format checking
- CYPRESS_INSTALL_BINARY=0 to prevent Cypress binary download issues

### Test Workflow

Updated `.github/workflows/test.yml` to include:
- CYPRESS_INSTALL_BINARY=0 to prevent Cypress binary download issues

### Packaging Workflow

Updated `.github/workflows/packaging-and-deployment.yml` to include:
- CYPRESS_INSTALL_BINARY=0 to prevent Cypress binary download issues

## Benefits

### Code Quality Benefits
- **Consistent Code Style:** Prettier enforces uniform formatting
- **Early Bug Detection:** ESLint catches potential issues before runtime
- **Type Safety:** TypeScript checking prevents type-related bugs
- **Automated Feedback:** PR comments provide immediate feedback to developers

### Security Benefits
- **Automated Updates:** Dependabot keeps dependencies up-to-date
- **Vulnerability Detection:** npm audit identifies known security issues
- **Proactive Monitoring:** Scheduled scans catch new vulnerabilities
- **Clear Remediation:** Automated comments suggest fixes
- **Audit Trail:** Artifact uploads maintain security history

## Usage Guide

### For Developers

**Before Committing:**
```bash
# Format your code
npm run format

# Check for lint issues
npm run lint

# Run type checking
npm run typecheck

# Run tests
npm test
```

**On Pull Requests:**
- Code quality workflow will automatically run
- Fix any reported issues before merging
- Review security audit comments if present

### For Maintainers

**Reviewing Dependabot PRs:**
1. Check the changelog for breaking changes
2. Review security advisories if present
3. Ensure CI passes
4. Merge if safe

**Handling Security Issues:**
1. Review npm-audit-report artifacts
2. Follow suggested remediation steps
3. Test fixes before merging
4. Update dependencies as needed

## Troubleshooting

### ESLint Errors
If ESLint reports errors, run `npm run lint:fix` to automatically fix many issues.

### Prettier Formatting
If Prettier check fails, run `npm run format` to automatically format all files.

### Dependency Vulnerabilities
Review the audit report and:
- Run `npm audit fix` for automatic fixes
- Manually update packages if needed
- Check for available patches or workarounds

### Cypress Installation Issues
All workflows are configured with `CYPRESS_INSTALL_BINARY=0` to prevent binary download issues in CI environments.
