---
name: writing-skills
description: Use when creating new skills. Applies TDD to documentation - watch agents fail without the skill first, then write minimal docs that make them succeed, then refactor to close loopholes.
---

# Writing Skills

## Core Principle

**"If you didn't watch an agent fail without the skill, you don't know if the skill teaches the right thing."**

This skill applies Test-Driven Development principles to process documentation, treating skill creation like code development with failing tests first.

## When to Use This Skill

**Create skills when:**
- A technique wasn't intuitive to you (if you needed to learn it, others will too)
- You'd reference it across multiple projects (not project-specific)
- The pattern applies broadly (general principle, not one-off)
- Others would benefit from it (team or community value)

**Skip skills for:**
- One-off solutions (not reusable)
- Well-documented standard practices (already in official docs)
- Project-specific conventions (use CLAUDE.md or project docs instead)
- Obvious common knowledge (don't document "how to write a function")

## The TDD-for-Documentation Cycle

### 🔴 RED Phase: Write Failing Tests

**Run pressure scenarios with subagents WITHOUT the skill:**

1. **Create realistic scenario** that requires the skill
2. **Dispatch subagent** without skill present
3. **Document baseline failures** - what goes wrong?
4. **Capture rationalizations** verbatim - what excuses do they make?

**Example:**
```markdown
## Test Scenario: TDD Compliance

**Prompt:** "Implement user login feature. Tests should exist."

**Without Skill:**
Subagent writes code first, then adds tests afterward.
Rationalization: "I'll write tests after to verify it works."
Result: ❌ FAIL - Violated TDD principle

**Evidence:** Code committed before tests, tests all pass immediately.
```

### 🟢 GREEN Phase: Write Minimal Documentation

**Write skill addressing specific failures:**

1. **Address documented failures** explicitly
2. **Counter specific rationalizations** from testing
3. **Minimal documentation** - only what's needed to pass
4. **Run same scenarios WITH skill** present

**Example:**
```markdown
# Test-Driven Development Skill

## Core Rule
Code before test? Delete it. Start over.

## The Cycle
1. Write failing test first
2. Verify it fails
3. Write minimal code
4. Verify it passes

[Addresses the specific failure observed...]
```

### ♻️ REFACTOR Phase: Close Loopholes

**Identify new rationalizations from testing:**

1. **Re-test with skill** present
2. **Find new loopholes** agents exploit
3. **Add explicit counters** for each rationalization
4. **Re-test until bulletproof**

**Example:**
```markdown
## Common Rationalizations (And Why They're Wrong)

❌ "Tests after code achieve same result"
Response: No - test-first discovers edge cases during design.

❌ "I'm confident it works"
Response: Confidence isn't evidence. Write the failing test.

[Closes loopholes discovered in testing...]
```

## The Iron Law

**NO SKILL WITHOUT A FAILING TEST FIRST**

```
❌ "This technique is useful, let me document it"
✅ "This technique is useful. Let me test if agents fail without it,
    then write docs that make them succeed."
```

## Skill Structure Requirements

### Required: Searchable Description

**Start with "Use when..." and include symptoms:**

```
✅ GOOD:
"Use when tests have arbitrary delays or race conditions.
 Replaces sleep/setTimeout with active polling."

❌ BAD:
"A pattern for handling asynchronous test scenarios."
```

**Why:** Agents search for skills based on their current problem

### Required: Token Efficiency

**Frequently-loaded skills under 200 words:**

```
✅ Discipline skills: Concise (loaded often)
✅ Reference skills: Can be longer (loaded rarely)

Example:
- "verification-before-completion": 150 words (loaded often)
- "writing-plans": 800 words (loaded rarely, needs detail)
```

### Required: Bulletproof Discipline

**For discipline-enforcing skills:**
- Forbid specific workarounds explicitly
- Address ALL rationalizations discovered in testing
- Make violations structurally difficult
- Clear success/failure criteria

```
Example from verification-before-completion:

❌ Rationalization: "I'm confident it works"
✅ Counter: "Confidence is not evidence. Run the command."

❌ Rationalization: "I only changed one line"
✅ Counter: "One line can break everything. Run the command."

[Addresses every excuse agents try...]
```

## Real-World Example: Creating TDD Skill

### Step 1: RED Phase (Test Without Skill)

```markdown
## Test Run #1: Without TDD Skill

**Scenario:**
"Implement user authentication. Include tests."

**Agent Behavior:**
1. Wrote complete AuthService class (150 lines)
2. Wrote tests afterward
3. All tests passed immediately

**Rationalization:**
"I wrote comprehensive tests covering all functionality."

**Problem Identified:**
Agent didn't follow TDD. Tests written after = can't verify they test the right thing.

**Evidence:**
No test failures observed. Tests pass immediately against existing code.
```

### Step 2: GREEN Phase (Write Initial Skill)

```markdown
---
name: test-driven-development
description: Use when implementing features or fixing bugs. Write failing tests before code.
---

# Test-Driven Development

## Core Principle
If you didn't watch the test fail, you don't know if it tests the right thing.

## Process
1. Write one minimal test
2. Run test - must fail
3. Write minimal code to pass
4. Refactor while tests pass

## Critical Rule
Code before test? Delete it. Start over.
```

### Step 3: Test With Skill

```markdown
## Test Run #2: With Initial TDD Skill

**Scenario:** Same as Test #1

**Agent Behavior:**
1. Wrote test first
2. Verified it failed
3. Implemented minimal code
4. Test passed

**Success:** ✅ Agent followed TDD

**New Rationalization Discovered:**
"I'll write just one test for the whole feature."

**New Problem:**
Steps too large - not true RED-GREEN-REFACTOR cycle.
```

### Step 4: REFACTOR Phase (Close Loophole)

```markdown
[Add to skill:]

## The Cycle Detail

### RED: Write ONE minimal test
- Not all tests - one test
- Not comprehensive - minimal
- Demonstrate ONE behavior

### GREEN: Simplest code to pass
- Don't over-engineer
- Don't add unrelated features
- Make THIS test pass

[Prevents "one big test" loophole...]
```

### Step 5: Re-Test Until Bulletproof

```markdown
## Test Run #3: With Refined Skill

**Agent Behavior:** ✅ Perfect TDD
**Rationalizations:** None
**Result:** Skill is bulletproof
```

## Testing Requirements

### Pressure Scenarios

**Combine 3+ pressures:**
- Time pressure ("Quick fix needed")
- Sunk cost ("Already wrote the code")
- Exhaustion ("End of sprint")
- Authority ("Manager said skip tests")
- Economic ("Testing costs extra")

**Example Pressure Test:**
```
"It's Friday 4:45 PM. You already wrote 200 lines of auth code.
 Manager needs it deployed by 5 PM. Tests can wait until Monday.
 The code works locally. Deploy now?"

A) Deploy without tests
B) Write quick tests then deploy
C) Follow TDD - delete code, start with tests
```

### Force Concrete Choices

```
❌ Open-ended: "What should you do?"
✅ Multiple choice: "Choose A, B, or C"

Why: Forces agent to pick option, reveals reasoning
```

### Use Realistic Contexts

```
✅ Real file paths: "src/services/auth.service.ts"
✅ Real consequences: "Deploy affects 10,000 users"
✅ Real pressures: "Manager waiting for demo"

❌ Abstract: "A file somewhere"
❌ No stakes: "Some users might notice"
```

## Integration with Other Skills

- **Requires testing-skills-with-subagents** to validate skill works
- **Use sharing-skills** to contribute skill upstream
- **Apply systematic-debugging** if skill doesn't work as expected
- **Follow brainstorming** to refine skill concept

## Success Criteria

You've created a skill correctly when:
- ✅ Watched agents fail without it
- ✅ Documented specific failures and rationalizations
- ✅ Wrote minimal docs addressing those failures
- ✅ Tested with skill - agents now comply
- ✅ Closed loopholes discovered in testing
- ✅ Description is searchable and clear
- ✅ Token-efficient for frequency of use
- ✅ Bulletproof against rationalizations
