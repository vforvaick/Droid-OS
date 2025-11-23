---
name: systematic-debugging
description: Use for any bug, test failure, or unexpected behavior. Four-phase methodology ensures root cause fixes rather than symptom patches, especially critical when stuck >10min or after 2+ failed attempts.
---

# Systematic Debugging

## Core Principle

**"ALWAYS find root cause before attempting fixes. Symptom fixes are failure."**

This framework ensures bugs are fixed at their root cause rather than through symptom-masking patches. It prevents wasted effort and the creation of secondary bugs.

## When to Use This Skill

Apply this methodology to **any technical issue:**
- Test failures or production bugs
- Unexpected system behavior
- Performance degradation
- Build or integration failures

**Especially critical when:**
- Time pressure creates temptation to guess
- "Quick fixes" seem obvious
- Multiple previous fix attempts have failed
- You lack full understanding of the problem
- **You've been stuck for more than 10 minutes**
- **You've attempted 2 or more fixes without success**

## The Four-Phase Methodology

### Phase 1: Root Cause Investigation 🔍

**Goal:** Understand WHAT is breaking and WHERE

**Actions:**
1. **Read error messages carefully** - Every word matters
2. **Examine stack traces** - Trace execution path
3. **Reproduce consistently** - Can you make it fail reliably?
4. **Review recent changes** - What changed before this broke?
5. **Trace data flow** - Follow data through multi-component systems
6. **Gather diagnostic evidence** - Logs, screenshots, network traces

**Stop when:** You can clearly articulate what's breaking and where it fails

**Critical Rule:** Gather evidence before forming theories

### Phase 2: Pattern Analysis 🔬

**Goal:** Understand WHY it's breaking

**Actions:**
1. **Find working similar code** - Locate comparable working implementations
2. **Compare working vs broken** - What's different?
3. **Identify ALL differences** - No matter how minor
4. **Understand dependencies** - What does this code rely on?
5. **Check assumptions** - Are they still valid?
6. **Map the pattern** - How should this work?

**Stop when:** You understand the pattern and why it's violated

**Critical Rule:** If you don't understand why it works elsewhere, keep investigating

### Phase 3: Hypothesis and Testing 🧪

**Goal:** Prove what needs to change

**Actions:**
1. **Form specific hypothesis** - "It fails because X is Y instead of Z"
2. **Test with minimal changes** - Change one variable at a time
3. **Verify results** - Did the test prove or disprove the hypothesis?
4. **Accept uncertainty** - Don't guess if you don't understand
5. **Iterate if wrong** - Return to Phase 1 or 2 if hypothesis fails

**Stop when:** You have a verified hypothesis about root cause

**Critical Rule:** Never proceed with fixes based on unverified guesses

### Phase 4: Implementation 🛠️

**Goal:** Fix the root cause permanently

**Actions:**
1. **Create failing test case first** - Prove the bug exists
2. **Implement the single root-cause fix** - Not multiple changes
3. **Verify fix resolves the issue** - Test passes now
4. **Check for regressions** - Did you break anything else?
5. **Document the fix** - Why was this the root cause?

**Stop when:** Tests pass, no regressions, fix is understood

**Critical Rule:** If 3+ fixes have failed, question whether the architecture itself is flawed

## Critical Red Flags 🚩

**Stop and restart Phase 1 if you catch yourself:**
- ❌ Proposing fixes without completing investigation
- ❌ Making multiple simultaneous changes
- ❌ Attempting a fourth fix without architectural discussion
- ❌ Saying "let's just try this and see"
- ❌ Bypassing failing tests instead of fixing them
- ❌ Adding workarounds instead of fixing causes

## Workflow Example

```
BUG: User login fails with 401 error

Phase 1 - Investigation:
- Error: "Invalid credentials" from /api/auth/login
- Stack trace shows UserService.validatePassword()
- Reproduced: Happens with correct password
- Recent change: Password hashing algorithm updated
- Evidence: Database has bcrypt hashes, code expects argon2

Phase 2 - Pattern Analysis:
- Working code: Registration uses bcrypt
- Broken code: Login expects argon2
- Difference: Mismatch between stored and expected format
- Dependency: Password library version changed
- Root cause: Migration script not run after library update

Phase 3 - Hypothesis:
- Hypothesis: "Login fails because passwords are bcrypt but code expects argon2"
- Test: Query database, confirm bcrypt format
- Test: Check if migration script exists
- Verified: Migration script exists but wasn't run

Phase 4 - Implementation:
- Test: Add test for bcrypt password validation
- Fix: Run migration script to convert to argon2
- OR: Revert code to support bcrypt (if migration unsafe)
- Verify: Login works, no regressions
- Document: "Must run migration after password library updates"
```

## Common Failure Patterns

### Failure Pattern #1: Guessing

```
❌ "Let's try adding a try-catch and see if that fixes it"
✅ "Let's trace where this exception originates"
```

### Failure Pattern #2: Multiple Changes

```
❌ "Let's refactor this whole module and fix the bug"
✅ "Let's change only what's necessary to fix the root cause"
```

### Failure Pattern #3: Symptom Masking

```
❌ "Let's return a default value when this fails"
✅ "Let's fix why it fails in the first place"
```

### Failure Pattern #4: Ignoring Architecture

```
❌ "Third fix failed, let's try a fourth approach"
✅ "Third fix failed, let's discuss if the architecture is wrong"
```

## Integration with Other Skills

- Use **root-cause-tracing** when errors occur deep in call stacks
- Apply **verification-before-completion** before claiming bug is fixed
- Combine with **test-driven-development** to create tests proving the fix
- Use **defense-in-depth** to prevent similar bugs in future

## Success Criteria

You're debugging systematically when:
- ✅ You can explain the root cause clearly
- ✅ Your fix addresses the cause, not symptoms
- ✅ You have a test that proves the bug is fixed
- ✅ You didn't make multiple simultaneous changes
- ✅ You understand why the fix works
- ✅ Similar bugs won't happen due to your fix

## When to Escalate

Consider architectural discussion if:
- Three or more fix attempts have failed
- Root cause investigation reveals systemic design issues
- The "fix" requires extensive workarounds
- Multiple components have similar issues
- The bug indicates a fundamental assumption is wrong
