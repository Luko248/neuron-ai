---
name: neuron-commit-message
description: Generate conventional commit messages in format 'type: ticket - description'. Use when creating commits via Windsurf's commit generate button or when asked to create/generate commit messages. Extracts ticket IDs from branch names and determines commit type from changes.
---

# Commit Message Generation

## Format

With ticket:

```
<type>: <ticket> - <description>
```

Without ticket:

```
<type>: <description>
```

**Output:** Plain text only - no code fences, no markdown formatting

## Process

1. Get branch name: `git branch --show-current`
2. Extract ticket from branch (pattern: `[A-Z]+-\d+` or `[A-Z]{2,}-\d+`)
3. Determine type using priority below
4. Write imperative description (~50 chars, lowercase, no period)
5. Output: `<type>: <ticket> - <description>` or `<type>: <description>`

## Type Selection

1. **feat** - New functionality or UI changes
2. **fix** - Bug fixes or regressions
3. **docs** - Documentation only
4. **chore** - Tooling, config, dependencies, maintenance
5. **refactor** - Code restructuring without behavior change
6. **test** - Test additions or modifications
7. **perf** - Performance improvements
8. **style** - Code style/formatting
9. **ci** - CI/CD configuration
10. **build** - Build system changes

## Examples

**Feature with ticket:**

```
Branch: feat/PROJ-1234-user-auth
Changes: Added login component
Output: feat: PROJ-1234 - add user authentication
```

**Bug fix without ticket:**

```
Branch: fix/validation-error
Changes: Fixed form validation
Output: fix: resolve form validation error
```

**Chore:**

```
Branch: chore/deps-update
Changes: Updated dependencies
Output: chore: update npm dependencies
```

**Common mistakes:**

- ❌ `fix: Fixed bug` (past tense) → ✅ `fix: resolve bug`
- ❌ `feat: Add feature` (capitalized) → ✅ `feat: add feature`
- ❌ `chore: update.` (period) → ✅ `chore: update`
- ❌ `feat: stuff` (vague) → ✅ `feat: add user authentication`
