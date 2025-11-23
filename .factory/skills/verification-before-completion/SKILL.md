---
name: verification-before-completion
description: Use before making ANY completion claim. Requires fresh command execution and empirical evidence before asserting success, correctness, or satisfaction with work.
---

# Verification Before Completion

## Core Principle

**"Evidence before claims, always."**

This skill enforces obtaining empirical evidence before making any claims about work status. It prevents false completion assertions by requiring fresh command execution and output verification.

## When to Use This Skill

Apply this skill **before:**
- Making any completion, correctness, or success statements
- Expressing satisfaction with work
- Committing code or creating pull requests
- Transitioning to subsequent tasks
- Delegating work to agents
- Saying "done", "working", "fixed", "passes", or any variant

**The rule applies to:**
- Exact phrases like "all tests pass"
- Paraphrases like "everything looks good"
- Implications like "ready to merge"
- Any communication suggesting work is finished or functioning

## The Core Gate Function

### Five Mandatory Steps

**1. Identify the Command**

Determine the specific command that validates your claim:
- Tests passing? → `npm test` or `pytest` or language-specific test runner
- Build working? → `npm run build` or `cargo build`
- Linter passing? → `npm run lint` or `eslint .`
- Types correct? → `tsc --noEmit` or `mypy`

**2. Execute Fresh Command**

Run the complete, fresh command:
- ❌ NOT: "I ran it 10 minutes ago"
- ❌ NOT: "It passed in my previous attempt"
- ❌ NOT: "It should still be passing"
- ✅ YES: Execute the command RIGHT NOW

**3. Read Full Output**

Examine the complete output:
- Exit codes (0 = success, non-zero = failure)
- Error messages and warnings
- Test counts (passed, failed, skipped)
- Build artifacts created
- Any unexpected output

**4. Verify Output Supports Claim**

Check whether output ACTUALLY supports your claim:
- Claim: "All tests pass" → Output: "12 passed, 0 failed" ✅
- Claim: "All tests pass" → Output: "10 passed, 2 failed" ❌
- Claim: "Build succeeds" → Exit code: 0 ✅
- Claim: "Build succeeds" → Exit code: 1 ❌

**5. Then Claim with Evidence**

Only after verification, make your claim with evidence attached:

```markdown
✅ CORRECT:
All tests pass (verified just now):
```
npm test
12 passed, 0 failed
Execution time: 3.2s
```
```

```markdown
❌ WRONG:
All tests pass!
(No evidence provided)
```

## Common Rationalizations (All Invalid)

### ❌ Rationalization #1: "I'm confident it works"

**Response:** Confidence is not evidence. Run the command.

### ❌ Rationalization #2: "I only changed one line"

**Response:** One line can break everything. Run the command.

### ❌ Rationalization #3: "I checked the main parts"

**Response:** Partial checking isn't verification. Run the complete command.

### ❌ Rationalization #4: "It's late and I'm tired"

**Response:** Tiredness doesn't excuse false claims. Run the command or wait.

### ❌ Rationalization #5: "The tests are flaky anyway"

**Response:** Then fix the tests first. Don't claim success without verification.

### ❌ Rationalization #6: "I tested in dev, it'll work in prod"

**Response:** Different environments can behave differently. Verify in target environment.

## Verification Examples

### Example 1: Test Completion Claim

**❌ WRONG:**
```
"I've fixed the bug. The tests should pass now."
```

**✅ CORRECT:**
```
"I've fixed the bug. Verification:

$ npm test
✓ user authentication works correctly
✓ password validation prevents weak passwords
✓ session management handles expiration

Test Suites: 3 passed, 3 total
Tests:       12 passed, 12 total
Time:        4.532s

All tests verified passing."
```

### Example 2: Build Success Claim

**❌ WRONG:**
```
"Build is working now!"
```

**✅ CORRECT:**
```
"Build is working. Verification:

$ npm run build
Creating optimized production build...
Compiled successfully in 8.2s

$ ls -lh dist/
total 245K
-rw-r--r-- 1 user user  156K bundle.js
-rw-r--r-- 1 user user   45K index.html
-rw-r--r-- 1 user user   32K styles.css

Build artifacts created successfully."
```

### Example 3: Linter Passing Claim

**❌ WRONG:**
```
"Fixed all linting issues."
```

**✅ CORRECT:**
```
"Fixed all linting issues. Verification:

$ npm run lint
✔ 45 files checked
✔ 0 errors
✔ 0 warnings

Linter passes cleanly."
```

## The Damage of False Claims

### Problem #1: Wasted Effort

False completion claims cause:
- Others to build on broken foundations
- Time wasted debugging issues that "shouldn't exist"
- Rework when problems discovered later
- Loss of trust in claimed status

### Problem #2: Cascading Failures

When you claim "tests pass" without verifying:
- Next developer assumes clean state
- They make changes based on false baseline
- Real failures attributed to their changes
- Hours wasted identifying actual culprit

### Problem #3: Broken Trust

Repeated false claims lead to:
- Team members verifying everything you say
- Loss of autonomy and responsibility
- Micromanagement due to lack of trust
- Damaged professional reputation

## Verification Checklists

### For Code Changes
- [ ] All tests run and pass (verified now)
- [ ] Build completes successfully (verified now)
- [ ] Linter passes (verified now)
- [ ] Type checker passes (verified now)
- [ ] No console errors in browser (verified now)
- [ ] Changes work in target environment (verified now)

### For Bug Fixes
- [ ] Created test that reproduces bug
- [ ] Test fails before fix (verified)
- [ ] Test passes after fix (verified)
- [ ] All other tests still pass (verified now)
- [ ] No regressions introduced (verified)

### For Features
- [ ] Feature works as specified (manually tested)
- [ ] All tests pass including new ones (verified now)
- [ ] No breaking changes (verified)
- [ ] Documentation updated (verified complete)
- [ ] Ready for code review (all above verified)

## Integration with Other Skills

- **REQUIRED** for **systematic-debugging** before claiming bug is fixed
- **REQUIRED** for **test-driven-development** before claiming tests pass
- Combine with **requesting-code-review** after verification completes
- Use with **finishing-a-development-branch** before merge decisions

## Success Criteria

You're verifying correctly when:
- ✅ Every completion claim has fresh command output
- ✅ You never say "done" without running verification
- ✅ You catch your own false assumptions before claiming
- ✅ Your claims are trusted because they're always verified
- ✅ You feel uncomfortable making claims without evidence
- ✅ Verification is automatic habit, not forced discipline

## Emergency Override (Almost Never)

The **only** valid reason to skip verification:
- System is completely broken and verification is impossible
- You're explicitly claiming partial progress with caveats
- You state clearly what ISN'T verified yet

**Even then, you must:**
- Be explicit: "NOT VERIFIED - system won't boot"
- State what needs verification later
- Never imply completion without verification
