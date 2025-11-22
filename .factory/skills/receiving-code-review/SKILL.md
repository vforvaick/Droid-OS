---
name: receiving-code-review
description: Use when processing reviewer feedback. Provides structured approach to evaluate and respond with technical rigor rather than performative agreement, requiring verification before implementation.
---

# Receiving Code Review

## Core Principle

**"Technical verification over performative agreement."**

This skill provides a structured approach to evaluate and respond to code review feedback with technical rigor, preventing blind implementation of suggestions by requiring verification against actual codebase behavior.

## When to Use This Skill

Apply when:
- **Receiving feedback** from reviewers (trusted or external)
- **Unclear guidance** on implementation details
- **Questionable suggestions** that might break functionality
- **Multi-item feedback** requiring prioritization
- **Conflicting advice** between different reviewers

## The Response Pattern

### Step 1: Read Complete Feedback Without Reaction 📖

**Absorb all feedback first:**
- Don't respond immediately to individual points
- Read entire review start to finish
- Note patterns in feedback
- Identify critical vs. optional items

**Don't:**
- ❌ React defensively to first criticism
- ❌ Respond before reading everything
- ❌ Focus on disagreements, miss valid points

### Step 2: Restate Requirements 🎯

**Confirm understanding:**
```
Reviewer: "Refactor the authentication flow"
You: "To clarify - you want me to:
     A) Extract validation logic to separate function
     B) Change from JWT to session-based auth
     C) Something else?

     Want to ensure I understand the requirement."
```

**If reviewer's intent is unclear:**
```
Reviewer: "Improve the error handling"
You: "Can you specify which error scenarios? Currently handling:
     - Invalid credentials (400)
     - Server errors (500)
     - Network timeouts (408)

     Which needs improvement, or is there a missing scenario?"
```

### Step 3: Verify Against Reality ✅

**Check actual codebase behavior:**
```
Reviewer: "User object is always undefined here"
You: *Checks code* "Actually, user is validated at line 23 via middleware.
     Let me verify this is working..."

     *Runs tests* "Tests confirm user is defined. Are you seeing undefined
     in a specific scenario I'm missing?"
```

**Don't assume reviewer is correct without verification:**
- ❌ "You're right, I'll fix it!" (without checking)
- ✅ "Let me verify this concern..." (check first, then respond)

### Step 4: Evaluate Technical Soundness 🔬

**Consider for this specific stack:**
```
Reviewer: "Use Redux for state management"
Your evaluation:
- Current stack: React with Context API
- Redux adds: 500KB bundle size, complexity
- Current solution: Works for current scale
- Decision: Defer Redux unless specific scaling issue identified
```

**Ask technical questions:**
```
Reviewer: "This query is slow"
You: "Good point. What query time are you seeing?
     I'm getting 50ms in tests.
     Is this production vs. dev environment difference?"
```

### Step 5: Respond with Technical Acknowledgment or Reasoning 💬

**For valid feedback:**
```
✅ "You're right - missing validation on email field.
    Adding validation and test for it now."
```

**For feedback requiring context:**
```
✅ "The redundant code you mentioned is intentional - auth and admin
    flows have different validation rules despite similar structure.
    Happy to add comments explaining why they're separate."
```

**For feedback that conflicts with requirements:**
```
✅ "OAuth is outside scope for this iteration. Requirements specify
    JWT-based auth. Can we defer OAuth to Phase 2 after MVP launch?"
```

### Step 6: Implement One Item at a Time 🔨

**Sequential implementation with testing:**
```
Feedback Item 1: Add email validation
→ Implement
→ Test
→ Commit
→ Verify no regressions

Feedback Item 2: Extract helper function
→ Implement
→ Test
→ Commit
→ Verify no regressions

[Continue pattern...]
```

**Don't:**
- ❌ Batch all changes without testing between
- ❌ Implement multiple suggestions simultaneously
- ❌ Skip testing individual changes

## Response Types by Feedback Source

### Trusted Partner Feedback

**Characteristics:**
- Deep context about project
- Aligned on requirements
- Track record of good judgment

**Response approach:**
```
1. Implement after understanding scope
2. Still clarify if unclear
3. Higher trust, but verify critical changes
4. Pushback when requirements conflict
```

### External Reviewer Feedback

**Characteristics:**
- May lack project context
- Different assumptions about goals
- Varying quality of suggestions

**Response approach:**
```
1. Be respectfully skeptical
2. Verify technical correctness first
3. Check for YAGNI violations
4. Ensure no regressions
5. Pushback when appropriate with reasoning
```

## Critical Distinctions

### Performative vs. Technical Acknowledgment

**❌ Performative (Avoid):**
```
"You're absolutely right!"
"Great catch!"
"I should have thought of that!"
```

**✅ Technical (Prefer):**
```
"Verified - missing null check at line 45. Adding now."
"Confirmed query returns 0 rows in edge case. Adding fallback."
"Checked tests - coverage is 60%. Adding tests for X, Y, Z scenarios."
```

### Implementation vs. Clarification

**❌ Blind Implementation:**
```
Reviewer: "Refactor this"
You: *Immediately refactors* ← Dangerous!
```

**✅ Clarification First:**
```
Reviewer: "Refactor this"
You: "Can you clarify what aspect? The:
     A) Function length (currently 50 lines)
     B) Naming (current: processUserData)
     C) Logic complexity (cyclomatic complexity: 8)
     D) Something else?"
```

## When to Pushback

### Scenario #1: Breaks Functionality

```
Reviewer: "Remove this validation check"
You: "That validation prevents SQL injection. Tests:
     test-sql-injection.ts would fail without it.
     Did you mean a different check?"
```

### Scenario #2: Lacks Context

```
Reviewer: "Use microservices here"
You: "Current traffic: 100 req/day. Microservices would add:
     - 4 new services to maintain
     - Docker orchestration complexity
     - Network latency between services

     Monolith is appropriate for current scale.
     Can we defer microservices until >10K req/day?"
```

### Scenario #3: Violates YAGNI

```
Reviewer: "Add support for 10 different databases"
You: "Requirements specify PostgreSQL only for MVP.
     Supporting 10 databases adds significant complexity:
     - 10 different adapters
     - Testing across 10 databases
     - Migration paths for each

     Can we focus on PostgreSQL and add others based on
     actual user demand?"
```

### Scenario #4: Conflicts with Architectural Decisions

```
Reviewer: "Switch to NoSQL"
You: "Architecture was decided in planning phase:
     - Strong consistency required (transactions)
     - Relational data model fits domain
     - Team expertise in PostgreSQL

     Switching to NoSQL would require re-architecting.
     Was there a specific issue with current approach?"
```

## Forbidden Patterns

### ❌ Pattern #1: Performative Agreement

```
"You're absolutely right! I'll fix everything you mentioned!"
```

**Why wrong:** Doesn't verify, doesn't evaluate, blind trust

### ❌ Pattern #2: Defensive Reaction

```
"That's not a bug, it's a feature!"
"You don't understand the requirements!"
"I did it this way because..."
```

**Why wrong:** Closes communication, misses valid feedback

### ❌ Pattern #3: Batching Without Testing

```
*Implements all 10 suggestions at once*
*Runs tests once at the end*
*Everything breaks*
```

**Why wrong:** Can't isolate which change caused issues

### ❌ Pattern #4: Avoiding Pushback

```
Reviewer suggests problematic change
You implement it anyway to avoid conflict
Feature breaks
```

**Why wrong:** False agreement damages project more than honest discussion

## Integration with Other Skills

- **Required with requesting-code-review** (request → receive → respond)
- **Use verification-before-completion** after implementing feedback
- **Apply test-driven-development** for feedback-driven changes
- **Use systematic-debugging** if feedback reveals bugs

## Success Criteria

You're receiving reviews effectively when:
- ✅ You verify feedback against actual code behavior
- ✅ You ask clarifying questions before implementing
- ✅ You pushback when feedback conflicts with requirements
- ✅ You implement changes one at a time with testing
- ✅ Your responses are technical, not performative
- ✅ You catch invalid suggestions before implementing
- ✅ Relationship with reviewer improves (honest communication builds trust)
