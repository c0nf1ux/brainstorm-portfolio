"# Brainstorm: Operations & Deployment

Deployment: Vercel (frontend), Render (backend), MongoDB Atlas (database)

Environment:
- MONGODB_URI: Database connection
- API endpoints: Stripe, pricing service
- Frontend: REACT_APP_API_URL pointing to backend

Cost Analysis (Launch):
- Vercel: $0 | Render: $7 | MongoDB: $0
- Stripe: 2.9% + $0.30 per premium signup
- At 100 subscriptions/month ($999): $36 infrastructure (~3.6%)

Monitoring:
- Frontend: Build success, page load, 404 errors
- Backend: API response (<100ms p95), error rates, connection pool
- Database: Query performance (<50ms p95), storage usage

Scaling (Future):
- Phase 1: Single node (current)
- Phase 2: Horizontal (multiple instances, upgraded MongoDB)
- Phase 3: Global (regional replicas)"
