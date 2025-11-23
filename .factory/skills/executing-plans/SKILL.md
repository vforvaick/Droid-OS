---
name: executing-plans
description: Use when given a complete implementation plan to execute. Processes tasks in batches with review checkpoints, allowing architect feedback between phases rather than after all work completes.
---

# Executing Plans

## Core Principle

**"Execute in batches, pause for feedback, iterate based on direction."**

This skill manages controlled implementation of complete plans through batched task execution with review checkpoints between phases.

## When to Use This Skill

**Deploy when:**
- Partner provides a "complete implementation plan" ready for execution
- Scenarios requiring human oversight at intermediate stages
- Working through detailed task lists systematically
- Need feedback between batches, not after completion

**Don't use when:**
- Plan needs initial review or revision first
- Tasks are independent and suited for parallel agents
- Prefer automated execution without checkpoints (**subagent-driven-development**)

## The Five-Step Framework

### Step 1: Load & Review 📋

**Critically examine the plan before starting:**
- Read complete plan thoroughly
- Identify unclear instructions
- Surface concerns or questions
- Validate prerequisites are met
- Confirm understanding

**Don't proceed until:**
- ✅ Plan makes sense
- ✅ Prerequisites are available
- ✅ No blocking concerns

### Step 2: Execute in Batches ⚡

**Default approach: Process first three tasks**
- Mark progress in task tracker
- Run specified verifications
- Follow TDD cycle per task
- Commit after each completed task

**Batch size guidelines:**
- Default: 3 tasks
- Simple tasks: Up to 5 tasks
- Complex tasks: 1-2 tasks
- Adjust based on feedback

### Step 3: Report Status 📊

**Present work completed:**
- Summarize tasks completed
- Show verification results
- Highlight any issues encountered
- **Pause for feedback** - Don't continue automatically

**Status Report Template:**
```markdown
## Batch [N] Complete

**Tasks Completed:**
- Task 1: [description] ✅
- Task 2: [description] ✅
- Task 3: [description] ✅

**Verification:**
```bash
npm test
# 15 passed, 0 failed
```

**Issues Encountered:**
- [Any blockers or concerns]

**Ready for next batch? Awaiting your direction.**
```

### Step 4: Iterate 🔄

**Apply partner feedback:**
- Address any concerns raised
- Adjust approach based on feedback
- Execute next batch per direction
- Continue checkpoint pattern

**Stop immediately when:**
- Partner requests changes
- Blocker encountered
- Unclear how to proceed

### Step 5: Handoff ✅

**Upon completion, final verification:**
- Run complete test suite
- Verify all requirements met
- Invoke **finishing-a-development-branch** skill
- Present final status for merge/PR decision

## Critical Guardrails

### Guardrail #1: Stop on Blockers

**Never force through obstacles:**
- Missing dependencies → Stop, request clarification
- Failed tests → Stop, report issue
- Unclear instructions → Stop, ask for guidance
- Unexpected behavior → Stop, seek direction

```
❌ "Test failed, but I'll skip it and continue"
✅ "Test failed. Stopping execution to report issue."
```

### Guardrail #2: Follow Plan Precisely

**Don't deviate without approval:**
- Follow steps in exact order
- Don't skip verifications
- Don't add "improvements" not in plan
- Don't refactor beyond what's specified

```
❌ "I'll also refactor this related code while I'm here"
✅ "Following plan exactly as written, task by task"
```

### Guardrail #3: Wait for Feedback

**Pause between batches:**
- Don't continue autonomously
- Present status and wait
- Apply feedback before proceeding
- Confirm direction for next batch

```
❌ "Batch complete, moving to next automatically"
✅ "Batch complete. Awaiting your feedback before continuing."
```

## Workflow Example

```markdown
# Executing: User Authentication Plan

## Batch 1: Tasks 1-3

Executing:
- Task 1: Create auth service test ✅
- Task 2: Implement auth service ✅
- Task 3: Add password hashing ✅

Verification:
```bash
npm test -- auth.service.test.ts
# ✓ should authenticate valid user
# ✓ should reject invalid password
# Tests: 2 passed, 2 total
```

Status: Batch 1 complete. Ready for feedback.

---

[Partner reviews and approves]

## Batch 2: Tasks 4-6

Executing:
- Task 4: Create login API route ✅
- Task 5: Add request validation ✅
- Task 6: Implement error handling ⚠️

Issue encountered:
Validation library (zod) not in package.json. Should I install it?

Stopped execution. Awaiting direction.

---

[Partner: "Yes, install zod and continue"]

Installing zod...
Continuing with Task 6: ✅

Verification:
```bash
npm test
# All tests passing
curl -X POST http://localhost:3000/api/auth/login
# Validation working correctly
```

Status: Batch 2 complete.

---

[Continue pattern until plan complete...]
```

## Batch Size Strategies

### Strategy 1: Small Batches (1-2 tasks)

**Use when:**
- Tasks are complex or risky
- Partner wants tight feedback loop
- Uncertainty about approach
- First time executing this type of plan

### Strategy 2: Medium Batches (3-5 tasks)

**Use when:**
- Tasks are straightforward
- Partner trusts the approach
- Clear instructions with no ambiguity
- Established pattern being followed

### Strategy 3: Adaptive Batching

**Adjust based on:**
- Feedback frequency from partner
- Complexity of remaining tasks
- Number of issues encountered
- Confidence in approach

## Integration with Other Skills

- **Requires writing-plans** to have plan to execute
- **Use test-driven-development** within each task
- **Apply verification-before-completion** at checkpoints
- **Invoke finishing-a-development-branch** upon completion
- **Use systematic-debugging** when tasks fail

## Success Criteria

You're executing plans effectively when:
- ✅ Following plan steps precisely without deviation
- ✅ Stopping immediately on blockers
- ✅ Reporting status clearly at checkpoints
- ✅ Waiting for feedback between batches
- ✅ Tests and verifications run successfully
- ✅ Partner feels informed and in control
- ✅ No surprises at completion - incremental validation throughout
