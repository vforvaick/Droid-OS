---
name: subagent-driven-development
description: Executes implementation plans by dispatching fresh subagent for each task with code review gates between tasks. Maintains continuous momentum within same session through automated quality checkpoints.
---

# Subagent-Driven Development

## Core Principle

**"Fresh subagent per task + review between tasks = high quality, fast iteration."**

This skill executes implementation plans by dispatching a fresh subagent for each task, with code review gates between tasks, maintaining continuous momentum through automated quality checkpoints.

## When to Use This Skill

**Ideal for scenarios where you're:**
- Staying in the same session (no context switching)
- Working with mostly independent tasks
- Wanting rapid iteration with built-in quality assurance
- Executing a complete implementation plan
- Managing multiple tasks systematically

**Avoid when:**
- Plan needs initial review or revision
- Tasks are tightly interdependent
- Partner prefers manual checkpoint approval (use **executing-plans**)
- Only 1-2 simple tasks (overhead not worth it)

## The Seven-Step Process

### Step 1: Load the Plan 📋

**Create task tracker from implementation plan:**

```markdown
## Implementation: User Authentication

**Plan:** docs/plans/2025-01-15-user-authentication.md

**Tasks:**
1. [ ] Create auth service test
2. [ ] Implement auth service
3. [ ] Add password hashing
4. [ ] Create login API route
5. [ ] Add request validation
6. [ ] Implement error handling
7. [ ] Add session management
8. [ ] Create logout endpoint
9. [ ] Add authentication middleware
10. [ ] Write integration tests

**Status:** 0/10 complete
```

### Step 2: Dispatch Implementation Subagent 🚀

**For first pending task, dispatch focused subagent:**

```markdown
**Task:** Implement Task 1 from user authentication plan

**Requirements:**
- Read: docs/plans/2025-01-15-user-authentication.md (Task 1 only)
- Follow instructions exactly
- Run specified verifications
- Commit as instructed
- Report completion

**Scope:** Task 1 only - do not proceed to Task 2

**Expected Output:**
- Task 1 implementation complete
- Verifications passing
- Changes committed
- Ready for review
```

**Key principle:** Fresh context per task (no confusion from previous tasks)

### Step 3: Code Review 🔍

**When task completes, dispatch code reviewer:**

```markdown
**Review Request**

**Changes:** [git SHA range from task completion]

**Requirement:** Task 1 - Create auth service test

**Implementation:** [subagent summary]

**Review Focus:**
- Does test follow TDD principles?
- Is test actually failing (RED phase)?
- Clear test expectations?
- Proper test structure?

**Expected Output:**
- Critical issues (must fix)
- Important issues (should fix)
- Minor suggestions (optional)
```

### Step 4: Address Feedback ⚡

**Triage review findings:**

**Critical Issues → Fix Immediately**
```markdown
Reviewer: "Test doesn't verify password hashing"
Action: Dispatch fix subagent to address before continuing
```

**Important Issues → Fix Before Proceeding**
```markdown
Reviewer: "Missing edge case for empty password"
Action: Add test case before moving to next task
```

**Minor Issues → Document for Later**
```markdown
Reviewer: "Consider extracting test helper"
Action: Add to backlog, proceed with next task
```

### Step 5: Mark Complete & Iterate ♻️

**Update task tracker:**

```markdown
## Implementation: User Authentication

**Tasks:**
1. [x] Create auth service test ✅
2. [ ] Implement auth service ← NEXT
3. [ ] Add password hashing
...

**Status:** 1/10 complete

**Next:** Dispatching Task 2 implementation...
```

**Repeat Steps 2-5 for each remaining task.**

### Step 6: Final Review 🎯

**After all tasks complete:**

```markdown
**Final Review Request**

**Changes:** [git SHA range for entire feature]

**Requirements:** Complete user authentication system
- All 10 tasks from plan completed
- All individual reviews addressed

**Holistic Review:**
- Integration between components
- Overall code quality
- Security considerations
- Performance implications
- Documentation completeness

**Expected Output:**
- Final approval or issues to address
```

### Step 7: Complete Development 🏁

**Invoke finishing-a-development-branch skill:**

```markdown
All tasks complete ✅
All reviews addressed ✅
Final review approved ✅

Invoking finishing-a-development-branch to present options...
```

## The Workflow Pattern

```
Load Plan
  ↓
Task 1: Dispatch Implementation → Review → Fix Issues → Mark Complete
  ↓
Task 2: Dispatch Implementation → Review → Fix Issues → Mark Complete
  ↓
Task 3: Dispatch Implementation → Review → Fix Issues → Mark Complete
  ↓
...
  ↓
Final Review → Address Issues
  ↓
Finish Branch → Merge/PR/Keep/Discard
```

## Key Characteristics

### Fresh Context Per Task

**Why this matters:**
- No confusion from previous tasks
- Clean mental model
- Focused execution
- Reduced errors from mixed context

```
Task 1 Subagent: Only sees Task 1 requirements
Task 2 Subagent: Only sees Task 2 requirements (fresh start)
Task 3 Subagent: Only sees Task 3 requirements (fresh start)
```

### Automated Quality Gates

**Review after EVERY task:**
- Catches issues early
- Prevents compounding problems
- Maintains quality throughout
- Builds confidence incrementally

```
✅ Task → Review → Fix → Complete
✅ Task → Review → Fix → Complete
✅ Task → Review → Fix → Complete

vs.

❌ Task → Task → Task → Review (find 20 issues at end)
```

### Continuous Momentum

**No waiting for human approval:**
- Same session throughout
- Automated checkpoints
- Rapid iteration
- Efficient execution

## Critical Rules

### Rule #1: One Task at a Time

```
❌ "Implement tasks 1-3 together"
✅ "Implement task 1 only, then review"
```

### Rule #2: Fresh Subagent Per Task

```
❌ "Continue with task 2" (same agent)
✅ "Dispatch new subagent for task 2" (fresh agent)
```

### Rule #3: Review Between Tasks

```
❌ Skip review to move faster
✅ Review every task - saves time by catching issues early
```

### Rule #4: Address Critical/Important Before Proceeding

```
❌ "Critical issue found, but I'll continue anyway"
✅ "Critical issue found, fixing before next task"
```

## Integration with Other Skills

- **Requires writing-plans** to have plan to execute
- **Uses requesting-code-review** between each task
- **Applies receiving-code-review** to process feedback
- **Invokes finishing-a-development-branch** at completion
- **Leverages test-driven-development** within each task

## Success Criteria

You're doing subagent-driven development correctly when:
- ✅ Each task executed by fresh subagent
- ✅ Code review after every task completion
- ✅ Critical/important issues fixed before proceeding
- ✅ Task tracker updated in real-time
- ✅ No tasks skipped or batched
- ✅ Final review conducted for holistic check
- ✅ High quality maintained throughout
- ✅ Continuous momentum without manual gates
