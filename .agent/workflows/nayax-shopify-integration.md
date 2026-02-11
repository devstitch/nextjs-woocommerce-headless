---
description: Nayax-Shopify Integration Development Workflow
---

# Nayax-Shopify Integration Workflow

This workflow guides the implementation of the Nayax-Shopify integration using Vercel PostgreSQL and Prisma.

## Prerequisites

- Vercel CLI installed and logged in
- Shopify Partner account
- Nayax account with webhook access

## Implementation Steps

### 1. Project Setup

// turbo

1. Create a new Next.js app or use existing one:
   ```bash
   npx create-next-app@latest nayax-shopify-app --typescript --tailwind --app
   ```
2. Install dependencies:
   ```bash
   npm install prisma @prisma/client @shopify/shopify-api @shopify/polaris @shopify/app-bridge-react bcryptjs jsonwebtoken date-fns zod
   npm install -D @types/bcryptjs @types/jsonwebtoken
   ```

### 2. Database Initialization

1. Initialize Prisma:
   ```bash
   npx prisma init
   ```
2. Link to Vercel and pull environment variables:
   ```bash
   vercel link
   vercel env pull .env.local
   ```
3. Update `prisma/schema.prisma` with the schema defined in `docs/nayax-shopify-integration-plan.md`.
4. Push schema to database:
   ```bash
   npx prisma db push
   ```
5. Generate Prisma Client:
   ```bash
   npx prisma generate
   ```

### 3. Core Implementation

1. Implement Database Helper: Create `lib/db.ts`.
2. Implement Shopify Utility: Create `lib/shopify.ts`.
3. Implement Security Utilities: Create `lib/security/webhook.ts`.
4. Implement OAuth Flow:
   - `app/api/auth/route.ts`
   - `app/api/auth/callback/route.ts`

### 4. Webhook & Event Processing

1. Implement Nayax Webhook Endpoint: `app/api/webhooks/nayax/route.ts`.
2. Implement Event Processor: `app/api/process-event/route.ts`.
3. Implement Queue Processor: `app/api/cron/process-queue/route.ts`.

### 5. Deployment & Configuration

1. Configure Vercel Cron in `vercel.json`.
2. Deploy to Vercel:
   ```bash
   vercel --prod
   ```
3. Set environment variables in Vercel dashboard.

## Monitoring

- Check Sentry for errors.
- Monitor Slack alerts for failed events.
- Review Audit Logs in the database.
