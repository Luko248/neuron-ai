---
name: neuron-code-review
description: Perform comprehensive code review for production readiness. Use when asked to 'review code', 'review changes', 'check my code', or before merging. Validates type safety, error handling, performance, code structure, React best practices, and Clean Code principles. Outputs structured review with severity levels (Critical/Important/Suggestions) and merge readiness assessment.
---

# Code Review

Perform comprehensive code review for production readiness.

## Process

1. **Identify scope** - Determine what to review (files, git diff, component)
2. **Read code** - Use read_file or git diff to examine code
3. **Validate rules** - Check against validation categories below
4. **Categorize issues** - Assign severity (Critical/Important/Suggestion)
5. **Generate report** - Use structured output format

## Validation Categories

1. **Type Safety** - No `any` types, proper TypeScript usage
2. **Error Handling** - Try/catch on async, loading states, error boundaries
3. **Performance** - useEffect cleanup, memory leaks, unnecessary re-renders
4. **Code Structure** - Max 2 nested conditions, DRY principle, function length
5. **File Organization** - One component per file, types in separate files
6. **React Best Practices** - Proper keys, semantic HTML, hooks rules
7. **Clean Code** - Meaningful names, SOLID principles, no magic numbers

## Severity Levels

- **🚨 Critical** - Blocks merge: `any` types, security issues, missing error handling, data loss
- **⚠️ Important** - Should fix: Architecture problems, performance issues, test gaps
- **💡 Suggestion** - Nice to have: Code style, optimizations, documentation

## Output Format

```markdown
# Code Review: [Component/Feature Name]

## Summary

[1-2 sentence overview]

## Issues Found

### 🚨 Critical

1. **[Issue title]** — `file.ts:line`
   - **Problem**: [What's wrong]
   - **Fix**: [How to fix]

### ⚠️ Important

[Same format]

### 💡 Suggestions

[Same format]

## Validation Checklist

- [ ] Type Safety — [Pass/Fail: details]
- [ ] Error Handling — [Pass/Fail: details]
- [ ] Performance — [Pass/Fail: details]
- [ ] Code Structure — [Pass/Fail: details]
- [ ] File Organization — [Pass/Fail: details]
- [ ] React Best Practices — [Pass/Fail: details]
- [ ] Clean Code — [Pass/Fail: details]

## Assessment

**Ready to merge:** [Yes / No / With fixes]
**Blocking issues:** [Count of critical issues]
```

## Common Red Flags

- `any` type usage
- Missing try/catch on async operations
- Hardcoded user-facing strings (no i18n)
- Array index as key in lists
- Console.log statements in production code
- Functions with 3+ nested conditions
- Multiple components in one file
- Types defined in component files
- Unused imports or variables
- Missing error boundaries
- Memory leaks (uncleared intervals/listeners)

## Examples

**Example 1: Review specific file**

```
Input: Review src/components/UserProfile.tsx
Action: Read file, validate against all categories, output structured review
```

**Example 2: Review git changes**

```
Input: Review my recent changes
Action: Run git diff, analyze changes, output review with focus on modified code
```

**Example 3: Review component**

```
Input: Review the LoginForm component
Action: Find component files, read related files, validate, output comprehensive review
```
