# Project Conventions

<!--
  INSTRUCTIONS: Customize this file for your project.
  This is loaded automatically at the start of every AI session.
  Focus on tribal knowledge that lives nowhere in the code.
  Keep it under ~500 tokens — every token here costs context in every session.
-->

## Tech Stack

<!-- List your core technologies, frameworks, and key libraries -->
- Language: [e.g., TypeScript, Go, Python, Rust]
- Framework: [e.g., Next.js, Django, Gin]
- Database: [e.g., PostgreSQL, MongoDB]
- Testing: [e.g., Jest, pytest, go test]

## Build & Run

```bash
# Install dependencies
# [e.g., npm install, go mod download, pip install -r requirements.txt]

# Run tests
# [e.g., npm test, make test, pytest]

# Lint
# [e.g., npm run lint, golangci-lint run]

# Build
# [e.g., npm run build, go build ./...]
```

## Non-Negotiables

<!-- Things you'd tell a new hire on day one. These apply to EVERY task. -->

- [e.g., Never mock the database in integration tests]
- [e.g., All API endpoints must have request validation]
- [e.g., Use absolute imports, never relative]
- [e.g., Error messages must be user-facing friendly, no stack traces]
- [e.g., All database changes require a migration file]

## Architecture Decisions

<!-- Conventions that aren't obvious from the code -->

- [e.g., We avoid ORMs because of X — use raw SQL with the query builder]
- [e.g., The auth service talks to the legacy system through a specific adapter pattern in pkg/adapters/]
- [e.g., Don't touch the billing module without running integration suite against staging]
- [e.g., Feature flags are in LaunchDarkly, not env vars]

## Code Style

<!-- Only include what linters/formatters DON'T catch -->

- [e.g., Prefer composition over inheritance]
- [e.g., Handler functions follow: validate → authorize → execute → respond]
- [e.g., Use domain errors, not generic HTTP errors, in business logic]

## What NOT to Do

<!-- Anti-patterns specific to this project -->

- [e.g., Don't add new dependencies without team discussion]
- [e.g., Don't use console.log for debugging — use the structured logger]
- [e.g., Don't bypass the API gateway for service-to-service calls]

## Directory Structure

<!-- Only include if the structure isn't self-explanatory -->

```
src/
├── api/          # HTTP handlers and middleware
├── domain/       # Business logic, no framework dependencies
├── infra/        # Database, external services, adapters
└── shared/       # Cross-cutting utilities
```

## PR & Review Conventions

<!-- How this team works -->

- [e.g., All PRs need at least one approval]
- [e.g., Include a test plan in PR description]
- [e.g., Squash merge to main]
