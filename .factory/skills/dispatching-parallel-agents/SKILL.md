---
name: dispatching-parallel-agents
description: Use when facing 3+ independent failures across different subsystems. Enables concurrent investigation by assigning separate agents to independent problem domains simultaneously.
---

# Dispatching Parallel Agents

## Core Principle

**"Dispatch one agent per independent problem domain to work concurrently."**

This technique enables concurrent investigation of multiple unrelated failures, dramatically reducing total investigation time compared to sequential debugging.

## When to Use This Skill

**Deploy parallel agents when:**
- **3+ failures exist** across different test files or subsystems
- **Problems are genuinely independent** - fixing one won't resolve others
- **No shared state** between investigations that would cause interference
- **Full system context isn't required** to understand each problem
- **Time is critical** and sequential debugging would be too slow

**Avoid when:**
- Failures are interdependent or related
- Root cause is shared across failures
- Understanding entire system is necessary
- Exploratory debugging where cause is unknown
- Fewer than 3 independent problems

## The Four-Step Methodology

### Step 1: Identify Independent Domains 🗂️

**Group failures by what's broken:**
- Tool approval system
- Batch completion logic
- Abort functionality
- User authentication
- Payment processing

**Ensure each represents:**
- Separate, solvable problem
- Independent from other failures
- Clear scope and boundaries
- Testable in isolation

```
Example grouping:
- Domain A: agent-tool-approval.test.ts (3 failures)
- Domain B: batch-execution.test.ts (5 failures)
- Domain C: abort-handling.test.ts (2 failures)
```

### Step 2: Create Focused Tasks 📝

**Each agent receives:**

```markdown
**Specific Scope:** One test file or subsystem

**Goal:** Fix all failures in [domain]

**Context:**
- Test file: [exact path]
- Failures: [list specific test names]
- Error messages: [paste error output]
- Related files: [only files needed for this domain]

**Constraints:**
- Do NOT modify: [files outside scope]
- Do NOT refactor: [unrelated code]
- FOCUS on: [specific problem domain]

**Expected Output:**
- All tests in [test file] passing
- Summary of what was broken
- Summary of fixes applied
- No changes outside specified scope
```

**Example Task:**
```markdown
**Agent 1 Task: Fix Tool Approval Tests**

**Scope:** agent-tool-approval.test.ts only

**Goal:** Fix 3 failing tests related to tool approval workflow

**Context:**
```
Error: Expected tool to be approved, but status is 'pending'
  at agent-tool-approval.test.ts:45
  at agent-tool-approval.test.ts:67
  at agent-tool-approval.test.ts:89
```

**Constraints:**
- Do NOT modify batch execution code
- Do NOT refactor agent core
- FOCUS on approval state transitions only

**Expected Output:**
- All 3 tests passing
- Explanation of approval state bug
- Minimal fix applied
```

### Step 3: Dispatch Concurrently 🚀

**Launch all agents simultaneously:**
- Don't wait for Agent 1 to finish before starting Agent 2
- Use parallel execution (Task tool for multiple agents)
- Each agent works independently
- No coordination needed during execution

```
# Parallel dispatch
Agent 1: Fix tool approval tests
Agent 2: Fix batch execution tests
Agent 3: Fix abort handling tests

All dispatched simultaneously →
```

### Step 4: Review & Integrate 🔗

**After all agents complete:**
1. **Examine each summary** - What did each agent fix?
2. **Verify no conflicting edits** - Did agents modify same files?
3. **Run complete test suite** - Do all tests pass together?
4. **Integrate changes** - Merge fixes if no conflicts

**Integration Checklist:**
- [ ] Read all agent summaries
- [ ] Check for file conflicts
- [ ] Run full test suite
- [ ] Verify no regressions
- [ ] Confirm independent fixes work together

## Critical Success Factors

### Factor #1: Specificity Matters

```
❌ Vague: "Fix all tests"
✅ Specific: "Fix agent-tool-abort.test.ts - 2 failures related to cleanup"

Why: Specific tasks prevent agents from wandering or conflicting
```

### Factor #2: Provide Context

```
❌ No context: "Fix this test file"
✅ With context: "Fix approval-tests.ts. Error: State transitions not updating.
               Current code uses syncState(), should use async updateState()."

Why: Context helps agents understand quickly without exploration
```

### Factor #3: Set Clear Constraints

```
❌ No constraints: "Fix the bug"
✅ With constraints: "Fix approval logic. Do NOT refactor unrelated auth code.
                    Do NOT change API contracts."

Why: Prevents scope creep and conflicting refactors
```

### Factor #4: Verify Integration

```
❌ "All agents done, ship it!"
✅ "All agents done. Running full test suite to verify integration..."

Why: Independent fixes might have subtle interactions
```

## Real-World Example

### Scenario: 10 Test Failures Across 3 Domains

**Problem:**
```
Test Failures:
- agent-tool-approval.test.ts: 3 failures
- batch-execution.test.ts: 5 failures
- abort-handling.test.ts: 2 failures
```

**Step 1: Identify Domains**
- Domain A: Tool approval workflow
- Domain B: Batch execution logic
- Domain C: Abort cleanup handling

**Step 2: Create Tasks**

```markdown
**Agent 1: Tool Approval Domain**
Scope: agent-tool-approval.test.ts
Failures:
- "should approve tool when conditions met"
- "should reject tool when unsafe"
- "should timeout approval after 30s"
Context: Approval state machine not transitioning correctly
Constraint: Don't modify batch execution code

**Agent 2: Batch Execution Domain**
Scope: batch-execution.test.ts
Failures:
- "should complete batch when all tasks done"
- "should handle partial batch completion"
- "should retry failed tasks"
- "should report batch progress"
- "should cancel batch on error"
Context: Batch completion detection broken
Constraint: Don't modify approval or abort code

**Agent 3: Abort Handling Domain**
Scope: abort-handling.test.ts
Failures:
- "should cleanup resources on abort"
- "should cancel pending operations on abort"
Context: Cleanup not running after abort
Constraint: Don't modify approval or batch code
```

**Step 3: Dispatch All Three**
```
Dispatch Agent 1, Agent 2, Agent 3 simultaneously
```

**Step 4: Integration**
```
Agent 1 Result: Fixed approval state transitions (3 tests passing)
Agent 2 Result: Fixed batch completion logic (5 tests passing)
Agent 3 Result: Fixed abort cleanup (2 tests passing)

Integration Check:
$ npm test
✓ 45 tests passing
✓ No conflicts detected
✓ All fixes compatible

Success! All 10 failures resolved in parallel.
```

## Common Mistakes

### Mistake #1: Overlapping Scopes

```
❌ Agent 1: "Fix user system"
   Agent 2: "Fix authentication"
   Problem: Auth is part of user system - conflict!

✅ Agent 1: "Fix user profile tests"
   Agent 2: "Fix login flow tests"
   Result: Clear separation, no overlap
```

### Mistake #2: Dependent Failures

```
❌ Agent 1: "Fix database connection"
   Agent 2: "Fix user queries"
   Problem: User queries depend on DB connection!

✅ Fix database connection first (sequential)
   Then parallelize user, order, payment queries
```

### Mistake #3: Insufficient Context

```
❌ "Fix test-file-A.test.ts" (no error messages, no context)
   Result: Agent wastes time exploring

✅ "Fix test-file-A.test.ts. Error: TypeError at line 45.
    Null user object. Need to handle missing user case."
   Result: Agent fixes directly
```

## Integration with Other Skills

- Use **systematic-debugging** for each individual domain
- Apply **verification-before-completion** per agent
- Combine with **root-cause-tracing** for complex failures
- Use **executing-plans** if failures require planned refactors

## Success Criteria

You're using parallel agents effectively when:
- ✅ 3+ truly independent problems identified
- ✅ Each agent has clear, specific scope
- ✅ Agents work without coordination
- ✅ Integration reveals no conflicts
- ✅ Total time significantly less than sequential
- ✅ All problems resolved correctly
- ✅ No duplicate or overlapping work
