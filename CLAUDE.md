# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

```bash
# Development
pnpm install                    # Install dependencies
pnpm dev                        # Start development server with Turbo
pnpm build                      # Build app (includes DB migration)
pnpm start                      # Start production server

# Code Quality
pnpm lint                       # Run ESLint and Biome linter
pnpm lint:fix                   # Fix linting issues
pnpm format                     # Format code with Biome

# Database Operations
pnpm db:generate                # Generate database migrations
pnpm db:migrate                 # Run database migrations
pnpm db:studio                  # Open Drizzle Studio
pnpm db:push                    # Push schema changes to database
pnpm db:pull                    # Pull schema from database

# Testing
pnpm test                       # Run Playwright e2e tests
```

## Architecture Overview

This is a Next.js AI chatbot application built with the AI SDK, featuring:

### Core Structure
- **Next.js 15** with App Router and React Server Components
- **AI SDK v5** for LLM integration with xAI Grok models as default
- **Authentication** via Auth.js with email/password and guest support
- **Database** using Drizzle ORM with PostgreSQL (Neon)
- **Styling** with Tailwind CSS and shadcn/ui components

### Key Directories
- `app/(auth)/` - Authentication pages (login, register) with middleware protection
- `app/(chat)/` - Main chat interface and API routes for streaming
- `artifacts/` - AI-generated content handlers (code, text, images, sheets)
- `components/` - Reusable UI components and chat-specific components
- `lib/ai/` - AI model configurations, providers, and tools
- `lib/db/` - Database schema, queries, and migration utilities
- `hooks/` - Custom React hooks for chat and UI state management

### Database Schema
The app uses a versioned message system:
- `User` - User accounts with email/password
- `Chat` - Chat sessions with visibility controls
- `Message_v2` - Current message format with parts and attachments
- `Document` - AI-generated artifacts (text, code, images, sheets)
- `Suggestion` - Document editing suggestions
- `Vote_v2` - Message voting system

### AI Integration
- Models configured in `lib/ai/providers.ts` with test mocks
- Streaming chat via `/api/chat/[id]/stream/route.ts`
- AI tools for document creation, weather, and suggestions in `lib/ai/tools/`
- Reasoning model support with middleware for advanced thinking

### Authentication Flow
- Middleware in `middleware.ts` handles route protection
- Guest users get temporary sessions via `/api/auth/guest`
- Protected routes redirect unauthenticated users automatically

### Environment Setup
Requires environment variables for:
- `AUTH_SECRET` - NextAuth session secret
- Database connection (Neon/Vercel Postgres)
- AI provider API keys (xAI)
- Optional: Redis for session management

### Testing
- Playwright e2e tests in `tests/` directory
- Test fixtures and helpers for consistent test data
- Tests cover chat functionality, artifacts, and reasoning features