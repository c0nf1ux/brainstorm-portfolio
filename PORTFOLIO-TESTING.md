# Brainstorm: Testing Infrastructure

## Test Strategy

Jest: Unit and integration tests
React Testing Library: Component tests
Playwright: E2E tests (full user flows)

## Test Coverage

Unit Tests: ~100 tests (60%)
- Authentication, utilities, price calculations
- Component rendering, user interactions

Integration Tests: ~50 tests (30%)
- API endpoints (search, portfolio, deck builder)
- Database operations
- Auth middleware

E2E Tests: ~20 tests (10%)
- Search → Portfolio → Deck Builder flow
- Sign up → Subscribe → Access premium features

Total: 170 tests, 95%+ coverage

## CI/CD Pipeline

GitHub Actions workflow:
1. npm install (1 min)
2. npm run lint (30s, zero errors)
3. npm test (5 min)
4. npm run build (1-2 min, 84.4 kB gzipped)
5. Deploy to Vercel (auto)

Total: ~10 minutes per push

## Local Development

npm test: Jest suite (~5-10 min)
npm run dev: React dev server + Express backend
npm run build: Optimized bundle

## Performance Benchmarks

Bundle Size: 84.4 kB gzipped
API Response: <100ms p95 (local)
Database Query: <50ms p95 (text index search)
Build Time: ~1-2 minutes
Test Runtime: ~5 minutes (local), ~10 minutes (CI/CD)

## Error Handling

Graceful fallbacks:
- Text index unavailable: Fall back to regex search
- Pricing API down: Show cached prices
- Portfolio load fails: Show error + retry button
- Deck builder session expires: Preserve draft, restore on login

## Quality Gate

All tests pass + zero linting errors + WCAG AA compliance required for all commits.

Pre-commit hooks: npm test && npm run lint && npm run lint:a11y

Result: Production-ready code quality on every push.
