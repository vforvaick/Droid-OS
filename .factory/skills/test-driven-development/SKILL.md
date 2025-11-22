---
name: test-driven-development
description: Use when implementing new features, fixing bugs, or refactoring code. Enforces Red-Green-Refactor methodology to ensure tests genuinely verify behavior.
---

# Test-Driven Development (TDD)

## Core Principle

**"If you didn't watch the test fail, you don't know if it tests the right thing."**

TDD ensures code quality by requiring tests to fail before implementation exists. This validates that tests genuinely verify behavior rather than just passing against existing code.

## When to Use This Skill

**Apply TDD to:**
- New features
- Bug fixes
- Refactoring work
- Behavior changes

**Exceptions (requiring team lead approval):**
- Throwaway prototypes
- Auto-generated code
- Configuration files

## The Red-Green-Refactor Cycle

### 🔴 RED Phase: Write Failing Test

1. Write **one minimal test** demonstrating desired behavior
2. The test **must initially fail** because the feature doesn't exist yet
3. Run the test and **verify it fails** with expected error message
4. **Never skip this step** - watching it fail proves the test works

### 🟢 GREEN Phase: Make It Pass

1. Write the **simplest code possible** to make the test pass
2. Avoid over-engineering or adding unrelated features
3. Focus only on making this one test pass
4. Run tests to verify they now pass

### ♻️ REFACTOR Phase: Improve Code

1. Improve code clarity while keeping tests passing
2. Remove duplication
3. Enhance naming and structure
4. Extract helpers if needed
5. Keep running tests to ensure nothing breaks

## Critical Rules

### Absolute Requirements

- ❌ **Code before test? Delete it. Start over.**
- ✅ **Always verify tests fail before coding** (mandatory step)
- ❌ **Never keep unverified code as "reference"**
- ✅ **Tests written after implementation pass immediately, proving nothing**

### Common Mistake to Avoid

Developers often rationalize skipping TDD with claims that tests added afterward achieve identical goals.

**This is wrong because:**
- Test-first discovers edge cases during design
- Test-after merely verifies remembered scenarios
- Test-first typically yields more comprehensive coverage
- You can't prove a test works if you never saw it fail

## Workflow Example

```
1. Write test for login() function
2. Run test → ❌ Fails (function doesn't exist)
3. Write minimal login() implementation
4. Run test → ✅ Passes
5. Refactor login() for clarity
6. Run test → ✅ Still passes
7. Commit

Repeat for next feature...
```

## Integration with Other Skills

- Combine with **systematic-debugging** when tests reveal unexpected failures
- Use **verification-before-completion** before claiming TDD compliance
- Apply **testing-anti-patterns** to avoid common pitfalls in test code

## Success Criteria

You're doing TDD correctly when:
- ✅ Every test failed at least once before passing
- ✅ You can explain what each test failure proved
- ✅ Code changes only in response to failing tests
- ✅ Refactoring happens with passing tests as safety net
- ✅ No production code exists without corresponding tests
