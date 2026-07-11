# Brainstorm Architecture

## System Overview

Frontend (React, Vercel) → Backend (Node.js, Render) → Database (MongoDB, Atlas)

## Data Models

### Users
{
  _id: ObjectId,
  email: String (unique),
  passwordHash: String (bcrypt),
  username: String (unique, lowercase),
  firstName: String,
  lastName: String,
  role: Enum (user | admin),
  isActive: Boolean (default true),
  subscription: {
    tier: Enum (free | $9.99/mo | $19.99/mo),
    status: Enum (active | cancelled | past_due),
    billedAt: Date
  },
  usage: {
    dailySearchCount: Number,
    portfolioLimit: Number,
    alertsLimit: Number,
    lastDailyReset: Date
  },
  createdAt: Date,
  updatedAt: Date
}

### Cards
{
  _id: ObjectId,
  scryfallId: String (unique),
  name: String,
  setCode: String,
  rarity: Enum (common | uncommon | rare | mythic),
  cmc: Number,
  type: String,
  keywords: [String],
  prices: {
    usd: Number,
    usdFoil: Number,
    timestamp: Date
  },
  imageUrl: String,
  createdAt: Date,
  updatedAt: Date
}

Text index on: name, type, keywords (for full-text search)

### Portfolio
{
  _id: ObjectId,
  userId: ObjectId (ref User),
  cards: [{
    cardId: ObjectId,
    quantity: Number,
    acquiredPrice: Number,
    acquiredDate: Date
  }],
  totalValue: Number (cached, recalculated daily),
  createdAt: Date,
  updatedAt: Date
}

### Watchlist
{
  _id: ObjectId,
  userId: ObjectId,
  cardId: ObjectId,
  targetPrice: Number,
  priceAlert: Boolean (default true),
  priceAlertChannel: Enum (email | sms | both),
  createdAt: Date
}

### Decks
{
  _id: ObjectId,
  userId: ObjectId,
  name: String,
  format: Enum (standard | modern | legacy | commander),
  mainboard: [{
    cardId: ObjectId,
    quantity: Number
  }],
  sideboard: [{
    cardId: ObjectId,
    quantity: Number
  }],
  description: String,
  createdAt: Date,
  updatedAt: Date
}

## API Contracts

### Authentication
POST /api/auth/register
{ email, password (8+ upper/lower/digit/special), firstName, lastName, username }
Response 201: { user, token }

POST /api/auth/login
{ email, password }
Response 200: { user, token }

### Card Search
GET /api/cards/search?q=...&set=...&rarity=...&type=...&format=...
Query: Full-text search with optional filters
Response 200: { cards: [...], total, page }

GET /api/cards/search?q=fireball
Response: { cards: [{ id, name, setCode, prices, imageUrl, ... }] }

### Portfolio
GET /api/users/me/portfolio
Header: Authorization: Bearer <token>
Response 200: { portfolio: { cards: [...], totalValue: ... } }

POST /api/users/me/portfolio/add
{ cardId, quantity, acquiredPrice }
Response 201: { portfolio }

GET /api/users/me/subscription
Header: Authorization: Bearer <token>
Response 200: { 
  tier, status, billedAt,
  features: { dailySearchLimit, portfolioSizeLimit, alertsLimit }
}

### Deck Builder
POST /api/oracle/session/initialize
{ colors, format, budget }
Response 201: { sessionId, initialDraft: [cards...] }

POST /api/oracle/chat
Header: Authorization: Bearer <token>
{ sessionId, message: "add more removal spells" }
Response 200: { response: "...", updatedDeck: [...] }

POST /api/oracle/guest/chat
{ message: "what's a good aggressive deck?" }
Response 200: { response: "..." }

### Meta & News
GET /api/meta
Response 200: { trending: [...], formats: { standard: {...}, modern: {...} } }

GET /api/news
Response 200: { articles: [...], meta: [...] }

## Guest vs. Session Authentication

### Guest Flow (No Auth)
GET /api/cards/search → Public, no auth required
POST /api/oracle/guest/chat → Public, no auth required
GET /api/meta → Public, no auth required
GET /api/news → Public, no auth required

Benefits: High engagement, no login friction, explore before committing

### Session Flow (JWT Required)
GET /api/users/me/portfolio → Requires auth
POST /api/users/me/portfolio/add → Requires auth
POST /api/oracle/session/initialize → Requires auth (collects behavior data)
GET /api/users/me/subscription → Requires auth

Benefits: Clear monetization boundary, user behavior tracking for ML

## Connection Pool Architecture

### Problem
3.1M connections created/destroyed per month = 4GB egress (~$60/mo)
maxIdleTimeMS: 30s caused connection closure after 30s idle
Each request: Create → Use → Close → Repeat

### Solution
maxIdleTimeMS: 300s (5 minutes) — idle connection lives longer
minPoolSize: 10 → 25 — pre-warm connections, don't create on demand

### Result
Expected: 3.1M → ~500k connections/month
Savings: ~4GB → ~0.5GB egress (~$50/mo reduction)
Side benefit: Faster query response (no connection creation overhead)

## Design System (Vault CSS)

### Token Library (CSS Variables)
Colors:
--primary-500: #3b82f6 (blue)
--primary-600: #2563eb (darker blue)
--primary-700: #1d4ed8 (darkest blue)
--gray-500: #6b7280
--gray-700: #374151 (dark)
--error-500: #ef4444 (red)

Typography:
--text-lg: 18px / 1.5
--text-base: 16px / 1.5
--text-sm: 14px / 1.5
--font-bold: 700
--font-semibold: 600

Spacing:
--space-1: 8px
--space-2: 16px
--space-3: 24px
--space-4: 32px

### Component Library
Button: Primary, secondary, disabled states, size variants
Badge: Success, warning, error colors
Card: Content container, hover effects
Input: Text, email, password, with validation states
AreaChart: Portfolio value over time (recharts)
Delta: Price change indicator (up/down with colors)
CardArt: TCG card display with image + metadata

All components use CSS variables, support dark/light mode via data-theme attribute

## Performance Characteristics

Bundle Size: 84.4 kB gzipped (all React + CSS + logic)
Load Time: <2s p95 (Vercel CDN)
API Response: <100ms p95 (local), <500ms p95 (with pricing API)
Database Query: <50ms p95 (text index search)

## Deployment

Frontend (Vercel): Auto-deploy on git push, CDN edge caching
Backend (Render): Auto-deploy, connection pool optimized (maxIdleTimeMS 300s, minPoolSize 25)
Database (MongoDB Atlas): Free M0 tier, text index on cards collection

## Monitoring

Frontend: Build success, page load times, 404 errors
Backend: API response times, error rates, connection pool usage
Database: Query performance, storage usage, index hit ratio

## Scalability (Future)

Redis: Cache frequently accessed cards, user sessions
Read Replicas: Analytics queries on separate MongoDB replica
Sharding: By userId for portfolio data if millions of users
CDN: Image caching for card artwork (already on Vercel)
