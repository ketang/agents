# Testing Standards

## Core Rules

- **Always write tests for new additions.** Tests ship with the code, not as a follow-up.
- **Update existing tests when modifying behaviour.**
- **Target >= 80% line coverage per package.** Glue/wiring packages are exempt.
- **Bug fixes require a reproduction test first** — write an automated test that demonstrates the bug (must fail), then fix the code and verify the test passes. Never attempt a fix without a failing test.

## Test Tiers

Define test tiers appropriate to the project. Typical structure:

| Tier | Use when | Typical time |
|---|---|---|
| Quick | During development | ~10-20s |
| Standard | Before committing | ~30-60s |
| Full | Before merge | ~2-5min |

## Unit vs Integration

- **Unit tests**: no DB, no network. Fast and isolated.
- **Integration tests**: use real external dependencies (database, APIs). Guard with environment checks so they skip gracefully when dependencies aren't available.
- **Never mock the database in integration tests.** Compilation and unit tests alone are not sufficient — runtime issues like type encoding mismatches only surface with a live connection.

## Test Organization

- Unit tests go next to the code they test (e.g., `foo.go` -> `foo_test.go`, `Foo.test.tsx`)
- Extract testable logic from functions that have side effects into separate, pure functions
- Use table-driven tests where appropriate
- Reuse shared test generators and fixtures — don't reinvent per test file

## Completion Checklist

Before declaring any feature or bug fix **done**, verify:

1. **Unit tests pass** — the module's own tests
2. **Linter clean** — no warnings in changed code
3. **Cross-boundary tests pass** — if touching shared types, protocols, or APIs
4. **End-to-end pipeline works** — if the project has E2E tests and you touched the relevant code path

A feature is not done until it works end-to-end. Closing based on unit tests alone risks silent breakage in the wider system.
