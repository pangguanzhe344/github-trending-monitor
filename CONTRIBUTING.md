# Contributing to GitHub Trending Monitor

First off, thank you for considering contributing to GitHub Trending Monitor! It's people like you that make this project better for everyone.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Community](#community)

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to the project maintainers.

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues as you might find out that you don't need to create one. When you are creating a bug report, please include as many details as possible using our [bug report template](.github/ISSUE_TEMPLATE/bug_report.md).

**Good Bug Reports Include:**
- A clear and descriptive title
- Exact steps to reproduce the problem
- Expected vs. actual behavior
- Screenshots if applicable
- Your environment details (OS, Node.js version, etc.)
- Relevant logs or error messages

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, please use our [feature request template](.github/ISSUE_TEMPLATE/feature_request.md) and include:

- A clear and descriptive title
- A detailed description of the proposed feature
- Why this enhancement would be useful
- Examples of how it would work
- Mockups or wireframes if applicable

### Your First Code Contribution

Unsure where to begin? You can start by looking through these issues:

- `good-first-issue` - Issues that should only require a few lines of code
- `help-wanted` - Issues that are a bit more involved

### Pull Requests

We actively welcome your pull requests! Here's how to contribute code:

1. Fork the repository
2. Create your feature branch
3. Make your changes
4. Write or update tests
5. Update documentation
6. Submit a pull request

See [Pull Request Process](#pull-request-process) for detailed steps.

## Development Setup

### Prerequisites

- Node.js >= 18.x
- npm >= 8.x
- Git

### Installation

```bash
# 1. Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/github-trending-monitor.git
cd github-trending-monitor

# 2. Install dependencies
npm install

# 3. Create environment file (optional)
cp .env.example .env

# 4. Run tests to verify setup
npm test

# 5. Start development server
npm start
```

### Project Structure

```
github-trending-monitor/
├── .github/              # GitHub-specific files (workflows, templates)
├── public/              # Frontend static files
│   ├── index.html
│   ├── css/
│   └── js/
├── services/           # Business logic
│   ├── githubService.js
│   ├── markdownGenerator.js
│   └── scheduler.js
├── tests/              # Test files
├── reports/            # Generated reports (git-ignored)
├── server.js          # Express server entry point
└── package.json
```

## Development Workflow

### Branch Naming Convention

```
feature/description      # New features
bugfix/description       # Bug fixes
hotfix/description       # Critical fixes for production
docs/description         # Documentation changes
refactor/description     # Code refactoring
test/description         # Test additions/updates
```

**Examples:**
```
feature/add-email-notifications
bugfix/fix-timezone-issue
docs/update-api-documentation
```

### Development Process

1. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Follow our [coding standards](#coding-standards)
   - Write clean, readable code
   - Add comments for complex logic

3. **Write Tests**
   - Add unit tests for new functions
   - Update existing tests if needed
   - Maintain >80% coverage

4. **Run Tests Locally**
   ```bash
   npm test                # Run all tests
   npm run test:watch      # Watch mode for development
   ```

5. **Commit Changes**
   ```bash
   git add .
   git commit -m "feat: add email notification feature"
   ```

6. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create Pull Request**
   - Use our PR template
   - Reference related issues
   - Provide clear description

## Coding Standards

We follow the standards defined in our [Development Constitution](.github/constitution.md). Here are the highlights:

### JavaScript Style

```javascript
// ✅ Good: Use const/let, not var
const userName = 'John';
let counter = 0;

// ✅ Good: Arrow functions for callbacks
repos.map(repo => repo.name);

// ✅ Good: Async/await for asynchronous code
async function fetchData() {
  const data = await apiCall();
  return processData(data);
}

// ✅ Good: Descriptive names
function calculateTotalStars(repositories) { }

// ❌ Bad: Short, unclear names
function calc(r) { }
```

### Naming Conventions

- **Variables & Functions**: `camelCase`
- **Constants**: `SCREAMING_SNAKE_CASE`
- **Classes**: `PascalCase`
- **Files**: `camelCase.js`
- **Test Files**: `fileName.test.js`

### Code Organization

- Keep functions small (< 50 lines)
- One responsibility per function
- Extract complex logic into separate functions
- Use meaningful variable names

### Error Handling

```javascript
// ✅ Always handle errors
try {
  const result = await riskyOperation();
  return result;
} catch (error) {
  logger.error('Operation failed', { error });
  throw new Error('User-friendly error message');
}
```

### Comments and Documentation

```javascript
/**
 * Fetches trending repositories from GitHub
 * @param {string} language - Programming language filter
 * @param {string} since - Time range: 'daily', 'weekly', 'monthly'
 * @returns {Promise<Array<Repo>>} Array of trending repositories
 */
async function fetchTrendingRepos(language = '', since = 'daily') {
  // Implementation
}
```

## Commit Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Commit Message Format

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks (dependencies, build, etc.)
- `perf`: Performance improvements

### Examples

```bash
feat(api): add endpoint for user preferences
fix(scheduler): correct UTC offset calculation
docs(readme): add installation instructions
test(github-service): add error handling tests
chore(deps): update axios to v1.6.0
```

### Best Practices

- Use the imperative mood ("add" not "added" or "adds")
- Don't capitalize the first letter
- No period at the end
- Keep the summary line under 72 characters
- Provide detailed description in the body if needed

## Pull Request Process

### Before Submitting

1. **Update your branch**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout your-branch
   git rebase develop
   ```

2. **Run all checks**
   ```bash
   npm test           # All tests must pass
   npm run lint       # If linting is configured
   npm audit          # Check for vulnerabilities
   ```

3. **Update documentation**
   - Update README.md if needed
   - Add JSDoc comments
   - Update relevant .md files

### Submitting Pull Request

1. **Fill out the PR template completely**
2. **Link related issues** using keywords:
   - `Fixes #123`
   - `Closes #456`
   - `Relates to #789`

3. **Request review** from maintainers

4. **Respond to feedback**
   - Address all review comments
   - Push new commits or amend existing ones
   - Re-request review when ready

### PR Checklist

- [ ] Code follows project style guidelines
- [ ] Self-reviewed code
- [ ] Commented complex code sections
- [ ] Updated documentation
- [ ] Added/updated tests
- [ ] All tests pass
- [ ] No new warnings
- [ ] Dependent changes merged

### After Approval

- Maintainers will merge using "Squash and merge"
- Your branch will be automatically deleted
- Your contribution will be included in the next release

## Testing Guidelines

### Running Tests

```bash
npm test                    # Run all tests once
npm run test:watch          # Run tests in watch mode
npm test -- --coverage      # Run with coverage report
```

### Writing Tests

```javascript
// Structure: describe > it/test
describe('githubService', () => {
  describe('fetchTrendingRepos', () => {
    it('should return array of repos', async () => {
      const repos = await fetchTrendingRepos();
      expect(Array.isArray(repos)).toBe(true);
      expect(repos.length).toBeGreaterThan(0);
    });

    it('should handle API errors', async () => {
      // Mock failure
      jest.spyOn(axios, 'get').mockRejectedValue(new Error('API Error'));

      await expect(fetchTrendingRepos()).rejects.toThrow();
    });
  });
});
```

### Test Coverage

- Maintain >80% overall coverage
- 100% coverage for critical business logic
- Test both success and error paths
- Test edge cases

## Documentation

### Code Documentation

- Add JSDoc comments for all public functions
- Document complex algorithms inline
- Keep comments up-to-date with code changes

### Project Documentation

When updating functionality, also update:

- README.md - User-facing features and setup
- .github/spec.md - Specification changes
- .github/plan.md - Implementation details
- API documentation (if API changes)

### Writing Good Documentation

- Use clear, simple language
- Provide examples
- Keep it concise
- Use proper formatting (headers, lists, code blocks)
- Include screenshots for UI changes

## Community

### Getting Help

- **GitHub Discussions**: Ask questions and share ideas
- **Issues**: Report bugs and request features
- **Email**: Contact maintainers for sensitive matters

### Recognition

Contributors are recognized in:
- README.md Contributors section
- Release notes
- Project documentation

### Becoming a Maintainer

Active contributors may be invited to become maintainers. Maintainers:
- Review and merge pull requests
- Triage issues
- Guide project direction
- Mentor new contributors

## Additional Resources

- [Development Constitution](.github/constitution.md) - Detailed coding standards
- [Project Specification](.github/spec.md) - Project goals and features
- [Technical Plan](.github/plan.md) - Implementation details
- [GitHub Spec-Kit](https://github.com/github/spec-kit) - Spec-driven development

## Questions?

Don't hesitate to ask questions! We're here to help:

- Open an issue with the `question` label
- Start a discussion in GitHub Discussions
- Contact maintainers directly

---

Thank you for contributing to GitHub Trending Monitor!

We appreciate your time and effort in making this project better.
