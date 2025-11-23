---
name: defense-in-depth
description: Use when bugs manifest deep in workflows or when single validation points get bypassed. Implements validation at every layer (entry, business logic, environment, debug) to catch invalid data early.
---

# Defense-in-Depth Validation

## Core Principle

**"Make bugs structurally impossible through multi-layer validation."**

This pattern validates data at every system layer it passes through. Rather than relying on a single validation checkpoint, implement checks across entry points, business logic, environmental contexts, and debug instrumentation.

## When to Use This Skill

Apply this pattern when:
- **Invalid data causes failures deep in execution workflows**
- **A single validation point has been bypassed** by different code paths
- **Refactoring or mocking could circumvent existing checks**
- **The cost of the bug manifesting is high**
- **Data flows touch multiple system components**
- **Code could be called through varied routes**

## The Single Validation Problem

```javascript
// ❌ SINGLE POINT OF FAILURE
function processUser(userId) {
  // Only check at entry
  if (!userId) throw new Error('userId required');

  const user = getUser(userId);
  const preferences = user.preferences; // Assumes user exists!
  const theme = preferences.theme; // Assumes preferences exists!
  return applyTheme(theme); // Assumes theme exists!
}

// Problems:
// - getUser() might be called directly elsewhere (bypass check)
// - Mocking processUser() bypasses validation entirely
// - Refactoring might remove entry check accidentally
// - Deep failures are hard to diagnose
```

## The Four-Layer Defense Pattern

### Layer 1: Entry Point Validation 🚪

**Reject obviously invalid input at API boundaries**

```javascript
// Layer 1: API Entry
function processUser(userId) {
  // Reject immediately invalid inputs
  if (!userId) {
    throw new Error('userId is required');
  }
  if (typeof userId !== 'string') {
    throw new Error('userId must be a string');
  }
  if (userId.trim() === '') {
    throw new Error('userId cannot be empty');
  }

  return executeUserProcess(userId);
}
```

**What to validate:**
- Required parameters present
- Correct types (string, number, object)
- Non-empty values
- Basic format rules (email format, UUID format, etc.)

### Layer 2: Business Logic Validation 🔍

**Ensure data semantically makes sense for the operation**

```javascript
// Layer 2: Business Logic
async function executeUserProcess(userId) {
  const user = await getUser(userId);

  // Validate business logic requirements
  if (!user) {
    throw new Error(`User ${userId} not found`);
  }
  if (!user.preferences) {
    throw new Error(`User ${userId} missing preferences`);
  }
  if (!user.preferences.theme) {
    throw new Error(`User ${userId} preferences missing theme`);
  }

  return applyUserTheme(user);
}
```

**What to validate:**
- Data exists in database
- Relationships are properly loaded
- Required properties are present
- Data is in expected state (not deleted, not expired, etc.)

### Layer 3: Environment Guards 🛡️

**Prevent dangerous operations in particular contexts**

```javascript
// Layer 3: Environment Guards
function initializeGitRepo(directory) {
  // Guard against dangerous operations in wrong environment
  if (process.env.NODE_ENV === 'test' && !directory.includes('/tmp/')) {
    throw new Error(
      'Test environment git operations must use /tmp/ directory. ' +
      `Attempted: ${directory}`
    );
  }

  if (!directory || directory === '/') {
    throw new Error('Cannot initialize git in root directory');
  }

  return git.init(directory);
}
```

**What to guard:**
- Testing doesn't affect production data
- Development doesn't send real emails
- Destructive operations require explicit confirmation
- File operations stay within safe boundaries

### Layer 4: Debug Instrumentation 🔬

**Capture context for forensic analysis when failures occur**

```javascript
// Layer 4: Debug Instrumentation
function applyUserTheme(user) {
  // Capture full context for debugging
  console.error('DEBUG: applyUserTheme called', {
    userId: user?.id,
    hasPreferences: !!user?.preferences,
    theme: user?.preferences?.theme,
    cwd: process.cwd(),
    env: process.env.NODE_ENV,
    stack: new Error().stack
  });

  // Even with all validation, add defensive check
  if (!user?.preferences?.theme) {
    throw new Error(
      'applyUserTheme called with invalid user object. ' +
      'This should be impossible - check earlier validation layers.'
    );
  }

  return themeEngine.apply(user.preferences.theme);
}
```

**What to capture:**
- Directory and working paths
- Environment variables
- Full stack traces
- All relevant variable values
- Timestamps for timing issues

## Full Defense-in-Depth Example

```javascript
// Layer 1: Entry Point
async function updateUserProfile(req, res) {
  const { userId, updates } = req.body;

  // Entry validation
  if (!userId || typeof userId !== 'string') {
    return res.status(400).json({ error: 'Valid userId required' });
  }
  if (!updates || typeof updates !== 'object') {
    return res.status(400).json({ error: 'Valid updates object required' });
  }

  try {
    const result = await profileService.update(userId, updates);
    return res.json(result);
  } catch (error) {
    return res.status(500).json({ error: error.message });
  }
}

// Layer 2: Business Logic
class ProfileService {
  async update(userId, updates) {
    // Business logic validation
    const user = await db.users.findById(userId);
    if (!user) {
      throw new Error(`User ${userId} not found`);
    }
    if (user.deleted) {
      throw new Error(`Cannot update deleted user ${userId}`);
    }
    if (!user.profile) {
      throw new Error(`User ${userId} missing profile - data corruption?`);
    }

    return this.applyUpdates(user, updates);
  }

  // Layer 3: Environment Guards
  async applyUpdates(user, updates) {
    // Guard against dangerous operations
    if (updates.email && process.env.NODE_ENV === 'development') {
      console.warn('Email update in development - email will not be sent');
      updates.emailVerified = true; // Skip verification in dev
    }

    if (updates.role === 'admin' && process.env.NODE_ENV === 'production') {
      throw new Error('Admin role changes must use administrative tool');
    }

    return this.performUpdate(user, updates);
  }

  // Layer 4: Debug Instrumentation
  async performUpdate(user, updates) {
    console.error('DEBUG: performUpdate', {
      userId: user?.id,
      updateKeys: Object.keys(updates || {}),
      currentRole: user?.role,
      newRole: updates?.role,
      env: process.env.NODE_ENV,
      timestamp: Date.now(),
      stack: new Error().stack
    });

    // Final defensive check
    if (!user || !user.id) {
      throw new Error(
        'performUpdate called with invalid user. ' +
        'This should be impossible - check validation layers.'
      );
    }

    return db.users.update(user.id, updates);
  }
}
```

## Implementation Process

### Step 1: Trace Complete Data Flow

Map where data enters and flows through your system:
```
Entry → Validation → Business Logic → Database → Return
```

### Step 2: Map All Checkpoints

Identify every layer data passes through:
- API endpoints
- Service layer functions
- Database queries
- Utility functions
- Return paths

### Step 3: Add Validation at Each Layer

Implement appropriate validation for each layer:
- Entry: Type and format checks
- Business: Semantic and existence checks
- Environment: Context-appropriate guards
- Debug: Instrumentation for failures

### Step 4: Test Bypass Scenarios

Verify that bypassing early layers still catches issues:
```javascript
// Test: What if business logic called directly?
test('business logic validates even when called directly', () => {
  expect(() => profileService.update(null, {}))
    .rejects.toThrow('User null not found');
});

// Test: What if data is mocked incorrectly?
test('final layer catches invalid mocked data', () => {
  const invalidUser = { id: 123 }; // Missing required fields
  expect(() => profileService.performUpdate(invalidUser, {}))
    .rejects.toThrow();
});
```

## Benefits of Defense-in-Depth

### Benefit #1: Bugs Caught Early

Invalid data triggers errors immediately, not deep in execution:
```
❌ Without: Error in applyTheme() after 10 function calls
✅ With: Error at entry point, clear cause
```

### Benefit #2: Bypass-Resistant

Even if one validation is bypassed, others catch issues:
```
❌ Without: Mocking bypasses validation completely
✅ With: Each layer validates independently
```

### Benefit #3: Clear Error Messages

Failures are specific about what's wrong and where:
```
❌ Without: "Cannot read property 'theme' of undefined"
✅ With: "User 123 preferences missing theme"
```

### Benefit #4: Refactor-Safe

Code changes don't accidentally remove critical checks:
```
❌ Without: Refactor removes only validation point
✅ With: Multiple layers ensure safety
```

## Common Mistakes

### Mistake #1: Only Validating at Entry

```
❌ "I validated at the API, so internal functions are safe"
✅ "Internal functions validate too - they might be called directly"
```

### Mistake #2: Trusting Mocks

```
❌ "Tests pass, so validation works"
✅ "Tests with realistic mocks AND validation in production code"
```

### Mistake #3: Skipping Environment Guards

```
❌ "It won't happen in production"
✅ "Tests shouldn't be able to affect production data at all"
```

### Mistake #4: No Debug Instrumentation

```
❌ "If validation fails, the error message is enough"
✅ "Capture full context so I can understand HOW validation was bypassed"
```

## Integration with Other Skills

- Use **root-cause-tracing** when defense layers catch issues
- Combine with **systematic-debugging** to add layers after fixing bugs
- Apply with **test-driven-development** to test each layer independently
- Use **verification-before-completion** to ensure all layers work

## Success Criteria

You've implemented defense-in-depth correctly when:
- ✅ Validation exists at multiple independent layers
- ✅ Bypassing early validation still catches issues
- ✅ Error messages clearly identify which layer failed
- ✅ Tests verify each layer independently
- ✅ Refactoring doesn't accidentally remove all validation
- ✅ Invalid data is structurally difficult to reach deep code
