# Brainstorm: Operations & Deployment

## Deployment Architecture

Frontend (Vercel): React, auto-deploy, CDN, $0
Backend (Render): Node.js/Express, auto-deploy, $7/mo
Database (MongoDB Atlas): Free M0 tier, auto-backups, $0

## Connection Pool Tuning (Major Optimization)

Root Cause: 3.1M connections/month = 4GB egress = ~$60/month

Solution:
- maxIdleTimeMS: 30s → 300s (idle connection lives 5 min)
- minPoolSize: 10 → 25 (pre-warm connections)

Impact:
- Connections: 3.1M → 500k (84% reduction)
- Egress: 4GB → 0.5GB (~$50/month savings)
- Latency: Faster queries (no connection creation)

## Cost Analysis (Launch)

Monthly: Vercel $0 + Render $7 + MongoDB $0 = $7
Plus Stripe fees: 2.9% + $0.30 per premium signup

At 100 premium subscriptions/month ($999):
- Stripe: ~$29
- Infrastructure: $7
- Total: ~$36 (3.6% of revenue)

## Monitoring

Frontend: Build success, page load times, 404 errors
Backend: API response times (<100ms p95), error rates, connection pool
Database: Query performance (<50ms p95), storage, index hit ratio

## Disaster Recovery

Backup: MongoDB Atlas daily auto-backups, 7-day retention
Restore: Navigate to Atlas console, select backup time, create cluster, update MONGODB_URI
Test: Monthly restore drill to staging

## Scaling (Future)

Phase 1: Single node (Vercel free, Render small, MongoDB M0)
Phase 2: Horizontal (Render multiple instances, MongoDB M2 + text index optimization)
Phase 3: Global (Vercel CDN, regional replicas)

## Performance Targets

Bundle Size: <100 kB gzipped (currently 84.4 kB)
Load Time: <2s p95 (Vercel CDN)
API Response: <100ms p95 (local)
Database Query: <50ms p95 (text index)
Build Time: <2 minutes
Test Runtime: ~5-10 minutes
