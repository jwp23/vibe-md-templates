---
name: conventional-commits
description: Use when running git commit, /commit, or any commit workflow. REQUIRED for all commits in this project.
---

# Conventional Commits

**This skill is MANDATORY for all commits in this project.** See CLAUDE.md § Git Commits.

## Rules

When creating a git commit message, follow these rules strictly:

1. **Format**: `<type>: <description>` (50 characters max, including type)
2. **Single line only** — no body, no footer, no multi-line messages
3. **Never reference story numbers, ticket IDs, or PRD references**
4. **Describe WHAT was done, not HOW** — focus on the outcome, not implementation details

### Allowed Types

| Type | Use When |
|------|----------|
| `feat` | Adding new functionality |
| `fix` | Fixing a bug |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace (no code logic change) |
| `refactor` | Restructuring code without changing behavior |
| `perf` | Performance improvements |
| `test` | Adding or updating tests |
| `build` | Build system or dependency changes |
| `ci` | CI/CD configuration changes |
| `chore` | Maintenance tasks, tooling updates |

### Optional Scope

Add scope in parentheses for context: `feat(auth): add login button`

Use scope sparingly — only when it adds clarity.

## Examples

**Good commits:**
- `feat: add user profile page`
- `fix: resolve null pointer on logout`
- `docs: update API authentication guide`
- `refactor: simplify cart total logic`
- `feat(api): add rate limiting`

**Bad commits (and why):**
- `feat: add user profile page with React components and Redux state` — describes HOW, not WHAT
- `fix: resolve null pointer on logout [JIRA-1234]` — contains ticket reference
- `Updated the thing` — missing type, vague description
- `feat: implement the user authentication flow using JWT tokens stored in httpOnly cookies` — too long, describes implementation

## Commit Command

When executing the commit, use:
```bash
git commit -m "<type>: <description>"
```