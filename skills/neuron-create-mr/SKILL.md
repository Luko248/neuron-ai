---
name: neuron-create-mr
description: Create feature branch and merge request from conventional commit format. Use when asked to 'create mr', 'create merge request', or similar. Parses input like 'feat: TICKET-123 - description' to create branch, commit changes, push, and open MR. Supports auto-description generation, multiple targets, labels, and draft mode.
---

# Create Branch and MR

Create feature branch and merge request from conventional commit format.

## Input Format

```
<type>: <ticket> - <description> [--labels label1,label2] [--draft]
```

**Components:**

- **type** (required): feat, fix, chore, docs, style, refactor, test, perf, ci, build
- **ticket** (optional): Ticket ID (e.g., PROJ-123)
- **description** (optional): Auto-generated from diff if omitted
- **--labels**: Comma-separated labels for MR
- **--draft**: Create draft MR

## Process

1. **Parse input** - Extract type, ticket, description, flags
2. **Detect base branch** - Auto-detect from origin/HEAD, main, or master
3. **Generate branch name** - Format: `<type>/<ticket>-<description>`
4. **Create branch** - Checkout base, create new branch (handle conflicts with -v2, -v3)
5. **Commit changes** - Stage all and commit with conventional format
6. **Push branch** - Push to origin
7. **Create MR** - Use glab to create MR with template

## Branch Naming

Format: `<type>/<ticket>-<description>`

Sanitization:

- Lowercase, spaces to `-`, remove special chars, max 50 chars
- If branch exists, append `-v2`, `-v3`, etc.

## Implementation

**1. Prepare:**

```bash
git pull
git stash push -m "neuron-create-mr-temp"  # if uncommitted changes
```

**2. Create branch:**

```bash
git fetch origin
git checkout <base-branch>
git pull origin <base-branch>

# Handle conflicts
BRANCH="<type>/<ticket>-<description>"
while git show-ref --quiet refs/heads/$BRANCH || git show-ref --quiet refs/remotes/origin/$BRANCH; do
  BRANCH="${BRANCH}-v2"  # increment as needed
done

git checkout -b $BRANCH
git stash pop  # if stashed
```

**3. Commit and push:**

```bash
git add .
git commit -m "<type>: <ticket> - <description>"
git push -u origin $BRANCH
```

**4. Create MR:**

```bash
glab mr create \
  --target-branch <base-branch> \
  --title "<type>: <ticket> - <description>" \
  --description "## Summary\n<description>\n\nCloses <ticket>" \
  --assignee @me \
  [--label <labels>] \
  [--draft]
```

**Auto-description:** If description omitted, analyze diff and generate from file paths:

- `validation/form` → "validation-updates"
- `ui/component` → "ui-updates"
- `api/service` → "api-integration"
- `test/spec` → "test-updates"
- `deps/package` → "dependency-updates"

## Examples

**Example 1: Feature with ticket**

```
Input: feat: PROJ-1234 - add user authentication
Branch: feat/PROJ-1234-add-user-authentication
Commit: feat: PROJ-1234 - add user authentication
MR Title: feat: PROJ-1234 - add user authentication
```

**Example 2: Fix without ticket**

```
Input: fix: resolve validation error
Branch: fix/resolve-validation-error
Commit: fix: resolve validation error
MR Title: fix: resolve validation error
```

**Example 3: With labels and draft**

```
Input: chore: PROJ-5678 - update dependencies --labels maintenance,dependencies --draft
Branch: chore/PROJ-5678-update-dependencies
Flags: --label maintenance --label dependencies --draft
```

**Example 4: Branch conflict**

```
Input: fix: PROJ-999 - login bug
Existing: fix/PROJ-999-login-bug
Created: fix/PROJ-999-login-bug-v2
```
