# CLAUDE.md

Guidance for Claude Code working with code in this repository.

> This file is intentionally stack-agnostic. Sections marked **[Adapt]** describe a pattern that
> applies broadly; fill in the concrete tools, framework names, and conventions for the current
> project. Delete any subsection that does not apply.

## General Principles

- Generate concise, short solutions for new modules or code.
- KISS principle: Keep it simple, stupid.
- Watch for over-engineering and oversized files needing refactor.
- Watch for syntax/style mismatching the rest of the codebase.
- Watch for obvious bugs and logical flaws.
- Prioritize concise, precise code and documentation changes.
- No emojis or special characters in comments.
- Write an activity log in `/docs` to refer back to if confused.
- Create a to-do list and run major changes by the user first.
- Review existing files before refactoring or making major changes.
- Match the existing file/naming conventions of the repo (check a few sibling files first).
- Don't auto-commit activity logs and docs.

## Code Quality

### General Standards

- Use the right data structures and algorithms for the problem.
- Don't expose data needlessly (least privilege principle).
- No external libraries unless necessary; check the project's dependency manifest for existing versions before adding one.
- Avoid redundancy unless it improves usability or maintainability.
- Comments: one-liner describing what the code does, direct and actionable.
- No stray debug prints (`console.log`, `print`, etc.) in production code; use the project's proper logging setup.
- Keep functions small and single-purpose; extract when a function does more than one thing.
- Fail loudly in development, degrade gracefully in production.

### Project Structure **[Adapt]**

Follow the layout the project already uses. Common patterns:

- **Backend:** separate concerns into layers (routes/controllers, business logic/services, data/models, middleware, config, utils). Keep controllers thin and delegate logic to services.
- **Frontend:** separate by role (components, pages/views, hooks/composables, utils, services, assets, validators). One component per file; break large components into smaller reusable pieces.
- **Shared/monorepo:** keep shared types and utilities in a common package; avoid duplicating logic across apps.

State the actual folder structure here once it's established, so future changes stay consistent.

### Backend **[Adapt]**

- Use the language's modern async idioms consistently (e.g. `async/await` over raw callbacks/promise chains).
- Validate all inputs (query, params, body) before processing.
- Return consistent responses with correct status codes and clear error messages.
- Use environment variables for all config (database URL, API keys, port, secrets).
- Centralize error handling (middleware or equivalent); return meaningful messages and never leak stack traces in production.
- Use parameterized queries / an ORM's safe interfaces to prevent injection.
- Enforce data-access rules at the boundary appropriate to your stack (row-level security, policy layer, or service-layer checks).

### Frontend **[Adapt]**

- Prefer the framework's modern component model (e.g. functional components with hooks) over legacy patterns.
- One component per file; break large components into smaller, reusable pieces.
- Memoize expensive computations and callbacks.
- Lazy-load heavy components and routes via code splitting.
- Avoid prop drilling; use the framework's shared-state mechanism for genuinely global state (auth, theme, locale). Reach for an external store only when truly needed.
- Minimize re-renders: correct dependency arrays, avoid creating inline objects/functions in render.
- Prefer scoped styling (CSS modules, scoped styles, or styled-components) over inline styles.
- No unused imports or dead code.

### Reusable UI States (Errors, Empty, Loading) **[Adapt]**

If the project has shared components for error pages, empty states, or loading skeletons, document
their contract here so they get reused instead of re-implemented. For each, record:

- **Import path(s)** and any convenience wrappers.
- **Props / API** (e.g. a single `code` prop that renders any HTTP status).
- **Styling approach** (CSS modules vs global vs utility classes) and where the styles live.
- **When to render each variant** — for example:
  - `404` auto-rendered via a catch-all route.
  - `401` / `403` rendered inline when an auth or permission check fails.
  - `5xx` rendered from an error boundary or an API failure handler, with a sensible fallback.

Keep one source of truth for these so a status/state isn't styled two different ways in two places.

### Database **[Adapt]**

- Use migrations for all schema changes; never alter production directly.
- Index heavily-queried columns and foreign keys.
- Enforce access-control policies where the database supports them (e.g. row-level security for multi-tenant data).
- Avoid N+1 queries; use joins or batch queries.
- Keep relationships normalized; avoid storing redundant data.
- Use transactions for multi-step operations.

## Documentation

### Code Comments

- One-line comments above complex logic explaining the "why," not the "what."
- Doc comments for public functions, exported components, and utilities.
- No documentation for obvious code (e.g. `const user = getUser();`).

### README

- Clear setup instructions (runtime version, install dependencies, environment variables).
- How to start the dev server and run tests.
- Architecture overview (tech stack, folder structure).
- Key endpoints (if an API) or main features (if a frontend).

### API Documentation **[Adapt]**

- Document every endpoint: method, path, authentication, request body, response structure.
- Include example requests and responses.
- Document the status codes each endpoint can return.
- Keep this in route-file comments or a separate `API.md`.

Example format:
```
POST /api/users
Auth: Required (Bearer token)
Body: { name, email, password }
Response: { id, name, email, createdAt }
Errors: 400 (invalid email), 409 (email exists), 500 (server error)
```

### Database Schema **[Adapt]**

- Maintain a `DATABASE.md` or inline comments in migration files.
- Document tables, columns, relationships, constraints, and indexes.
- Explain any access-control policies.

## API Security & Endpoints **[Adapt]**

### Authentication & Authorization

- Use stateless tokens (e.g. JWT) or the framework's session mechanism; store credentials securely (HTTP-only cookies or secure storage).
- Validate tokens/sessions on every protected route.
- Rotate refresh tokens for long-lived sessions.
- Never put sensitive data (passwords, keys) inside tokens.
- Use role-based (or attribute-based) access control.

### Input Validation

- Validate and sanitize all user inputs (query, params, body).
- Use a schema-validation library for the language.
- Reject missing or malformed data early.
- Limit request payload size to prevent abuse.

### Data Protection

- Serve over HTTPS only; reject plaintext HTTP.
- Hash passwords with a strong adaptive algorithm (bcrypt, Argon2, scrypt); never store plaintext.
- Enforce authorization at the data layer where possible.
- Encrypt sensitive fields at rest when required.
- Set secure headers (`Content-Security-Policy`, `X-Frame-Options`, `Strict-Transport-Security`, etc.).

### Rate Limiting & Abuse Prevention

- Rate-limit auth-sensitive endpoints (login, signup, password reset).
- Use middleware/throttling appropriate to the stack.
- Set reasonable limits (e.g. a small number of login attempts per window).

### Response Standards

- Return consistent error responses, e.g.:
```json
{ "error": "Invalid email", "status": 400, "timestamp": "2024-01-10T10:00:00Z" }
```
- Never expose internal paths, database structure, or stack traces.
- Use generic error messages in production; log details server-side.

## Frontend Efficiency **[Adapt]**

### Performance

- Lazy-load routes and split code.
- Use dynamic imports for heavy libraries.
- Audit and minimize bundle size; remove unused packages.
- Cache API responses with appropriate headers.
- Optimize images: compress, use modern formats, lazy-load.

### State Management

- Keep state as close as possible to where it's used (local > shared context > external store).
- Use global state only for genuinely global concerns (auth, theme, language).
- Avoid unnecessary state updates; use immutable updates.

### Network Requests

- Use a single configured HTTP client instance with interceptors.
- Set request timeouts.
- Retry transient failures with backoff where safe (idempotent requests).
- Cache GET requests; invalidate on mutations.
- Always show loading and error states to users.

### Rendering Optimization

- Memoize components that receive stable props.
- Paginate or virtualize large lists.
- Avoid large inline styles or object creation in render.
- Use stable, unique `key`s in lists (never the array index).

## Testing **[Adapt]**

### Backend

- Unit tests for services and utilities.
- Integration tests for endpoints (request-to-response validation).
- Test error and edge cases.
- Mock external dependencies in unit tests; use a test database for integration tests.
- Set a minimum coverage target for critical paths.

### Frontend

- Unit tests for components and hooks.
- Test user interactions, not implementation details.
- Test error boundaries and fallback UIs.
- Avoid testing third-party library behavior.

### Test File Conventions

- Colocate tests with the code they cover (e.g. `component.test.*` next to `component.*`).
- Use descriptive test names: `should render error message when API fails`.

## Environment & Deployment **[Adapt]**

### Environment Variables

- Define all keys in `.env.example` (without real values).
- Never commit real `.env` files.
- Separate env files per stage (`.env.local`, `.env.test`, `.env.production`).
- Validate required env vars at startup.

### Local Development

- Provide setup instructions in the README.
- Containerize local dependencies (database, cache) if it simplifies onboarding.
- Use auto-restart / hot-reload tooling for the stack.

### Production Deployment

- Validate all environment variables at startup.
- Expose a health-check endpoint.
- Log errors and monitor uptime.
- Front the app with a reverse proxy for HTTPS, compression, and static files where applicable.
- Run tests in CI before deploying.

## Version Control

### Commits

- Commit after significant changes with clear, atomic messages.
- Use imperative mood: "Add user authentication," not "Added authentication."
- Reference issues where relevant: "Fix #42: Resolve login timeout."
- Keep commits focused; one feature or fix per commit.
- No auto-push; always review before pushing.

### Branches

- Use descriptive branches: `feature/user-auth`, `fix/login-timeout`, `docs/api-guide`.
- Keep branches short-lived (1-3 days).
- Require PR review before merging to main.

## AI Restrictions

### Data Privacy

- No real personal data (names, emails, phone numbers, account numbers, transactions) unless explicitly approved or using mock data.
- No credentials: passwords, API keys, tokens, connection strings, OAuth secrets.
- No confidential internal documentation or business logic.

### Security

- Never hardcode secrets; always use environment variables.
- Don't generate code that bypasses security checks.
- Sanitize and validate all user inputs before storage or processing.
- Don't expose error messages that leak system information.

## Lessons Learned (Recurring Pitfalls)

Project-specific hard-won lessons live here. Mine them from `/docs/activity-log.md`. Each entry should
cost real time or a user-visible bug only once — check against these before repeating a pattern. Keep
each entry in the same shape: a short title, a **Recognize** cue, and an **Instead** fix.

The following are stack-agnostic starters that tend to recur on most projects; keep the ones that apply
and add your own as you hit them.

### Third-party animation/config params don't always mean what they sound like
A library prop named `duration` (or `timeout`, `limit`, `size`) may feed an approximation rather than a hard bound — e.g. a spring is asymptotic, not time-bounded, so lowering its "duration" doesn't reliably cap wall-clock time.
- **Recognize:** the user gives a hard numeric constraint ("under 1.5s", "exactly N", "no more than X") for anything driven by a third-party parameter.
- **Instead:** check what the knob actually controls (read the type signature / source). For a hard bound, use a mechanism with a provable bound, compute the worst case, and state the guaranteed number back to the user.

### A "zero matches" search result is not proof of absence
A regex sweep can silently miss cases it wasn't shaped for (e.g. a Unicode range that skips astral-plane characters), then report "clean" when instances remain.
- **Recognize:** about to declare a codebase-wide sweep "clean" (no more X, no remaining references) from a single grep/regex pass — especially involving Unicode, generated code, or something already missed once.
- **Instead:** corroborate with a second, differently-shaped check (broader pattern, plain-text keyword, or reading the candidate files directly) before reporting completeness.

### Scoped styles still leak across pages via shared class names
Lazily-loaded page styles can persist after navigation; a generic class name defined in two page stylesheets creates a load-order-dependent conflict, not a source-order one.
- **Recognize:** about to add/edit a rule with a short generic class name (`.badge`, `.card`, `.compact`, `.icon`, `.dot`...) in a page-specific stylesheet.
- **Instead:** grep sibling page stylesheets for the same class first. If it's genuinely shared, move it to the shared/global stylesheet instead of letting two pages each define their own.

### Refactors leave dead styles and dead controls behind, silently
A selector that matches nothing doesn't error — it just stops applying. Changing an element's tag/structure can orphan its styles.
- **Recognize:** just changed an element's tag or structure (e.g. `<button>` → `<input>`, removed a wrapper).
- **Instead:** grep the paired stylesheet for selectors referencing the old element/class immediately after the change, not during some later audit.

### Ephemeral hosting breaks local-disk state assumptions
Writing state (tokens, uploads, caches) to local disk breaks on hosts whose filesystem is wiped on redeploy (serverless, containers, PaaS).
- **Recognize:** any file read/write used for state that must outlive a single process, on an ephemeral/serverless target.
- **Instead:** move that state to a database or object store before it becomes a deploy-day surprise.

### Don't auto-install a package whose identity you inferred yourself
Running `npx`/`pip install`/etc. for a tool named only generally by the user — where you had to search to find the actual package name — risks installing the wrong thing.
- **Recognize:** about to install a tool the user referenced by a general name, where you looked up the real package name.
- **Instead:** ask the user to confirm or run it themselves, or fall back to a read-only equivalent (its published docs) and say plainly the real thing isn't installed.

### Multiple dev servers need fixed, non-colliding ports
In a monorepo, apps sharing a default dev-server port cause whichever starts first to serve the other's requests — surfacing as a confusing "route that should obviously exist doesn't match."
- **Recognize:** debugging a routing/navigation error in a multi-app repo, especially a route that should clearly exist but doesn't match.
- **Instead:** check which app is actually listening on the port before touching routing code. Keep each app's port fixed (strict port) in its dev config.

### Resuming interrupted parallel work
When several parallel background tasks/subagents are interrupted mid-edit, resume each with an explicit "re-check current file state before continuing" instruction rather than just "continue," so no task duplicates or conflicts with work it already applied.

## Activity Log

- Record significant changes, decisions, and blockers in `/docs/activity-log.md`.
- Format: date, summary, decision/outcome.
- Reference this when returning to the codebase after a gap.
