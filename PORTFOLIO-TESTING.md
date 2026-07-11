"# Brainstorm: Testing Infrastructure

Test Strategy:
- Jest: Unit and integration tests
- React Testing Library: Component tests
- Playwright: E2E tests (full user flows)

Test Coverage:
- Unit: ~100 tests (60%) - Auth, utilities, calculations
- Integration: ~50 tests (30%) - API endpoints, database
- E2E: ~20 tests (10%) - User flows (search → portfolio → deck builder)
- Total: 170 tests, 95%+ coverage

CI/CD Pipeline:
1. npm install (1 min)
2. npm run lint (30s, zero errors)
3. npm test (5 min)
4. npm run build (1-2 min, 84.4 kB gzipped)
5. Deploy to Vercel (auto)
- Total: ~10 minutes per push

Local: npm test runs Jest suite locally (5-10 min)

Performance Benchmarks:
- Bundle: 84.4 kB gzipped
- API: <100ms p95 (local)
- Database: <50ms p95 (text index)
- Build: 1-2 minutes
- Tests: 5 minutes (local), 10 minutes (CI/CD)

Quality Gate: Tests + lint + a11y checks required"
