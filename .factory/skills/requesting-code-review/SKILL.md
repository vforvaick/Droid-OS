---
name: requesting-code-review
description: Use after completing tasks or major features. Dispatches code-reviewer subagent to evaluate implementation quality against requirements, acting as quality gate before proceeding.
---

# Requesting Code Review

## Core Principle

**"Review early, review often."**

This skill dispatches a code-reviewer subagent to evaluate implementation quality against stated requirements or plans, acting as a quality gate that catches issues before they compound.

## When to Use This Skill

**Required scenarios:**
- After completing each task in subagent-driven development
- Upon finishing major features
- Prior to merging into the main branch
- Before claiming work is complete

**Beneficial scenarios:**
- When development feels stuck (fresh perspective helps)
- Before undertaking significant refactoring (establishes baseline)
- After resolving complex bugs (verify the fix is sound)
- When unsure about approach quality

## The Three-Step Process

### Step 1: Capture Context 📝

**Gather review materials:**
- **Git SHAs** marking start and end of work
- **Brief description** of what was implemented
- **Requirements or plan** that guided implementation
- **Test results** showing verification

**Example Context:**
```markdown
Work completed between commits:
- Start: abc123def
- End: xyz789ghi

Implementation: User authentication with JWT tokens

Requirements:
- Secure password hashing (bcrypt)
- JWT token generation
- Login/logout endpoints
- Session management

Tests: All passing (15 passed, 0 failed)
```

### Step 2: Dispatch Review 🚀

**Submit Task using code-reviewer:**

```markdown
**Task:** Review user authentication implementation

**Context:**
Commits: abc123def...xyz789ghi

Implemented features:
- User login with email/password
- JWT token generation and validation
- Secure password hashing with bcrypt
- Session management via HTTP-only cookies

Requirements (from plan):
[Paste relevant sections from original plan or requirements]

**What to review:**
- Security: Password handling, token security
- Code quality: Structure, naming, patterns
- Testing: Coverage and test quality
- Completeness: All requirements met
- Performance: Any obvious issues

**Constraints:**
- Focus on authentication code only
- Don't review unrelated modules

**Expected output:**
- Critical issues (must fix before proceeding)
- Important issues (should fix soon)
- Minor suggestions (optional improvements)
```

### Step 3: Act on Findings ⚡

**Triage review feedback:**

**Critical Issues → Fix Immediately**
```
Reviewer: "Passwords stored in plain text in logs"
Action: Fix immediately, this is a security vulnerability
```

**Important Issues → Fix Before Proceeding**
```
Reviewer: "No rate limiting on login endpoint"
Action: Add rate limiting before marking feature complete
```

**Minor Issues → Document for Later**
```
Reviewer: "Consider extracting validation logic to helper"
Action: Create ticket for future refactor, proceed for now
```

**Pushback When Appropriate**
```
Reviewer: "Should use OAuth instead of JWT"
Action: "Requirements specify JWT. OAuth is out of scope for this iteration."
```

## Code Review Request Template

```markdown
**Code Review Request**

## What Changed
Commits: [start-sha]...[end-sha]
Summary: [One sentence description]

## Requirements Met
- [ ] [Requirement 1]
- [ ] [Requirement 2]
- [ ] [Requirement 3]

## Implementation Approach
[2-3 sentences about key technical decisions]

## Test Coverage
```bash
npm test
# [Test results]
```

## Areas of Concern
[Optional: Specific areas you'd like reviewer to focus on]

## Out of Scope
[What the reviewer should NOT review in this iteration]
```

## Integration with Development Workflows

### With Subagent-Driven Development

```markdown
Task 1: Implement login → Review → Fix issues
Task 2: Implement logout → Review → Fix issues
Task 3: Add session mgmt → Review → Fix issues
Final Review → Merge
```

### With Feature Development

```markdown
Feature branch created
↓
Implementation complete
↓
Request code review ← YOU ARE HERE
↓
Address feedback
↓
Request final review
↓
Merge to main
```

## Common Review Findings

### Security Issues
- Passwords in logs or error messages
- Missing input validation
- SQL injection vulnerabilities
- XSS vulnerabilities
- Insecure token storage
- Missing rate limiting

### Code Quality Issues
- Unclear variable names
- Overly complex functions
- Duplicate code
- Missing error handling
- Tight coupling
- Poor abstraction

### Testing Issues
- Low test coverage
- Tests not following TDD
- Testing implementation not behavior
- Missing edge case tests
- Flaky tests

### Completeness Issues
- Requirements not fully met
- Missing error states
- Incomplete documentation
- Missing TypeScript types
- Accessibility not considered

## Responding to Review Feedback

### Pattern #1: Acknowledge and Fix

```
Reviewer: "Missing null check on user object"
You: "Good catch. Added null check at line 45. Verified with test."
```

### Pattern #2: Ask for Clarification

```
Reviewer: "Refactor this for better performance"
You: "Can you specify which part is concerning? The query or the loop?"
```

### Pattern #3: Respectful Pushback

```
Reviewer: "Add caching layer for all database queries"
You: "Caching is out of scope for this task. Requirements specify simple CRUD first.
     Can we defer caching to performance optimization phase?"
```

### Pattern #4: Request Examples

```
Reviewer: "Use better naming conventions"
You: "Can you provide an example? I want to ensure I understand the preferred pattern."
```

## Review Cadence

### Frequent Small Reviews
```
✅ Review after each 3-5 tasks
✅ Quick feedback loop
✅ Issues caught early
✅ Less overwhelming

❌ One massive review at the end
❌ Delayed feedback
❌ Issues compound
❌ Overwhelming rework
```

### Strategic Checkpoints
- After Phase 1 (foundation)
- After Phase 2 (core features)
- Before Phase 3 (integration)
- Final review before merge

## Integration with Other Skills

- **Required in subagent-driven-development** between tasks
- **Follows verification-before-completion** (verify before review)
- **Precedes finishing-a-development-branch** (review before merge)
- Use **receiving-code-review** to process feedback
- **Complements test-driven-development** (tests + review = quality)

## Success Criteria

You're requesting reviews effectively when:
- ✅ Reviews happen frequently, not just at the end
- ✅ Context is clear and complete
- ✅ Scope is focused (not reviewing entire codebase)
- ✅ Feedback is actionable
- ✅ Critical issues are fixed before proceeding
- ✅ Review becomes natural part of workflow
- ✅ Quality improves measurably over time
