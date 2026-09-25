# Agent Rules — <project-name>

## Tech Stack

### Repository / Tooling
1. Package manager & runtime: Bun (not npm/yarn/node directly)
2. Monorepo tooling: Turborepo + pnpm workspaces (apps/api, apps/web, packages/*)
3. Linter/formatter: Biome (not ESLint/Prettier)
4. Git hooks: Lefthook — don't bypass with --no-verify unless explicitly told to
5. Pre-commit checks run via Lefthook — lint, format, and typecheck must pass before a commit is made

### Frontend
6. Framework: Next.js (App Router, TypeScript)
7. UI components: shadcn/ui
8. Styling: TailwindCSS (v4 syntax — `@theme inline`, not the old config-based theme)
9. Data fetching/caching: TanStack Query
10. Tables: TanStack Table
11. Forms: react-hook-form + zod resolver

### Backend
12. Framework: Express.js (v5 — mind the path-to-regexp v8 wildcard syntax: use `*splat`, not `*`)
13. ORM: Drizzle ORM — schema in drizzle.config.ts, migrations via drizzle-kit
14. Validation: Zod for all input schemas
15. Auth: Better Auth, mounted on the Express API; frontend talks to it over HTTP, not directly to the DB

### Database
16. PostgreSQL

### Testing
17. API testing: Bruno collections (not Postman) — remember the collection-level Origin header workaround for CORS

## Conventions
18. Routers are mounted under /api/<domain> — one router per domain (auth, orders, payments, etc.)
19. New endpoints start as stubs returning 501 "Not implemented" until wired up — don't silently implement logic no one asked for
20. Use the existing branch workflow — feature branches off main, no direct pushes to main

## Boundaries
21. Don't modify drizzle-generated migration files directly — change the schema and regenerate
22. Don't add new auth logic outside Better Auth — no hand-rolled JWT, no parallel auth systems
23. Don't introduce a new state-management, ORM, CSS, or data-fetching library without asking first — one per category per repo
24. Don't touch packages/types without checking both apps/api and apps/web still compile against it
25. Don't rename or restructure the monorepo layout (apps/, packages/) without explicit approval
26. Don't bypass Lefthook/pre-commit checks without explicit instruction

## Style
27. TypeScript strict mode — no implicit any, no @ts-ignore without a comment explaining why
28. Explain non-obvious decisions in commit messages or PR descriptions — don't just say "fixed bug"

## Environment & Secrets
29. Never commit .env files or hardcode secrets/API keys — read from process.env only
30. When a new env var is needed, add it to .env.example with a placeholder value, not the real one
31. Don't log secrets, tokens, or full request/response bodies containing credentials

## Error Handling
32. Every Express route handler must handle errors explicitly (try/catch or error middleware) — no unhandled promise rejections
33. Return consistent error shapes across the API (e.g. { error: { code, message } }) — don't invent a new error format per route
34. Validate all external input (request body, query params, route params) with Zod before touching the DB — don't trust client data

## API Design
35. Follow REST conventions already established in the codebase (resource-based URLs, correct HTTP verbs/status codes) — don't mix in RPC-style endpoints without discussion
36. Paginate any endpoint that can return an unbounded list — check existing pagination pattern before inventing a new one
37. Keep TanStack Query keys consistent and colocated with the hooks that use them, not scattered inline

## Database
38. Every schema change goes through a Drizzle migration — no manual ALTER TABLE against the running DB
39. Add indexes for columns used in WHERE/JOIN/ORDER BY on tables expected to grow — but ask before adding an index "just in case"
40. Prefer transactions for multi-step writes that must succeed or fail together

## Testing & Verification
41. After generating backend logic, verify it against the Bruno collection before declaring it done — don't assume it works because it compiles
42. New endpoints get at least a happy-path Bruno request added to the collection
43. Run Biome + typecheck locally before considering a task complete, not just relying on the Lefthook hook to catch it

## Security
44. Sanitize/escape any user input rendered in the frontend — no raw HTML injection via dangerouslySetInnerHTML without a clear reason
45. Rate-limit or at least flag public-facing auth endpoints (login, signup, password reset) as needing it
46. CORS config should be explicit per-environment, not a wildcard `*` in production code

## Agent Behavior
47. When unsure which existing pattern to follow (e.g. how errors are shaped, how a similar route is structured), look for a precedent in the codebase before inventing a new one
48. Don't add new dependencies without checking package.json first — flag if something already covers the need
49. Prefer small, reviewable diffs over large rewrites unless explicitly asked to refactor
50. Ask before deleting or significantly restructuring existing files, even if they look unused
