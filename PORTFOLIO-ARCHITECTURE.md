"# Brainstorm Architecture

## System Overview

Frontend (React) → Backend (Node.js) → Database (MongoDB)

## Data Models

Users: id, email (unique), username, subscription tier, usage counters
Cards: scryfallId (unique), name, set, rarity, cmc, prices (updated daily)
Portfolio: userId, cards[], totalValue (cached daily)
Watchlist: userId, cardId, targetPrice, alerts
Decks: userId, name, format, mainboard, sideboard

## Design System (Vault CSS)

Token Library: Colors (blue, gray, dark), typography (lg/base/sm), spacing (8px scale)
14+ Components: Button, Badge, Card, Input, AreaChart, Delta, CardArt
Dark mode by default, CSS variables for theming

## API Contracts

GUEST (Public):
- GET /api/cards/search → Card results
- POST /api/oracle/guest/chat → AI response
- GET /api/meta → Trending cards
- GET /api/news → News feed

SESSION (Auth Required):
- GET /api/users/me/portfolio → Holdings
- POST /api/users/me/portfolio/add → Add card
- POST /api/oracle/session/initialize → Deck builder
- GET /api/users/me/subscription → Tier + features

## Connection Pool Optimization

Problem: 3.1M connections/month = 4GB egress (~$60/mo)
Solution: maxIdleTimeMS 30s → 300s, minPoolSize 10 → 25
Result: ~84% reduction (~0.5GB/month), ~$50/mo savings

## Performance

Bundle: 84.4 kB gzipped
Load time: <2s p95
API response: <100ms p95
Database query: <50ms p95"
