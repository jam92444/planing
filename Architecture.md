### High-Level Enterprise-Grade Architecture

┌────────────────────┐
│ Frontend UI │ ← React + Tailwind (multi-tenant aware)
└────────┬───────────┘
│
▼
┌────────────────────┐
│ API Gateway │ ← Rate limiting, auth, routing
└────────┬───────────┘
▼
┌──────────────────────────────────────────────┐
│ Backend Services │
│ ┌────────────┐ ┌────────────┐ ┌────────────┐ │
│ │ Auth │ │ Billing │ │ Products │ │
│ │ + Roles │ │ + Discounts │ │ + Stock │ │
│ └────────────┘ └────────────┘ └────────────┘ │
│ ┌────────────┐ ┌────────────┐ ┌────────────┐ │
│ │ Subscription│ │ Reporting │ │ Branding │ │
│ │ Plans (Stripe)│ │ + Exports │ │ + Assets │ │
│ └────────────┘ └────────────┘ └────────────┘ │
└────────┬─────────────────────────────────────┘
▼
┌──────────────────────────────────────────┐
│ Data Layer (Multi-tenant) │
│ - MongoDB (per-tenant isolation or field)│
│ - Redis (cache) │
│ - S3 (logos, PDFs) │
└──────────────────────────────────────────┘
