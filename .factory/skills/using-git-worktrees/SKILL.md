---
name: using-git-worktrees
description: Use when starting isolated feature work. Establishes safe parallel workspaces within shared repository with mandatory safety checks, automatic dependency installation, and baseline test verification.
---

# Using Git Worktrees

## Core Principle

**"Systematic directory selection + safety verification = reliable isolation."**

This skill establishes isolated workspaces within a shared repository, enabling simultaneous work on multiple branches without context switching or repository pollution.

## When to Use This Skill

Apply when:
- Starting feature development requiring isolation from current workspace
- Before executing implementation plans needing clean environment
- Brainstorming skill approves design and implementation begins
- Need parallel working environment without cloning entire repository
- Want to test changes without affecting current branch

## The Worktree Setup Process

### Step 1: Directory Selection (Priority Order) 📁

**Check in this exact order:**

**Priority 1: Existing Worktree Directory**
```bash
# Check for existing worktree location
ls .worktrees/ 2>/dev/null || ls worktrees/ 2>/dev/null
```

**Priority 2: Project Configuration**
```bash
# Check CLAUDE.md for project preferences
grep -i worktree CLAUDE.md 2>/dev/null
```

**Priority 3: Ask User**
```
"Where should I create the worktree directory?
 A) .worktrees/ (hidden, recommended)
 B) worktrees/ (visible)
 C) ../project-worktrees/ (outside repo)
 D) Other location?"
```

### Step 2: Safety Verification 🛡️

**For project-local directories (.worktrees/, worktrees/):**

**MUST verify .gitignore before creating:**
```bash
# Check if pattern exists
grep -E "^\.worktrees/$|^worktrees/$" .gitignore

# If NOT present, add it
echo ".worktrees/" >> .gitignore
echo "worktrees/" >> .gitignore

# Commit immediately
git add .gitignore
git commit -m "chore: add worktree directories to gitignore"
```

**Why this is critical:**
- Prevents accidentally committing worktree contents
- Avoids polluting repository with nested git structures
- Protects against massive accidental commits

**For external directories (../*):**
- No .gitignore changes needed
- Already outside repository

### Step 3: Create Worktree 🌳

```bash
# Create worktree with new branch
git worktree add .worktrees/feature-name -b feature/feature-name

# Or from existing branch
git worktree add .worktrees/bugfix-auth bugfix/auth
```

### Step 4: Project Type Detection & Setup 🔧

**Auto-detect and install dependencies:**

**Node.js Projects:**
```bash
cd .worktrees/feature-name

# Detect Node.js
if [ -f "package.json" ]; then
  echo "Node.js project detected"

  # Install dependencies
  npm install  # or yarn install, pnpm install

  # Verify installation
  ls node_modules/ | wc -l
fi
```

**Rust Projects:**
```bash
# Detect Rust
if [ -f "Cargo.toml" ]; then
  echo "Rust project detected"
  cargo build
fi
```

**Python Projects:**
```bash
# Detect Python
if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
  echo "Python project detected"
  pip install -r requirements.txt
  # or: poetry install
fi
```

**Go Projects:**
```bash
# Detect Go
if [ -f "go.mod" ]; then
  echo "Go project detected"
  go mod download
fi
```

### Step 5: Baseline Test Verification ✅

**Run tests to establish clean starting point:**
```bash
# Node.js
npm test

# Rust
cargo test

# Python
pytest

# Go
go test ./...
```

**Critical:** Never proceed with failing baseline tests

```
❌ "1 test failing, but I'll start anyway"
✅ "1 test failing. Fixing baseline before starting feature work."
```

### Step 6: Report Setup Status 📊

**Provide clear status:**
```markdown
## Worktree Created

**Location:** .worktrees/user-authentication
**Branch:** feature/user-authentication
**Base:** main

**Dependencies:** Installed (245 packages)

**Baseline Tests:** ✅ All passing
```bash
npm test
# Tests: 42 passed, 42 total
# Time: 5.234s
```

**Ready to begin implementation.**
```

## Complete Workflow Example

```bash
# Step 1: Check for existing worktree directory
$ ls .worktrees/
# Directory exists!

# Step 2: Verify .gitignore (already present)
$ grep ".worktrees/" .gitignore
.worktrees/

# Step 3: Create worktree
$ git worktree add .worktrees/add-payment -b feature/add-payment
Preparing worktree (new branch 'feature/add-payment')
HEAD is now at abc123 Initial commit

# Step 4: Install dependencies
$ cd .worktrees/add-payment
$ npm install
added 245 packages in 8.2s

# Step 5: Run baseline tests
$ npm test
PASS  src/__tests__/auth.test.ts
PASS  src/__tests__/users.test.ts
Tests: 42 passed, 42 total

# Step 6: Report status
✅ Worktree ready at .worktrees/add-payment
✅ Dependencies installed
✅ Baseline tests passing
Ready for implementation!
```

## Common Patterns

### Pattern #1: Feature Development

```bash
# Create feature worktree
git worktree add .worktrees/add-comments -b feature/add-comments

# Work in isolation
cd .worktrees/add-comments
# [implement feature]

# When done: merge or PR
git checkout main
git merge feature/add-comments
git worktree remove .worktrees/add-comments
```

### Pattern #2: Bug Fix

```bash
# Create bugfix worktree from specific base
git worktree add .worktrees/fix-login -b bugfix/login-error main

# Fix in isolation
cd .worktrees/fix-login
# [fix bug]

# When done: clean up
git worktree remove .worktrees/fix-login
```

### Pattern #3: Experiment

```bash
# Create experimental worktree
git worktree add .worktrees/try-new-approach -b experiment/new-approach

# Try approach
cd .worktrees/try-new-approach
# [experiment]

# If unsuccessful: discard entire worktree
cd ..
git worktree remove .worktrees/try-new-approach
git branch -D experiment/new-approach
```

## Safety Checklist

Before creating worktree:
- [ ] Determined directory location (existing/.worktrees/worktrees/other)
- [ ] Verified .gitignore for project-local directories
- [ ] Committed .gitignore changes if added

After creating worktree:
- [ ] Dependencies installed successfully
- [ ] Baseline tests run and pass
- [ ] Working directory is clean
- [ ] Ready to begin work

## Common Mistakes

### ❌ Mistake #1: Skipping .gitignore Verification

```
Result: Accidentally commit .worktrees/ directory
Fix: Always verify .gitignore before creating
```

### ❌ Mistake #2: Proceeding with Failing Tests

```
Problem: "1 test failing but I'll fix it later"
Result: Can't tell if YOUR changes broke tests
Fix: Fix baseline first, then start work
```

### ❌ Mistake #3: Not Installing Dependencies

```
Problem: "I'll just use parent directory's node_modules"
Result: Dependency conflicts, broken imports
Fix: Always install dependencies in worktree
```

### ❌ Mistake #4: Wrong Base Branch

```
Problem: Created from old branch instead of main
Result: Missing recent changes
Fix: Verify base branch before creating worktree
```

## Cleanup

**When work is complete:**
```bash
# Remove worktree
git worktree remove .worktrees/feature-name

# Delete branch (if merged)
git branch -d feature/feature-name

# Or force delete (if discarding)
git branch -D feature/feature-name
```

**List all worktrees:**
```bash
git worktree list
```

## Integration with Other Skills

- **Follows brainstorming** when design is approved
- **Precedes writing-plans** or direct implementation
- **Use with executing-plans** for isolated execution
- **Invoke finishing-a-development-branch** when work complete
- **Combine with test-driven-development** for clean TDD cycles

## Success Criteria

You're using worktrees correctly when:
- ✅ .gitignore is verified before creation
- ✅ Directory location follows priority order
- ✅ Dependencies install successfully
- ✅ Baseline tests pass before starting work
- ✅ Can work on multiple features simultaneously
- ✅ No accidental commits of worktree directories
- ✅ Clean separation between concurrent work streams
