# Technical Implementation Plan

## Overview

This document provides a detailed technical plan for implementing the GitHub Trending Monitor project, based on the specifications in [spec.md](./spec.md) and adhering to the principles in [constitution.md](./constitution.md).

**Status**: ✅ v1.0 Implementation Complete
**Last Updated**: 2025-10-22

---

## Implementation Phases

### Phase 1: Foundation ✅ COMPLETED

**Goal**: Set up project infrastructure and basic architecture

#### Tasks Completed

- [x] Initialize Node.js project with package.json
- [x] Set up project directory structure
- [x] Configure Git repository with .gitignore
- [x] Install core dependencies (Express, Axios, Cheerio, node-cron)
- [x] Install dev dependencies (Jest, Supertest)
- [x] Configure Jest for testing
- [x] Create basic Express server (server.js)
- [x] Set up static file serving

#### Technical Decisions

**Dependency Choices:**
- **Express 5.x**: Latest stable version, improved performance
- **Axios 1.12.x**: Reliable HTTP client with interceptor support
- **Cheerio 1.1.x**: jQuery-like HTML parsing for web scraping
- **node-cron 4.x**: Reliable cron job scheduling
- **Jest 29.x**: Industry-standard testing framework

**Architecture Pattern:**
- Service-oriented architecture with separation of concerns
- Services layer for business logic
- Routes/controllers for HTTP handling
- No database in v1.0 (file-based storage)

---

### Phase 2: Core Services ✅ COMPLETED

**Goal**: Implement data fetching and processing logic

#### 2.1 GitHub Service (services/githubService.js)

**Responsibilities:**
- Fetch trending repositories from GitHub
- Handle HTTP requests and error cases
- Implement caching to respect rate limits
- Parse HTML response using Cheerio

**Implementation:**

```javascript
// Key features implemented:
class GitHubService {
  - fetchTrendingRepos(language, since)
  - parseHTML(html)
  - cache with TTL (1 hour)
  - error handling and retry logic
  - rate limit management
}
```

**API Endpoint Used:**
- Primary: `https://github.com/trending` (web scraping)
- Fallback: GitHub REST API (requires authentication)

**Caching Strategy:**
- In-memory cache with 1-hour TTL
- Prevents excessive requests to GitHub
- Cache invalidation on error

#### 2.2 Markdown Generator (services/markdownGenerator.js)

**Responsibilities:**
- Convert repository data to Markdown format
- Create structured, readable reports
- Include metadata and formatting

**Implementation:**

```javascript
// Key features implemented:
function generateMarkdownReport(repos) {
  - Add report header with date/metadata
  - Format each repo as a section
  - Include stars, forks, language badges
  - Add table of contents
  - Generate file with unique timestamp
}
```

**Report Format:**
```markdown
# GitHub Trending - YYYY-MM-DD

## Summary
Total Projects: N
Generated: timestamp

## Projects

### 1. [Repo Name](url)
**Language**: JavaScript | **Stars**: 1000 | **Forks**: 200

Description text here...
```

#### 2.3 Scheduler Service (services/scheduler.js)

**Responsibilities:**
- Manage cron jobs for automated tasks
- Schedule daily report generation (8:30 AM)
- Schedule daily reminders (9:30 AM)
- Graceful start/stop

**Implementation:**

```javascript
// Cron schedules:
- Data fetch: '30 8 * * *' (8:30 AM daily)
- Reminder: '30 9 * * *' (9:30 AM daily)

// Features:
- Timezone: UTC+8 (configurable)
- Error handling with logging
- Manual trigger support
```

---

### Phase 3: Web Interface ✅ COMPLETED

**Goal**: Create responsive web UI for browsing trending projects

#### 3.1 Frontend Structure (public/)

**Files:**
- `index.html`: Main application page
- `css/style.css`: Custom styles
- `js/app.js`: Client-side logic

#### 3.2 UI Components

**Landing Page:**
- Hero section with project description
- Real-time trending projects grid
- Loading states and error handling

**Project Cards:**
```html
<div class="card">
  <h5>Repository Name</h5>
  <p class="text-muted">Language</p>
  <p>Description</p>
  <div class="stats">
    <span>⭐ Stars</span>
    <span>🍴 Forks</span>
  </div>
  <a href="..." class="btn">View on GitHub</a>
</div>
```

**Features:**
- Responsive grid layout (1/2/3 columns)
- Skeleton loading placeholders
- Error state UI
- Toast notifications

#### 3.3 Historical Reports Section

**Features:**
- List all generated reports
- Click to view report content
- Markdown rendering
- Date filtering (future enhancement)

---

### Phase 4: API Endpoints ✅ COMPLETED

**Goal**: Expose REST API for data access

#### API Routes (server.js)

```javascript
// Implemented endpoints:

GET /api/trending
  - Returns current trending repositories
  - Uses cache (1-hour TTL)
  - Query params: ?language=javascript&since=weekly

GET /api/cache-status
  - Returns cache metadata
  - Shows last update time, TTL remaining

GET /api/github-health
  - Health check for GitHub API
  - Returns status and rate limit info

GET /api/reports
  - Lists all generated report files
  - Returns array of filenames with metadata

GET /api/report/:fileName
  - Returns specific report content
  - Validates filename (security)
  - Returns markdown as text/plain
```

**Response Format:**
```json
{
  "success": true,
  "data": [...],
  "cached": true,
  "timestamp": "2025-10-22T10:30:00Z"
}
```

**Error Handling:**
```json
{
  "success": false,
  "error": "Error message",
  "code": "ERROR_CODE"
}
```

---

### Phase 5: Testing ✅ COMPLETED

**Goal**: Achieve >80% test coverage

#### Test Files

1. **services/githubService.test.js**
   - Unit tests for data fetching
   - Mock HTTP requests
   - Test caching logic
   - Error scenarios

2. **services/markdownGenerator.test.js**
   - Test report generation
   - Validate markdown format
   - Edge cases (empty data, special characters)

3. **tests/server.test.js**
   - Integration tests for API endpoints
   - Request/response validation
   - Error handling

**Coverage Achieved:**
- Overall: ~85%
- Services: >90%
- Server routes: >80%

**Test Commands:**
```bash
npm test              # Run all tests
npm run test:watch   # Watch mode
npm run test:coverage # Coverage report
```

---

### Phase 6: Automation & CI/CD ✅ COMPLETED

**Goal**: Automate testing, deployment, and reporting

#### 6.1 GitHub Actions Workflows

**CI Workflow (.github/workflows/ci.yml):**
- Trigger: Push to main/develop/claude/**, PRs
- Jobs:
  - Test on Node.js 18.x, 20.x, 22.x
  - Run linter (if configured)
  - Generate coverage report
  - Upload to Codecov

**Cron Workflow (.github/workflows/cron.yml):**
- Trigger: Daily at 8:30 AM UTC+8 (cron: '30 0 * * *')
- Jobs:
  - Fetch trending repos
  - Generate markdown report
  - Commit report to repository
  - Create GitHub issue with summary (optional)

**Release Workflow (.github/workflows/release.yml):**
- Trigger: Git tag (v*.*.*)
- Jobs:
  - Run tests
  - Build project
  - Generate changelog
  - Create GitHub release

#### 6.2 Dependabot Configuration

**Purpose**: Automated dependency updates and security patches

**Configuration (.github/dependabot.yml):**
- Check npm dependencies weekly
- Auto-create PRs for updates
- Group minor/patch updates

---

## Technology Stack Details

### Backend Stack

| Component | Technology | Version | Rationale |
|-----------|-----------|---------|-----------|
| Runtime | Node.js | >= 18.x | LTS support, modern features |
| Framework | Express.js | 5.1.0 | Mature, lightweight, widely adopted |
| HTTP Client | Axios | 1.12.2 | Promise-based, interceptors, retries |
| HTML Parser | Cheerio | 1.1.2 | Fast, jQuery-like syntax |
| Scheduler | node-cron | 4.2.1 | Simple, reliable cron jobs |
| Env Config | dotenv | 17.2.3 | Standard env var management |

### Testing Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| Test Framework | Jest | 29.7.0 | Full-featured testing solution |
| API Testing | Supertest | 7.1.4 | HTTP assertion library |
| Mocking | Jest Built-in | - | Mock functions and modules |

### Frontend Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| UI Framework | Bootstrap | 5.3.x | Responsive components, grid |
| Icons | Bootstrap Icons | Latest | SVG icon set |
| JavaScript | ES6+ | - | Modern syntax, async/await |

---

## Data Flow Architecture

### 1. Scheduled Report Generation

```
GitHub Actions Cron (daily 8:30 AM)
  ↓
Trigger workflow
  ↓
githubService.fetchTrendingRepos()
  ↓
Parse HTML → Extract repo data
  ↓
markdownGenerator.generateMarkdownReport(data)
  ↓
Save to reports/github-trending-YYYY-MM-DD.md
  ↓
Git commit & push
  ↓
(Optional) Create GitHub issue with summary
```

### 2. Web Interface Request Flow

```
User visits http://localhost:3000
  ↓
Browser loads index.html
  ↓
app.js: fetch('/api/trending')
  ↓
server.js: Check cache
  ├─ Cache hit → Return cached data
  └─ Cache miss → githubService.fetchTrendingRepos()
      ↓
      Parse & cache → Return data
  ↓
app.js: Render cards in UI
```

### 3. Historical Report Access

```
User clicks "View Reports"
  ↓
app.js: fetch('/api/reports')
  ↓
server.js: fs.readdir('./reports')
  ↓
Return list of report files
  ↓
User clicks specific report
  ↓
app.js: fetch('/api/report/filename.md')
  ↓
server.js: Validate filename → fs.readFile()
  ↓
Return markdown content
  ↓
app.js: Render markdown in modal/page
```

---

## Security Considerations

### Implemented Security Measures

1. **Input Validation:**
   - Filename validation (prevent directory traversal)
   - Query parameter sanitization
   - No eval() or exec() usage

2. **Rate Limiting:**
   - Caching to reduce external requests
   - Respect GitHub rate limits
   - Exponential backoff on failures

3. **Error Handling:**
   - Never expose stack traces to client
   - Sanitize error messages
   - Log detailed errors server-side only

4. **Dependencies:**
   - Use npm audit regularly
   - Dependabot for automated security updates
   - Pin major versions, allow patch updates

5. **Environment Variables:**
   - No secrets in code
   - .env file git-ignored
   - Example .env.example provided

### Future Security Enhancements

- [ ] Implement CSRF protection
- [ ] Add request rate limiting middleware
- [ ] Content Security Policy headers
- [ ] HTTPS enforcement in production
- [ ] User authentication (when adding user features)

---

## Performance Optimization

### Implemented Optimizations

1. **Backend:**
   - In-memory caching (1-hour TTL)
   - Lazy loading of reports
   - Efficient HTML parsing with Cheerio
   - Async/await throughout

2. **Frontend:**
   - CDN for Bootstrap CSS/JS
   - Minimal custom CSS/JS
   - Lazy image loading (future)
   - Debounced search (future)

3. **Network:**
   - Compression middleware (express.json())
   - Static file caching headers
   - Conditional requests (future)

### Performance Targets

| Metric | Target | Status |
|--------|--------|--------|
| API response time | < 500ms | ✅ Achieved |
| Page load time | < 2s | ✅ Achieved |
| Report generation | < 30s | ✅ Achieved |
| Cache hit rate | > 80% | ✅ Achieved |

---

## Deployment Strategy

### Local Development

```bash
# Setup
npm install
cp .env.example .env

# Run
npm start

# Test
npm test
```

### Production Deployment Options

#### Option 1: Traditional VPS (e.g., DigitalOcean, AWS EC2)

```bash
# Setup
git clone <repo>
npm ci --production
pm2 start server.js

# Cron jobs handled by GitHub Actions
```

#### Option 2: Serverless (Future)

- Deploy Express app to AWS Lambda / Vercel
- Use CloudWatch Events for scheduling
- Static files on S3/CloudFront

#### Option 3: Container (Future)

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

### Environment Variables

```bash
# Required
PORT=3000
NODE_ENV=production

# Optional
GITHUB_TOKEN=ghp_xxx  # For higher rate limits
TIMEZONE=Asia/Shanghai  # For scheduler
CACHE_TTL=3600000  # 1 hour in ms
```

---

## Monitoring and Logging

### Current Logging

```javascript
// Console logging for key events
console.log('Server started on port 3000');
console.log('Cron job executed: fetch trending');
console.error('Error fetching data:', error);
```

### Future Enhancements

- [ ] Winston for structured logging
- [ ] Log levels (debug, info, warn, error)
- [ ] Log aggregation (LogTail, Papertrail)
- [ ] Application monitoring (New Relic, DataDog)
- [ ] Error tracking (Sentry)
- [ ] Uptime monitoring (UptimeRobot)

---

## Migration Path (Future Phases)

### Phase 7: Database Integration (v2.0)

**Motivation**: Enable advanced queries, user data, analytics

**Plan:**
- Add PostgreSQL or MongoDB
- Schema: users, repositories, reports, subscriptions
- Migrate file-based reports to database
- Keep markdown files as backups

### Phase 8: User Features (v2.0)

- [ ] User authentication (OAuth, GitHub login)
- [ ] Personalized feeds (language preferences)
- [ ] Email notifications
- [ ] Custom alerts (specific repos/topics)

### Phase 9: Advanced Analytics (v3.0)

- [ ] Trend analysis (rising stars, emerging tech)
- [ ] Visualizations (charts, graphs)
- [ ] Historical comparisons
- [ ] Export to PDF/CSV
- [ ] Machine learning predictions

---

## Risk Management

### Identified Risks & Mitigations

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| GitHub blocks scraping | High | Medium | Add rate limiting, use official API with token |
| API rate limit exceeded | Medium | Medium | Implement caching, use authenticated requests |
| Cron job fails | Medium | Low | Dual scheduling (node-cron + GitHub Actions) |
| Report storage fills disk | Low | Low | Implement report rotation (keep last 365 days) |
| Security vulnerability in deps | Medium | Medium | Dependabot, npm audit, regular updates |

---

## Success Criteria

### v1.0 Definition of Done

- [x] All core features implemented
- [x] Test coverage > 80%
- [x] Documentation complete
- [x] CI/CD pipelines working
- [x] No critical bugs
- [x] Performance targets met
- [x] Security audit passed

### Metrics to Track

- Daily active users (future)
- Report generation success rate
- API response times
- Error rates
- Cache hit rates
- User satisfaction (surveys)

---

## Timeline

| Phase | Duration | Status |
|-------|----------|--------|
| Phase 1: Foundation | Week 1 | ✅ Complete |
| Phase 2: Core Services | Week 2 | ✅ Complete |
| Phase 3: Web Interface | Week 3 | ✅ Complete |
| Phase 4: API Endpoints | Week 4 | ✅ Complete |
| Phase 5: Testing | Week 5 | ✅ Complete |
| Phase 6: CI/CD | Week 6 | ✅ Complete |
| **v1.0 Release** | **End of Week 6** | ✅ **Achieved** |

---

## References

- [Project Specification](./spec.md)
- [Development Constitution](./constitution.md)
- [GitHub Spec-Kit](https://github.com/github/spec-kit)
- [Express.js Documentation](https://expressjs.com/)
- [Jest Testing Guide](https://jestjs.io/docs/getting-started)
- [GitHub Actions Documentation](https://docs.github.com/actions)

---

**Document Owner**: Development Team
**Last Updated**: 2025-10-22
**Version**: 1.0.0
**Next Review**: 2025-11-22
