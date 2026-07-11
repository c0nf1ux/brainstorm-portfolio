# Brainstorm: Key Decisions & Trade-Offs

## 1. Design System as Foundation (Vault CSS)

Decision: Implement complete CSS token library before building components.

Why: Consistency across all screens, dark/light mode support, component reuse.

Result: 84.4 kB gzipped, zero styling bugs, easy theming.

## 2. Guest vs. Session Authentication

Decision: Public features (search, news) require no auth. Premium features (portfolio, deck builder) require JWT.

Why: Lower friction, clear monetization boundary, collect behavior data from paid users.

Result: High engagement on public features, clear conversion funnel.

## 3. Connection Pool Optimization

Decision: Diagnose and fix 4GB/month egress by tuning MongoDB parameters.

Why: $60/month waste. maxIdleTimeMS 30s caused millions of connection creates.

Solution: Increase maxIdleTimeMS 30s → 300s, minPoolSize 10 → 25.

Result: ~84% reduction (3.1M → 500k connections/month), ~$50/month savings, faster queries.

## 4. Real-time Pricing Integration

Decision: Connect to live card pricing instead of stale data.

Why: Trust (current market prices), competitive advantage, accurate portfolio.

Result: Portfolio value always current, better UX.

## 5. Rules-Based Oracle v0 (ML Foundation)

Decision: Build rules-based AI assistant that collects behavior data for future ML training.

Why: Launch without ML infrastructure, gather training data immediately.

Result: ML-ready foundation, low operational cost now.

## 6. Mock Data → Real APIs (Incremental)

Decision: Start with mock data, incrementally replace with real backend APIs.

Why: Frontend ships before backend, reduces integration pain.

Result: Parallel development, lower integration risk.

## Summary

Every decision balances launch speed with production-ready quality, scalability, and operational efficiency.
