"# Brainstorm — TCG Investment Platform

Full-stack web application for Magic: The Gathering investment and deck-building.

**Tech:** React, Node.js, MongoDB, CSS design system (Vault)
**Performance:** 84.4 kB gzipped, WCAG AA compliant
**Features:** Portfolio management, real-time pricing, metagame tracking, AI deck builder

## Core Features

Portfolio: View holdings, track value, price alerts, activity log
Card Search: Scryfall-grade filters, full-text search, guest access
Meta & News: Trending cards, format stats, TCG news feeds
Deck Builder: Form-based input, AI Oracle chat, deck generation

## Architecture

Frontend (React): Design system (Vault CSS with 14+ components), dark mode default
Backend (Node.js): Auth routes, portfolio, search, Oracle, admin
Database (MongoDB): Cards, portfolios, watchlist, decks
Payments (Stripe): Premium tier subscriptions

## Key Achievements

1. Design System: Complete CSS token library (colors, typography, spacing)
2. Connection Pool Optimization: Diagnosed 4GB/month egress, fixed with parameter tuning (84% reduction)
3. Full-Stack Patterns: Guest auth (public), session auth (premium), ML data collection
4. Performance: Bundle size 84.4 kB gzipped, load time <2s p95

## Deployment

Frontend (Vercel): Auto-deploy, CDN, $0
Backend (Render): Auto-deploy, $7/mo
Database (MongoDB): Free tier, $0

Questions: github.com/c0nf1ux"
