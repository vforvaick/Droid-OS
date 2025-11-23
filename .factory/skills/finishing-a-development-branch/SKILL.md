---
name: finishing-a-development-branch
description: Use when implementation is complete and all tests pass. Guides structured verification and presents clear options for merge, PR, cleanup, or discard workflows.
---

# Finishing a Development Branch

## Core Principle

**"Verify first, then present options with clear consequences."**

This skill guides completion of development work through structured verification and decision process, ensuring code quality before integration.

## When to Use This Skill

Deploy when:
- Implementation is complete
- All tests pass
- Need to decide between merge, PR, cleanup, or discard workflows
- Ready to integrate work back to main branch

**Typically called by:**
- Sub agent-driven-development after tasks complete
- Executing-plans after plan execution
- Manual workflows when feature is done

## The Four-Step Process

### Step 1: Verify Tests ✅

**Run complete test suite before proceeding:**

```bash
# Run all tests
npm test  # or pytest, cargo test, go test, etc.
```

**Critical Rule:** If tests fail, STOP immediately

```
❌ Tests failing: "I'll present options anyway"
✅ Tests failing: "Stopping. Cannot proceed until tests pass."
```

**Don't present options until tests pass:**
- No merge option if tests fail
- No PR option if tests fail
- Only option: Fix tests first

### Step 2: Determine Base Branch 🌿

**Identify which branch this feature split from:**

```bash
# Check current branch
git branch --show-current
# feature/user-authentication

# Find base branch (usually main or master)
git show-branch | grep '\*' | grep -v "$(git rev-parse --abbrev-ref HEAD)" | head -n1 | sed 's/.*\[\(.*\)\].*/\1/' | sed 's/[\^~].*//'

# Or check git config
git config --get branch.$(git rev-parse --abbrev-ref HEAD).merge
```

**Common base branches:**
- `main`
- `master`
- `develop`
- `staging`

### Step 3: Present Options 🎯

**Offer exactly four structured choices:**

```markdown
## Development Complete ✅

**Branch:** feature/user-authentication
**Base:** main
**Tests:** ✅ All passing (42 passed, 0 failed)
**Commits:** 12 new commits

## What would you like to do?

**Option A: Merge back to main locally**
- Merges feature branch into main
- Keeps all commit history
- Immediately available in main branch
- Cleans up worktree and deletes feature branch
- Use when: Solo development, changes are ready

**Option B: Push and create a Pull Request**
- Pushes feature branch to remote
- Opens PR for team review
- Keeps worktree and branch for potential changes
- Use when: Team review needed, collaborative project

**Option C: Keep the branch as-is**
- No merge, no PR
- Preserves worktree for continued work
- Branch remains available for future work
- Use when: Not ready to integrate, want to pause

**Option D: Discard this work**
- Removes worktree
- Deletes feature branch
- Work is permanently lost
- Requires typing "discard" to confirm
- Use when: Experiment failed, approach abandoned

Which option would you like? (A/B/C/D)
```

### Step 4: Execute & Cleanup 🔨

**Execute chosen option with appropriate cleanup:**

#### Option A: Merge Locally

```bash
# Switch to base branch
git checkout main

# Merge feature branch
git merge feature/user-authentication

# Verify merge successful
git log --oneline -n 5

# Remove worktree
git worktree remove .worktrees/user-authentication

# Delete feature branch
git branch -d feature/user-authentication

# Confirmation
echo "✅ Merged to main and cleaned up"
```

#### Option B: Push and Create PR

```bash
# Push feature branch
git push -u origin feature/user-authentication

# Create PR using GitHub CLI
gh pr create \
  --title "Add user authentication" \
  --body "$(cat <<'EOF'
## Summary
- Implemented JWT-based authentication
- Added login/logout endpoints
- Secure password hashing with bcrypt

## Test Plan
- [x] All tests passing (42 passed)
- [x] Manual testing complete
- [x] Ready for review
EOF
)"

# Note: Worktree remains for potential PR updates
# Note: Branch remains on remote

echo "✅ PR created - worktree preserved for updates"
```

#### Option C: Keep As-Is

```bash
# No action taken
# Worktree remains at: .worktrees/user-authentication
# Branch remains: feature/user-authentication

echo "✅ Branch preserved - no changes made"
```

#### Option D: Discard Work

```bash
# Require explicit confirmation
read -p "Type 'discard' to confirm: " confirm

if [ "$confirm" = "discard" ]; then
  # Remove worktree
  git worktree remove .worktrees/user-authentication

  # Force delete branch (unmerged)
  git branch -D feature/user-authentication

  echo "✅ Work discarded - branch and worktree removed"
else
  echo "❌ Discard cancelled"
fi
```

## Critical Rules

### Rule #1: Never Skip Test Verification

```
❌ "Tests probably pass, let's merge"
✅ "Running tests now to verify before presenting options"
```

### Rule #2: Always Require Typed "discard" Confirmation

```
❌ "Are you sure? (y/n)"
✅ "Type 'discard' to confirm permanent deletion"
```

### Rule #3: Only Clean Up for Merge and Discard

```
✅ Merge (A): Remove worktree, delete branch
✅ Discard (D): Remove worktree, delete branch
❌ PR (B): Keep worktree, keep branch (for PR updates)
❌ Keep (C): Keep worktree, keep branch (for continued work)
```

### Rule #4: Verify Base Branch Before Merge

```
❌ Merge to wrong branch
✅ Confirm: "Merging to 'main' - is this correct? (y/n)"
```

## Workflow Examples

### Example 1: Successful Feature → Merge

```
$ git worktree list
/project/.worktrees/add-auth  feature/add-auth

[Implementation complete, tests pass]

$ [finishing-a-development-branch invoked]

Running tests...
✅ Tests: 42 passed, 0 failed

Options presented: A/B/C/D
User chooses: A (Merge)

$ git checkout main
$ git merge feature/add-auth
Updating abc123..def456
Fast-forward
 12 files changed, 450 insertions(+), 23 deletions(-)

$ git worktree remove .worktrees/add-auth
$ git branch -d feature/add-auth

✅ Merged to main successfully
```

### Example 2: Feature → PR for Review

```
[Implementation complete, tests pass]

Options presented: A/B/C/D
User chooses: B (PR)

$ git push -u origin feature/add-payment
$ gh pr create --title "Add Stripe payment integration" --body "..."

✅ PR #42 created: https://github.com/user/repo/pull/42
Worktree preserved at .worktrees/add-payment for PR updates
```

### Example 3: Experiment → Discard

```
[Experiment failed, approach didn't work]

Options presented: A/B/C/D
User chooses: D (Discard)

Type 'discard' to confirm: discard

$ git worktree remove .worktrees/experiment
$ git branch -D experiment/new-approach

✅ Experiment discarded - worktree and branch removed
```

## Integration with Other Skills

- **Follows executing-plans** or **subagent-driven-development**
- **Requires verification-before-completion** (tests must pass)
- **May invoke requesting-code-review** before presenting options
- **Precedes merge/PR** workflows
- **Works with using-git-worktrees** (cleans up worktrees)

## Success Criteria

You're finishing branches correctly when:
- ✅ Tests verified before presenting options
- ✅ Four clear options presented with consequences
- ✅ Base branch identified correctly
- ✅ Discard requires explicit confirmation
- ✅ Cleanup appropriate for chosen option
- ✅ Worktrees removed for merge/discard
- ✅ Worktrees preserved for PR/keep
- ✅ User has full control over final decision
