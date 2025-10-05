# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Common Development Commands

### Package Management
- `pnpm install` - Install dependencies
- `pnpm dev` - Start development server with Prisma generation and Turbo mode
- `pnpm dev:email` - Start email development server on port 9000
- `pnpm dev:content` - Watch and build Velite content
- `pnpm build` - Full production build (content + Prisma + Next.js)
- `pnpm start` - Start production server

### Code Quality
- `pnpm lint` - Lint source files in src directory
- `pnpm format` - Format TypeScript/JavaScript/CSS files with Prettier

### Database Operations
- `pnpm prisma db push` - Push database schema changes (required for setup)
- `pnpm prisma generate` - Generate Prisma client
- `pnpm prisma studio` - Open Prisma Studio for database management

### Content Management
- `pnpm build:content` - Build Velite content with clean flag
- Content files are located in `src/content/` with collections for changelog and about pages

## Architecture Overview

### Tech Stack
- **Framework**: Next.js 15 with App Router and React 19
- **Authentication**: Custom Lucia-based auth with GitHub OAuth and email verification
- **Database**: PostgreSQL with Prisma ORM
- **UI**: Shadcn/ui components with Radix UI primitives
- **Styling**: Tailwind CSS with CSS variables for theming
- **Content**: Velite for markdown content management
- **Payments**: Stripe integration for subscriptions
- **Email**: React Email with Resend
- **File Uploads**: UploadThing
- **Internationalization**: next-international with English and French support

### Project Structure

#### Authentication System
- Custom authentication built on Lucia patterns in `src/lib/server/auth/`
- GitHub OAuth integration with email verification fallback
- Session management with secure cookie handling
- User model includes Stripe integration fields for subscription management

#### Database Schema
- **User**: Core user data with GitHub ID, email verification, and Stripe fields
- **Session**: User session management
- **Project**: User-owned projects with domain mapping
- **EmailVerificationCode**: OTP-based email verification

#### UI Component Architecture
- Shadcn/ui components in `src/components/ui/`
- Layout components in `src/components/layout/`
- Section components for landing page in `src/components/sections/`
- Shared utilities and providers in `src/components/shared/`

#### Content Management
- Velite configuration handles markdown content collections
- Changelog entries in `src/content/changelog/`
- About pages in `src/content/about/`
- Type-safe content with Zod schema validation

#### Configuration
- Site configuration centralized in `src/config/site.ts`
- TypeScript path mapping with `~/*` alias for `src/*`
- Environment variables documented in `example.env`

### Development Patterns

#### Code Style (from .windsurfrules)
- Use functional and declarative programming patterns
- Prefer interfaces over types
- Use descriptive variable names with auxiliary verbs
- Minimize 'use client' directive, favor React Server Components
- Use lowercase with dashes for directories
- Structure files: exported component, subcomponents, helpers, static content, types

#### Authentication Flow
- Email-based signup with OTP verification
- GitHub OAuth as primary authentication method
- Server actions handle authentication state changes
- Middleware protection for authenticated routes

#### Server Actions
- Type-safe server actions using next-safe-action
- Authentication middleware for protected actions
- Error handling with custom error types (e.g., FreePlanLimitError)

#### Database Patterns
- Prisma client singleton pattern in `src/lib/server/db.ts`
- Transaction handling for complex operations
- Soft validation with Zod schemas

## Environment Setup

1. Copy `.env.example` to `.env` and configure:
   - PostgreSQL database URLs (Vercel Postgres recommended)
   - GitHub OAuth credentials
   - Resend API key for emails
   - UploadThing keys for file uploads
   - Stripe keys for payment processing

2. Database initialization:
   ```bash
   pnpm prisma db push
   ```

3. Start development:
   ```bash
   pnpm dev
   ```

## Key Integrations

### Stripe Subscription Management
- Webhook handling in `src/app/api/stripe/route.ts`
- User subscription data stored in User model
- Billing forms and subscription management in dashboard

### File Upload System
- UploadThing integration in `src/app/api/uploadthing/`
- Image upload modal component for user avatars
- Utility functions for CDN URL handling

### Email System
- React Email templates in `emails/` directory
- Resend integration for transactional emails
- Email verification workflow

### Internationalization
- Locale-based routing with `[locale]` dynamic segments
- Translation files in `src/locales/`
- Locale toggler component for language switching

## Deployment Notes

- Configured for Vercel deployment with environment variables
- Service worker integration with Serwist for PWA capabilities
- Optimized builds with Turbo mode in development
- Git hooks configured with Lefthook for pre-commit checks