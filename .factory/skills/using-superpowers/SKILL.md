---
name: using-superpowers
description: Use at start of ANY task. Establishes mandatory protocol to check available skills, read current versions, and execute workflows before taking action. Prevents skipping proven methodologies.
---

# Using Superpowers

## Core Principle

**"If a skill for your task exists, you must use it or you will fail."**

This skill establishes a mandatory protocol for identifying and executing relevant skills before taking any action on tasks, preventing rationalization that leads to skipping proven workflows.

## When to Use This Skill

Apply at the start of **ANY conversation or task**, regardless of perceived complexity or simplicity.

**This applies universally:**
- New feature implementation
- Bug fixes
- Code refactoring
- Planning and design
- Testing and debugging
- Code review
- Documentation
- **Even tasks that seem straightforward**

## The Non-Negotiable Protocol

### Step 1: Inventory Available Skills 📚

**Mentally review skills library:**
- Testing skills (TDD, condition-based-waiting, anti-patterns)
- Debugging skills (systematic-debugging, root-cause-tracing, verification, defense-in-depth)
- Collaboration skills (brainstorming, planning, executing, reviewing, git workflows)
- Meta skills (writing-skills, sharing-skills, testing-skills)

### Step 2: Assess Task Relevance 🎯

**Match task to skill categories:**
```
Task: "Implement user login"
→ Relevant: test-driven-development (new feature)
→ Relevant: verification-before-completion (before claiming done)

Task: "Fix authentication bug"
→ Relevant: systematic-debugging (bug investigation)
→ Relevant: root-cause-tracing (if error is deep in stack)
→ Relevant: verification-before-completion (before claiming fixed)

Task: "Design new API"
→ Relevant: brainstorming (rough idea → clear design)
→ Relevant: writing-plans (design → implementation plan)
```

### Step 3: Read Current Version 📖

**Use Skill tool to read the current version:**
```
Don't rely on memory or assumptions
Always read the CURRENT version
Skill may have been updated since last use
```

### Step 4: Announce Skill Usage 📢

**Explicitly state which skill you're using:**
```
✅ "Using test-driven-development skill for this feature implementation"
✅ "Applying systematic-debugging skill to investigate this failure"
✅ "Invoking brainstorming skill to refine requirements"

❌ [Silently applies skill]
❌ [Doesn't mention skill at all]
```

**Why announce:**
- Transparency to user
- Confirms skill is active
- Accountability for following it

### Step 5: Execute Skill Exactly 🎬

**Follow the skill's instructions precisely:**
- Don't skip steps
- Don't modify process
- Don't rationalize shortcuts
- Complete all requirements

## Critical Anti-Rationalization Rules

### ❌ Rationalization #1: "Task is too simple"

**Response:** Simplicity doesn't exempt from discipline

```
Example:
Task: "Fix typo in comment"
Still use: verification-before-completion (before claiming done)
```

### ❌ Rationalization #2: "I know what to do"

**Response:** Knowing ≠ Following proven workflow

```
Example:
"I know how to debug" → Still use systematic-debugging skill
"I know TDD" → Still follow test-driven-development skill
```

### ❌ Rationalization #3: "Skill would slow me down"

**Response:** Skills save time by preventing mistakes

```
Reality:
- Skipping TDD → Write buggy code → Spend hours debugging
- Using TDD → Write correct code first time → Ship faster
```

### ❌ Rationalization #4: "I'll apply the spirit of the skill"

**Response:** "Spirit" means you're not following the skill

```
Example:
"I'll follow the spirit of TDD" = Writing tests after code = Not TDD
"Spirit" is code word for "skipping the discipline"
```

### ❌ Rationalization #5: "Just this once"

**Response:** Every violation starts with "just this once"

```
Reality:
- "Just this once" becomes habit
- Exceptions become the rule
- Discipline erodes completely
```

## Checklist Enforcement

**When skills contain checklists:**

```
❌ "I'll track these mentally"
✅ Create TodoWrite todos for each checklist item

Why: Mental tracking = items get forgotten
     Written todos = accountability
```

**Example:**
```markdown
Skill checklist:
- [ ] Write failing test
- [ ] Verify test fails
- [ ] Implement minimal code
- [ ] Verify test passes
- [ ] Refactor
- [ ] Commit

→ Create 6 TodoWrite items, mark each as completed when done
```

## Why This Matters

### Skills Document Proven Techniques

**Not arbitrary rules - battle-tested patterns:**
- Developed through experience
- Refined through testing with subagents
- Validated to prevent common failures
- Designed to save time and prevent mistakes

### Skipping Skills Repeats Solved Problems

```
Without Skills:
→ Make known mistakes
→ Fall into common traps
→ Waste time solving solved problems
→ Lower quality results

With Skills:
→ Apply proven patterns
→ Avoid known failures
→ Leverage collective knowledge
→ Higher quality results
```

### Simplicity Is When Discipline Matters Most

```
❌ "Simple tasks don't need skills"
✅ "Simple tasks benefit most from discipline"

Reality:
- Simple tasks → Easy to skip discipline
- Skip discipline → Bugs in simple code
- Bugs in simple code → Time wasted debugging
- Discipline on simple tasks → Clean, correct code
```

## Real-World Example

```markdown
## Task: "Add email validation to login form"

### Step 1: Inventory Skills
Available: TDD, systematic-debugging, verification, brainstorming, etc.

### Step 2: Assess Relevance
- test-driven-development: ✅ (implementing feature)
- verification-before-completion: ✅ (before claiming done)

### Step 3: Read Skills
[Uses Skill tool to read both skills]

### Step 4: Announce
"Using test-driven-development skill for email validation implementation"

### Step 5: Execute
1. Write failing test for email validation ✅
2. Verify test fails ✅
3. Implement validation logic ✅
4. Verify test passes ✅
5. Refactor if needed ✅
6. Use verification-before-completion before claiming done ✅

Result: Clean, tested implementation following proven workflow
```

## Integration with Other Skills

- **Foundational** - precedes all other skills
- **Mandatory** - not optional, required for all tasks
- **Universal** - applies to every task type
- **Meta** - ensures other skills are actually used

## Success Criteria

You're using superpowers correctly when:
- ✅ Check for relevant skills at start of EVERY task
- ✅ Read current skill version (don't rely on memory)
- ✅ Announce which skill you're using
- ✅ Follow skill exactly without shortcuts
- ✅ Never rationalize skipping skills
- ✅ Create TodoWrite items for skill checklists
- ✅ Discipline applies to simple tasks as much as complex
- ✅ Skills are mandatory, not optional
