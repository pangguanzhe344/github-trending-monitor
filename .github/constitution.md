# Development Constitution

## Purpose

This document establishes the foundational principles, standards, and practices for developing and maintaining the GitHub Trending Monitor project. All contributors and AI coding agents should follow these guidelines to ensure consistency, quality, and maintainability.

---

## Core Principles

### 1. Simplicity First
- Prefer simple, readable solutions over clever code
- Avoid premature optimization
- Keep functions small and focused (< 50 lines)
- Use clear, descriptive naming

### 2. Reliability Over Features
- Ensure existing features work before adding new ones
- Never merge code that breaks existing tests
- Graceful error handling is mandatory
- Log errors with context for debugging

### 3. User-Centric Design
- Performance matters: optimize for user experience
- Accessibility is not optional
- Mobile-first responsive design
- Clear, helpful error messages

### 4. Open and Collaborative
- Welcome contributions from everyone
- Document decisions and rationale
- Code reviews are learning opportunities
- Be kind and constructive in feedback

---

## Architecture Standards

### Project Structure

```
github-trending-monitor/
├── .github/              # GitHub-specific files
│   ├── workflows/        # CI/CD workflows
│   ├── ISSUE_TEMPLATE/   # Issue templates
│   ├── spec.md          # Project specification
│   ├── constitution.md  # This document
│   └── plan.md          # Technical implementation plan
├── public/              # Static frontend assets
│   ├── index.html       # Main HTML page
│   ├── css/            # Stylesheets
│   └── js/             # Client-side JavaScript
├── services/           # Business logic layer
│   ├── githubService.js    # GitHub data fetching
│   ├── markdownGenerator.js # Report generation
│   └── scheduler.js        # Cron job management
├── tests/              # Test files (mirrors src structure)
├── reports/            # Generated markdown reports (git-ignored except .gitkeep)
├── server.js          # Express app entry point
└── package.json       # Project metadata
```

### Separation of Concerns

1. **Services Layer**: Pure business logic, no HTTP concerns
2. **Routes/Controllers**: Handle HTTP requests/responses
3. **Utilities**: Reusable helper functions
4. **Tests**: Mirror source structure, one test file per source file

### Module Design

- Each module should have a single, clear responsibility
- Export only what's necessary (prefer named exports)
- Keep dependencies minimal and explicit
- No circular dependencies

---

## Coding Standards

### JavaScript Style

#### General Rules
- Use ES6+ features (const/let, arrow functions, async/await)
- No var declarations
- Prefer const over let
- Use template literals for string interpolation
- Semicolons are required

#### Naming Conventions
```javascript
// Variables and functions: camelCase
const userName = 'John';
function fetchData() {}

// Constants: SCREAMING_SNAKE_CASE
const MAX_RETRIES = 3;
const API_BASE_URL = 'https://api.example.com';

// Classes: PascalCase
class ReportGenerator {}

// Private methods: prefix with _
_internalHelper() {}

// Boolean variables: descriptive predicates
const isLoading = true;
const hasError = false;
const canSubmit = true;
```

#### Function Guidelines
```javascript
// ✅ Good: Small, focused, well-named
async function fetchTrendingRepos() {
  const data = await fetch(TRENDING_URL);
  return parseRepoData(data);
}

// ❌ Bad: Too many responsibilities
async function doEverything() {
  // 100 lines of mixed concerns
}

// ✅ Good: Clear error handling
async function safeFetch(url) {
  try {
    return await fetch(url);
  } catch (error) {
    logger.error('Fetch failed', { url, error });
    throw new Error(`Failed to fetch ${url}: ${error.message}`);
  }
}
```

#### Async/Await
```javascript
// ✅ Good: Use async/await for asynchronous code
async function fetchData() {
  const data = await apiCall();
  return processData(data);
}

// ❌ Bad: Mixing promises and callbacks
function fetchData() {
  return apiCall().then(data => {
    processData(data, (err, result) => {
      // callback hell
    });
  });
}
```

### Error Handling

```javascript
// Always handle errors explicitly
try {
  const result = await riskyOperation();
  return result;
} catch (error) {
  // Log with context
  logger.error('Operation failed', {
    operation: 'riskyOperation',
    error: error.message,
    stack: error.stack
  });

  // Return meaningful error to user
  throw new Error('Unable to complete operation. Please try again.');
}

// Use custom error classes for domain errors
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}
```

### Comments and Documentation

```javascript
// ✅ Good: JSDoc for public APIs
/**
 * Fetches trending repositories from GitHub
 * @param {string} language - Programming language filter (optional)
 * @param {string} since - Time range: 'daily', 'weekly', 'monthly'
 * @returns {Promise<Array<Repo>>} Array of trending repositories
 * @throws {Error} If GitHub API is unavailable
 */
async function fetchTrendingRepos(language = '', since = 'daily') {
  // Implementation
}

// ✅ Good: Comments explain "why", not "what"
// Retry logic needed because GitHub API occasionally returns 500
const data = await retryFetch(url, { maxRetries: 3 });

// ❌ Bad: Comments explain obvious code
// Increment counter by 1
counter += 1;
```

---

## Testing Standards

### Test Structure

```javascript
// Use describe/it blocks for organization
describe('githubService', () => {
  describe('fetchTrendingRepos', () => {
    it('should return array of repos for valid request', async () => {
      const repos = await fetchTrendingRepos();
      expect(Array.isArray(repos)).toBe(true);
      expect(repos.length).toBeGreaterThan(0);
    });

    it('should handle API errors gracefully', async () => {
      // Mock API failure
      jest.spyOn(axios, 'get').mockRejectedValue(new Error('API Error'));

      await expect(fetchTrendingRepos()).rejects.toThrow();
    });
  });
});
```

### Coverage Requirements

- **Minimum Coverage**: 80% for all code
- **Critical Paths**: 100% coverage for core business logic
- **Error Paths**: All error handlers must be tested

### Testing Pyramid

1. **Unit Tests**: 70% - Fast, isolated, single-function tests
2. **Integration Tests**: 20% - Test module interactions
3. **E2E Tests**: 10% - Test complete user workflows

---

## Git Workflow

### Branch Strategy

```
main (production-ready code)
  ├── develop (integration branch)
  │   ├── feature/add-user-auth
  │   ├── feature/email-notifications
  │   ├── bugfix/fix-date-parsing
  │   └── claude/* (AI-generated branches)
  └── hotfix/critical-security-patch
```

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Test additions or modifications
- `chore`: Maintenance tasks

**Examples:**
```bash
feat(api): add endpoint for historical reports
fix(scheduler): correct timezone offset calculation
docs(readme): update installation instructions
test(github-service): add tests for error handling
```

### Pull Request Process

1. **Create Feature Branch**: From `develop`
2. **Write Code**: Follow coding standards
3. **Write Tests**: Maintain coverage requirements
4. **Run CI Checks**: All tests and linting must pass
5. **Create PR**: Use PR template, provide context
6. **Code Review**: At least 1 approval required
7. **Merge**: Squash and merge to `develop`

---

## Security Guidelines

### Environment Variables

```javascript
// ✅ Good: Use environment variables for config
const PORT = process.env.PORT || 3000;
const GITHUB_TOKEN = process.env.GITHUB_TOKEN;

// ❌ Bad: Hard-coded secrets
const API_KEY = 'sk_live_12345...';
```

### Input Validation

```javascript
// Always validate and sanitize user input
function getReport(fileName) {
  // Prevent directory traversal
  if (fileName.includes('..') || fileName.includes('/')) {
    throw new ValidationError('Invalid file name');
  }

  // Sanitize input
  const sanitized = fileName.replace(/[^a-zA-Z0-9-_.]/g, '');
  return readReport(sanitized);
}
```

### Dependencies

- Review dependencies before adding
- Use `npm audit` regularly
- Keep dependencies updated
- Prefer well-maintained packages with active communities

---

## Performance Guidelines

### Backend Performance

```javascript
// ✅ Good: Implement caching
const cache = new Map();
async function getCachedData(key, ttl = 3600000) {
  if (cache.has(key)) {
    const { data, timestamp } = cache.get(key);
    if (Date.now() - timestamp < ttl) {
      return data;
    }
  }
  const fresh = await fetchFreshData(key);
  cache.set(key, { data: fresh, timestamp: Date.now() });
  return fresh;
}

// ✅ Good: Avoid N+1 queries
const repos = await fetchAllRepos();
const enriched = await enrichReposInBatch(repos); // Single batch call

// ❌ Bad: N+1 query pattern
for (const repo of repos) {
  await enrichRepo(repo); // N separate calls
}
```

### Frontend Performance

- Minimize bundle size
- Lazy load images
- Use CDN for external libraries
- Implement loading states

---

## Documentation Requirements

### Code Documentation

- JSDoc for all public functions and classes
- README for each major module
- Inline comments for complex logic

### Project Documentation

- **README.md**: Project overview, setup, usage
- **CONTRIBUTING.md**: How to contribute
- **CHANGELOG.md**: Version history (automated)
- **API.md**: API endpoint documentation

---

## Continuous Integration

### Required Checks (must pass before merge)

1. ✅ All tests pass
2. ✅ Test coverage ≥ 80%
3. ✅ Linting passes (when configured)
4. ✅ No security vulnerabilities
5. ✅ Build succeeds

### Automated Tasks

- Run tests on every push
- Generate coverage reports
- Security scanning (Dependabot)
- Deploy to staging on merge to develop
- Deploy to production on release tag

---

## Deployment

### Environments

1. **Development**: Local machines
2. **Staging**: Cloud deployment from `develop` branch
3. **Production**: Cloud deployment from `main` branch

### Deployment Checklist

- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] CHANGELOG updated
- [ ] Environment variables configured
- [ ] Monitoring and logging configured

---

## AI Coding Agent Guidelines

When working with AI coding agents (like Claude Code):

1. **Always reference this constitution** before making changes
2. **Follow the project structure** defined above
3. **Write tests** for all new code
4. **Update documentation** when changing behavior
5. **Use descriptive commit messages** following conventional commits
6. **Ask for clarification** when requirements are ambiguous
7. **Prioritize existing patterns** in the codebase

---

## Review and Updates

This constitution is a living document:

- **Review Cycle**: Quarterly
- **Update Process**: PR to `.github/constitution.md`
- **Approval**: Requires maintainer consensus
- **Effective Date**: 2025-10-22

---

**Last Updated**: 2025-10-22
**Version**: 1.0.0
**Maintainers**: Development Team
