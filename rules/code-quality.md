# Code Quality Standards

## Inline Documentation

Comments explain **why** or document **non-obvious contracts** — never restate what the code already says. If a name needs a comment, choose a better name first.

**What to document:**
- Public API contracts: preconditions, postconditions, error behavior, ownership semantics
- Non-obvious design choices: why an algorithm was chosen, why a field exists, why ordering matters
- Lint/warning suppressions: always explain why the suppression is needed

**What NOT to document:**
- What the code does when the code already says it (`// returns the sum` above `fn sum()`)
- Type information visible in the signature (`// takes a string` above `fn foo(s: string)`)
- Existence of language constructs (`// uses a switch statement`)

**The delete test:** If you can delete a comment and the code is equally clear, delete it.

## No Magic Numbers or String Literals

Define named constants for default values, timeouts, error codes, capability lists, and any value that appears in both production code and tests. Tests must reference the constant, not duplicate the literal.

## No Hardcoded Absolute Paths

Never write a literal absolute path (e.g., `"/home/user/project/..."`) in code, tests, output artifacts, or documentation. Runtime-computed absolute paths are fine — use the language's path resolution utilities. The rule prohibits hardcoded paths that bake in machine-specific locations.

## Parallel Code Paths Must Maintain Parity

When two functions process the same domain, they must handle the same cases. When adding a capability to one path, check the other. Grep for the parallel code path before declaring done.

## Security Basics

- **Never commit** `.env` or files containing secrets — only `.env.example`
- **Never add** `node_modules/`, `dist/`, `target/`, or build output to git
- **Never use string interpolation to build SQL** — always use parameterized queries
- **Never bypass linter warnings** without a comment explaining why

## File Format Preferences

- **YAML** for structured data and configuration files
- **Markdown** for formatted text (documentation, plans, notes, specs)
