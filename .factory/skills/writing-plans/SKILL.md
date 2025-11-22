---
name: writing-plans
description: Use when design is finalized and ready for implementation. Creates comprehensive plans with exact file paths, complete code examples, and verification steps for developers with minimal codebase familiarity.
---

# Writing Plans

## Core Principle

**"Bite-sized tasks with exact file paths and complete code examples."**

This skill generates comprehensive implementation plans that break features into actionable 2-5 minute tasks, making execution possible even for developers unfamiliar with the codebase.

## When to Use This Skill

**Use when:**
- Design is finalized and ready for engineering implementation
- Need to onboard developers unfamiliar with your codebase
- Creating detailed specifications that don't assume domain knowledge
- Breaking down complex features into manageable steps
- Preparing work for parallel execution or delegation

**Don't use when:**
- Design is still being refined (use **brainstorming** first)
- Task is simple enough to implement directly
- Plan already exists and just needs execution

## Plan Structure

### Required Headers

Every plan must include:

```markdown
# [Feature Name]

## Goal
[One sentence: what we're building and why]

## Architecture Overview
[2-3 paragraphs: how this fits into existing system]

## Tech Stack
- Framework: Next.js 14+ / React 18+ / etc.
- Database: PostgreSQL with Prisma ORM
- Styling: Tailwind CSS with shadcn/ui
[List all relevant technologies]

## Implementation Tasks
[Numbered list of atomic tasks]

## Verification
[How to verify the feature works]
```

## Task Granularity

### The 2-5 Minute Rule

Each task represents **2-5 minutes of work** following TDD principles:

```markdown
## Task 1: Create user authentication test

**File:** `src/services/__tests__/auth.service.test.ts`

**Action:** Create new file with failing test

**Code:**
```typescript
import { AuthService } from '../auth.service';

describe('AuthService', () => {
  it('should authenticate valid user', async () => {
    const authService = new AuthService();
    const result = await authService.login('user@example.com', 'password123');

    expect(result.success).toBe(true);
    expect(result.token).toBeDefined();
  });
});
```

**Verification:**
```bash
npm test -- auth.service.test.ts
# Should fail: AuthService doesn't exist yet
```

**Commit:**
```bash
git add src/services/__tests__/auth.service.test.ts
git commit -m "test: add failing test for user authentication"
```
```

### Task Template

```markdown
## Task [N]: [Clear action verb] [What]

**File:** `exact/path/to/file.ts`
**Lines:** 45-67 (if modifying existing file)

**Action:** [One sentence describing what to do]

**Code:**
```language
[Complete, copy-pasteable code]
```

**Verification:**
```bash
[Exact command to verify]
# [Expected output]
```

**Commit:**
```bash
[Exact git commands]
```
```

## Documentation Standards

### Exact File Paths

```
❌ "Create a user service"
✅ "Create src/services/user.service.ts"

❌ "Update the config"
✅ "Update next.config.js lines 12-15"
```

### Complete Code Examples

```
❌ "Add the login function"
✅ "Add this exact code:
    ```typescript
    async function login(email: string, password: string) {
      // [complete implementation]
    }
    ```"
```

### Specific Verification

```
❌ "Make sure it works"
✅ "Run: npm test
    Expected: 12 passed, 0 failed"
```

## TDD Workflow per Task

### Step 1: Write Failing Test (RED)

```markdown
## Task N: Write test for login validation

**File:** `src/__tests__/login.test.ts`
**Action:** Create test that will fail

[Complete test code...]

**Verification:** npm test -- login.test.ts
Expected: ❌ FAIL (function doesn't exist)
```

### Step 2: Minimal Implementation (GREEN)

```markdown
## Task N+1: Implement login function

**File:** `src/auth/login.ts`
**Action:** Write minimal code to pass test

[Complete implementation code...]

**Verification:** npm test -- login.test.ts
Expected: ✅ PASS (all tests pass)
```

### Step 3: Refactor (REFACTOR)

```markdown
## Task N+2: Refactor login for clarity

**File:** `src/auth/login.ts`
**Lines:** 12-24
**Action:** Extract validation logic to helper

[Refactored code...]

**Verification:** npm test
Expected: ✅ PASS (still passing after refactor)
```

## Core Principles

### DRY (Don't Repeat Yourself)

```
❌ Plan duplicates similar code in multiple tasks
✅ Plan creates reusable helper, then uses it in multiple places
```

### YAGNI (You Aren't Gonna Need It)

```
❌ Plan includes "might need later" features
✅ Plan implements only requested requirements
```

### Frequent Commits

```
❌ One commit at the end
✅ Commit after every task (every 2-5 minutes)
```

## Execution Models

After plan completion, choose execution approach:

### Option A: Subagent-Driven Development

- Fresh agent per task
- Code review between tasks
- Use **subagent-driven-development** skill

### Option B: Parallel Session Execution

- Separate session processes batches
- Review checkpoints between batches
- Use **executing-plans** skill

### Option C: Manual Execution

- Follow plan step-by-step manually
- Commit after each task
- Use **verification-before-completion** skill

## Plan Storage

Save plans to standardized location:

```
docs/plans/YYYY-MM-DD-<feature-name>.md
```

Examples:
- `docs/plans/2025-01-15-user-authentication.md`
- `docs/plans/2025-01-16-payment-integration.md`

## Real-World Example

```markdown
# User Authentication System

## Goal
Implement secure JWT-based authentication for existing Next.js application.

## Architecture Overview
Add authentication to existing Next.js 14 app using App Router. Backend
uses PostgreSQL via Prisma. Frontend uses Server Actions for form
submission. Authentication state managed via HTTP-only cookies.

Current structure: src/app (routes), src/services (business logic),
src/lib (utilities). We'll add: src/services/auth.service.ts,
src/app/api/auth routes, src/app/login page.

## Tech Stack
- Framework: Next.js 14+ (App Router)
- Database: PostgreSQL with Prisma ORM
- Auth: JWT tokens with bcrypt password hashing
- Validation: Zod schemas
- Testing: Jest with @testing-library/react

## Implementation Tasks

### Task 1: Create auth service test
**File:** `src/services/__tests__/auth.service.test.ts`
**Action:** Write failing test for user login
**Code:**
```typescript
import { AuthService } from '../auth.service';

describe('AuthService', () => {
  it('should authenticate valid user', async () => {
    const service = new AuthService();
    const result = await service.login('user@test.com', 'password');
    expect(result.success).toBe(true);
  });
});
```
**Verification:** `npm test` → ❌ FAIL (AuthService not found)
**Commit:** `git commit -m "test: add failing test for auth service"`

### Task 2: Implement auth service
**File:** `src/services/auth.service.ts`
**Action:** Create minimal implementation
**Code:**
```typescript
import { prisma } from '@/lib/prisma';
import bcrypt from 'bcrypt';

export class AuthService {
  async login(email: string, password: string) {
    const user = await prisma.user.findUnique({ where: { email } });
    if (!user) return { success: false };

    const valid = await bcrypt.compare(password, user.passwordHash);
    return { success: valid };
  }
}
```
**Verification:** `npm test` → ✅ PASS
**Commit:** `git commit -m "feat: implement basic auth service"`

[... continue with remaining tasks ...]

## Verification
1. Run all tests: `npm test` → all pass
2. Start dev server: `npm run dev`
3. Navigate to http://localhost:3000/login
4. Try login with test credentials
5. Verify redirect to dashboard on success
```

## Integration with Other Skills

- **Follows brainstorming** for design validation
- **Use executing-plans** for batch execution
- **Use subagent-driven-development** for iterative execution
- **Apply test-driven-development** in each task
- **Use verification-before-completion** for quality gates

## Success Criteria

You've written a good plan when:
- ✅ Each task is 2-5 minutes of focused work
- ✅ Every file path is exact and specific
- ✅ All code examples are complete and copy-pasteable
- ✅ Verification steps have exact commands + expected output
- ✅ A developer unfamiliar with the codebase can execute it
- ✅ Plan follows TDD principles (test → implement → refactor)
- ✅ No assumptions about domain knowledge
- ✅ DRY and YAGNI principles are applied
