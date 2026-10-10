## Business Requirements

This project is building a [project name] e-commerce store App. Key features:
- Anyone can register with name, email, and password and sign in; a signed-in user can update their name on their profile page
- Visitors can browse latest and featured products (with a banner carousel and deal countdown), search by name, and filter by category, price, and rating with sorting and pagination
- Anyone can add products to a cart; a guest's cart is kept with a session cookie and moved to their account when they sign in
- A signed-in user checks out in steps: shipping address, payment method (PayPal, Stripe, or Cash on Delivery), then place order; a receipt is emailed when an order is paid
- A signed-in user can see their order history and leave or update one rating and review per product
- Admins have a dashboard with sales stats and a monthly sales chart, and can manage products (with multiple image uploads and an optional featured banner), orders (mark paid or delivered, delete), and users (edit name and role, delete)
- The UI supports light, dark, and system themes, and gives toast feedback, loading states, and custom unauthorized and 404 pages

## Limitations

Seeded demo accounts are `admin@example.com` / `123456` (admin) and `user@example.com` / `123456` (user); running `db/seed.ts` (there is no npm script for it) resets products and users.

Sign-in is email and password only; there are no OAuth providers, email verification, password reset, or self-service account deletion.

There are only two roles, `user` and `admin`. Product search matches on name only.

Prices use fixed rules: free shipping over $100 (otherwise $10) and 15% tax. Stock is decreased when an order is paid.

The PostgreSQL database (Neon), PayPal, Stripe, Uploadthing, and Resend credentials must be supplied in `.env`. The app runs locally with `npm run dev` and is deployed to Vercel; there is no Docker setup.

## Technical Decisions

- Next.js (App Router) with React, written in TypeScript with the `@/` import alias; shared types are inferred from Zod schemas in `types/`
- Use route groups: `app/(root)` for the store and checkout, `app/(auth)` for sign-in and sign-up, `app/user` for the user area, and `app/admin` for the admin area, each with its own layout
- No separate backend: pages are async server components; data reads and mutations are server actions in `lib/actions/*.actions.ts` that return `{ success, message }` (using `formatError`) and call `revalidatePath` after changes. Route handlers in `app/api/` are only for NextAuth, Uploadthing, and the Stripe webhook
- Mark components `'use client'` only when they need browser state or interactivity
- ShadCN UI (Radix primitives in `components/ui/`) with Tailwind CSS, Lucide icons, and `next-themes` for dark mode; use the ShadCN toast (`hooks/use-toast.ts`) for notifications
- React Hook Form with `@hookform/resolvers/zod` for forms; define every input schema in `lib/validators.ts` and parse with it in the server action before writing to the database
- NextAuth.js v5 (Auth.js) with the Credentials provider, passwords hashed with `bcrypt-ts-edge`, JWT sessions, and the Prisma adapter; the session includes the user's `id` and `role`
- `middleware.ts` uses the edge-safe `auth.config.ts` to protect checkout, `/user`, `/order`, and `/admin` routes and to set the `sessionCartId` cookie; admin pages call `requireAdmin()` from `lib/auth-guard.ts`, and admin server actions must also check the admin role
- PostgreSQL on Neon with Prisma ORM (`prisma/schema.prisma`, migrations in `prisma/migrations/`) through the Neon serverless adapter in `db/prisma.ts`; that client converts `Decimal` prices to strings, and results are passed to client components with `convertToPlainObject`. Run multi-table changes in `prisma.$transaction`
- PayPal REST API (`lib/paypal.ts`) with React PayPal JS, Stripe Payment Intents with React Stripe JS plus a webhook at `app/api/webhooks/stripe`, and Cash on Delivery marked paid by an admin
- Uploadthing for product and banner image uploads, allowed only for signed-in users
- Resend with React Email templates in `email/` for purchase receipts
- Recharts for the admin sales chart, Embla Carousel for the featured banner, `query-string` for building filter URLs, and `slugify` for product slugs
- Configuration such as page size, payment methods, and roles lives in `lib/constants/index.ts` with `.env` overrides
- Jest with `ts-jest` in `tests/`, with `npm test` and `npm run test:coverage` (run with `TZ=UTC`); coverage thresholds are 98% lines/statements, 95% functions, and 90% branches

## Coding Standards

- Follow the coding standards and coding style in the [jstoops/prostore](https://github.com/jstoops/prostore) repo, and use its code as the example to match for structure, naming, and patterns
