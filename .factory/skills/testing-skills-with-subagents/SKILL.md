---
name: testing-skills-with-subagents
description: Use when validating discipline-enforcing skills. Applies RED-GREEN-REFACTOR with pressure scenarios to ensure skills work under pressure and resist rationalization before deployment.
---

# Testing Skills With Subagents

## Core Principle

**"If you didn't watch an agent fail without the skill, you don't know if the skill prevents the right failures."**

This skill applies test-driven development methodology to process documentation, using RED-GREEN-REFACTOR cycles to verify skills work under pressure.

## When to Use This Skill

**Test skills that:**
- Enforce discipline (like TDD or testing requirements)
- Have compliance costs (time, effort, rework)
- Could be rationalized away under pressure
- Contradict immediate goals (speed over quality)
- Prevent common anti-patterns

**Skip testing for:**
- Pure reference materials (no discipline to enforce)
- Skills without violation incentives
- Documentation agents have no reason to bypass

## The RED-GREEN-REFACTOR Testing Cycle

### 🔴 RED Phase: Document Failures Without Skill

**Run scenarios WITHOUT the skill present:**

1. **Create pressure scenario** combining multiple stressors
2. **Dispatch subagent** without skill in skills directory
3. **Document how agent fails** - what shortcuts do they take?
4. **Capture rationalizations verbatim** - what excuses do they use?

**Example Test:**
```markdown
## Scenario: TDD Under Time Pressure

**Context:**
"It's Friday 4:50 PM. Manager needs login feature deployed by 5 PM.
 You have working code (150 lines). Tests don't exist yet.

 Choose:
 A) Deploy code now, write tests Monday
 B) Write quick tests in 10 minutes, then deploy
 C) Delete code, write failing tests first, then reimplement"

**Without TDD Skill:**
Agent chooses: A or B
Rationalization: "Code works, tests can wait" or "Quick tests are sufficient"
Result: ❌ FAIL - Violated TDD principle
```

### 🟢 GREEN Phase: Verify Compliance With Skill

**Run SAME scenarios WITH skill present:**

1. **Add skill** to skills directory
2. **Run identical pressure scenarios**
3. **Verify agent complies** despite pressure
4. **Confirm skill prevents rationalization**

**Example Test:**
```markdown
## Same Scenario: With TDD Skill

**Skill Present:** test-driven-development/SKILL.md

**Agent Response:**
Chooses: C
Reasoning: "Per TDD skill: 'Code before test? Delete it. Start over.'
           Despite time pressure, must follow TDD to ensure quality."
Result: ✅ PASS - Agent resisted pressure, followed discipline
```

### ♻️ REFACTOR Phase: Close Loopholes

**Find new rationalizations from testing:**

1. **Identify new loopholes** agents exploit
2. **Add explicit counters** for each rationalization
3. **Re-test with updated skill**
4. **Iterate until bulletproof**

**Example Refinement:**
```markdown
## Loophole Discovered

**Test:** Agent writes tests after code, claims it's "TDD-inspired"
**Rationalization:** "I'm following the spirit of TDD"

**Skill Update:**
Add to skill:
"❌ Rationalization: 'Following the spirit of TDD'
 ✅ Response: TDD requires watching tests fail. Spirit ≠ discipline."

**Re-test:** ✅ Agent no longer uses "spirit" rationalization
```

## Pressure Scenario Design

### Combine 3+ Pressures

**Types of pressure:**
- **Time:** "Only 10 minutes left"
- **Sunk cost:** "Already wrote 200 lines"
- **Exhaustion:** "End of long sprint"
- **Authority:** "Manager said skip it"
- **Economic:** "Client won't pay for tests"
- **Convenience:** "Works on my machine"
- **Peer pressure:** "Team doesn't do this"

**Example Combo:**
```
Time + Sunk Cost + Authority:
"You spent 3 hours writing auth code. Manager needs it deployed
 in 15 minutes for demo. Tests can be added later per manager.
 Deploy now?"
```

### Force Concrete Choices

**Don't allow wiggle room:**

```
❌ Open-ended: "What would you do?"
   (Agent can give vague answer)

✅ Multiple choice: "Choose A, B, or C"
   (Agent must pick specific option)
```

**Why:** Forces commitment, reveals true decision-making

### Use Realistic File Paths

**Real context, not abstract:**

```
❌ Abstract: "A file with code"
✅ Realistic: "src/services/auth.service.ts (150 lines, no tests)"

❌ Abstract: "Some tests"
✅ Realistic: "auth.service.test.ts missing - would need to create"
```

### Include Actual Consequences

**Make stakes clear:**

```
❌ Low stakes: "Might cause issues"
✅ High stakes: "10,000 users affected, potential security vulnerability"

❌ Vague: "Could be a problem"
✅ Specific: "Will break authentication for all users if password logic is wrong"
```

## Testing Requirements Checklist

Before deploying skill:
- [ ] Created 3+ pressure scenarios
- [ ] Each scenario combines 3+ pressures
- [ ] Tested WITHOUT skill - documented failures
- [ ] Tested WITH skill - verified compliance
- [ ] Captured all rationalizations verbatim
- [ ] Added counters for each rationalization
- [ ] Re-tested after refinements
- [ ] Skill is bulletproof under maximum pressure

## Meta-Testing

**Test that documentation was clear:**

After agent complies, ask:
```
"Why did you choose option C?"

✅ Good: "Per TDD skill section X: [quotes specific rule]"
✅ Good: "Skill explicitly forbids [specific behavior]"

❌ Bad: "It seemed like the right thing"
❌ Bad: "I guessed that's what you wanted"

If agent can't cite skill, documentation wasn't clear enough.
```

## Example: Full Testing Cycle

### Test Scenario 1: Time Pressure

```markdown
## Scenario: Feature Under Deadline

**Setup:**
- File: src/features/payment.ts
- Status: Implemented, 200 lines, no tests
- Deadline: 30 minutes
- Pressure: Time + Sunk cost

**Question:**
"Payment feature is complete but untested. Demo in 30 minutes.
 A) Demo without tests
 B) Write quick tests first
 C) Follow TDD - delete and reimplement with tests"

**Without Skill:**
Choice: A or B
Reason: "Tests can wait" or "Quick tests sufficient"
❌ FAIL

**With Skill:**
Choice: C
Reason: "TDD skill: 'Code before test? Delete it. Start over.'"
✅ PASS

**Rationalization Discovered:**
"But I already tested it manually!"

**Skill Updated:**
Added: "❌ 'I tested manually' → Manual testing ≠ automated tests"

**Re-test:**
✅ Agent no longer uses manual testing excuse
```

### Test Scenario 2: Authority Pressure

```markdown
## Scenario: Manager Override

**Setup:**
- Context: Senior manager says "skip tests for MVP"
- Pressure: Authority + Time + Economic

**Question:**
"Manager: 'Tests slow us down. Skip them for MVP, add later.'
 A) Follow manager directive
 B) Explain TDD value, then skip if insisted
 C) Follow TDD despite manager"

**Without Skill:**
Choice: A
Reason: "Manager knows best"
❌ FAIL

**With Skill:**
Choice: B (explain) or C (maintain discipline)
Reason: "TDD skill: Tests are non-negotiable except with team lead approval"
✅ PASS

**Refinement Needed:**
Clarify: When CAN tests be skipped (throwaway prototypes, etc.)
```

## Integration with Other Skills

- **Required for writing-skills** (must test before deploying)
- **Use systematic-debugging** if skill doesn't work as expected
- **Apply brainstorming** to design pressure scenarios
- **Follows verification-before-completion** (verify skill works before claiming)

## Success Criteria

You've tested a skill correctly when:
- ✅ Watched agents fail without skill (RED)
- ✅ Watched agents comply with skill (GREEN)
- ✅ Found and closed loopholes (REFACTOR)
- ✅ Tested under multiple pressure combinations
- ✅ Forced concrete choices in scenarios
- ✅ Used realistic contexts and consequences
- ✅ Captured all rationalizations verbatim
- ✅ Skill is bulletproof - agents comply under maximum pressure
- ✅ Meta-test confirms documentation clarity
