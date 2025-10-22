# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

Please report security vulnerabilities by creating a GitHub Security Advisory or by emailing the maintainers.

**Please do NOT report security vulnerabilities through public GitHub issues.**

### What to Include

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

### Response Timeline

- Acknowledgment: Within 48 hours
- Initial assessment: Within 7 days
- Fix and disclosure: Within 30 days

## Security Best Practices

### For Users

- Keep dependencies updated with `npm audit`
- Use environment variables for sensitive data
- Never commit secrets to git
- Use HTTPS in production

### For Contributors

- Follow secure coding guidelines in `.github/constitution.md`
- Validate all user inputs
- Handle errors properly
- Review dependencies before adding them

## Known Security Considerations

- GitHub API rate limiting applies
- No user authentication in v1.0
- Reports contain only public GitHub data
- File system access is restricted to reports directory

---

Last Updated: 2025-10-22
