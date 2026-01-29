---
name: clean-code-js
description: "Clean Code principles for JavaScript. Use when writing, generating, or modifying JavaScript/TypeScript code. Triggers: write code, create function, implement, generate JS/TS, build component."
---

# Clean Code Principles for JavaScript

Apply these principles when writing JavaScript or TypeScript code. Priority order: (1) Correctness, (2) Readability, (3) Performance. When in doubt, choose the more readable solution.

## The Golden Rules

### Naming
- **Intention-revealing names**: Names should explain why it exists, what it does, how it's used
- **No cryptic abbreviations**: Only `i`, `j`, `k` acceptable in small loops
- **Searchable names**: Extract magic numbers to UPPER_SNAKE_CASE constants
- **No encodings**: No Hungarian notation, no `m_` prefixes, no `I` for interfaces

### Functions
- **Keep functions small**: Target <20 lines, ideal 2-4 lines
- **Do one thing**: Single responsibility at a single abstraction level
- **Minimize arguments**: Ideal 0-2, avoid flag arguments (split into separate functions)
- **No side effects**: Function should only do what its name suggests
- **Descriptive names**: Long descriptive name > short cryptic name

### Error Handling
- **Prefer exceptions over error codes**: Separate error handling from happy path
- **Never return null**: Use exceptions, empty arrays `[]`, or Null Object pattern
- **Never pass null**: Validate inputs, throw early
- **Provide context**: Include operation attempted, failure type, and relevant values

### Code Organization
- **DRY (Don't Repeat Yourself)**: Extract duplication immediately
- **Single Responsibility Principle**: One reason to change per class/module
- **Dependency Injection**: Don't create dependencies; receive them as parameters
- **Law of Demeter**: Only talk to immediate friends, avoid `a.getB().getC().doD()`

### Comments & Documentation
- **Self-documenting code**: If code needs a comment, rewrite the code
- **Never commit commented-out code**: Use version control instead
- **Acceptable comments**: Legal notices, intent explanation for complex algorithms, warnings

## Quick Reference

| Instead of... | Do this... |
|---------------|------------|
| `const d = 18;` | `const elapsedDays = 18;` |
| `function process(data, flag)` | Split into `processActive()` and `processInactive()` |
| `return null;` | `return [];` or throw specific error |
| `if (x[0] === 4)` | `if (cell.isFlagged())` |
| `// get active users` + complex code | Refactor until comment unnecessary |
| `this.db = new Database()` | `constructor(db) { this.db = db; }` |

## Priority Levels

**MUST** (Always follow):
- Intention-revealing names
- Small functions that do one thing
- No null returns
- DRY principle
- Single Responsibility

**SHOULD** (Follow unless specific reason not to):
- Searchable names (named constants)
- Minimize function arguments
- No flag arguments
- Exceptions over error codes
- Dependency injection

**CONSIDER** (Apply when appropriate):
- Law of Demeter
- Null Object pattern
- Organize for extensibility (Open/Closed Principle)
