---
name: root-cause-tracing
description: Use when errors occur deep in call stacks or stack traces show lengthy function call chains. Traces backward through execution to find where invalid data originated or incorrect behavior began.
---

# Root Cause Tracing

## Core Principle

**"Bugs often manifest deep in the call stack, but originate at entry points."**

Root cause tracing systematically identifies the original source of bugs that surface far from where they started. Rather than fixing symptoms, trace backward to discover where problems began.

## When to Use This Skill

Apply this technique when:
- **Errors occur far down the call stack** (not at entry points)
- **Stack traces show lengthy chains** of function calls
- **Source of corrupted data is unclear**
- **You need to determine which code path** triggered a problem
- **Error messages don't reveal the root cause**

## The Problem

```
Error: Cannot read property 'id' of undefined
  at processPayment (payment.js:45)
  at handleCheckout (checkout.js:123)
  at completeOrder (order.js:89)
  at submitForm (form.js:234)
  at onClick (button.js:12)

// Your instinct: Fix at payment.js:45
// The real issue: Invalid data passed at onClick
```

**Fixing at the symptom location doesn't prevent it from happening again.**

## The Four-Step Tracing Process

### Step 1: Observe the Symptom 📍

**Document where the error surfaces:**
- File and line number
- Error message
- What operation was being attempted
- What value was unexpectedly invalid

```
Example: payment.js:45 tries to access user.id
but user is undefined
```

### Step 2: Find Immediate Cause ⬆️

**Identify the code directly causing it:**
- What function contains the error?
- What value is invalid?
- Where did that value come from?

```
Example: processPayment(user, amount)
user parameter is undefined
Caller: handleCheckout() at line 123
```

### Step 3: Ask "What Called This?" ⬆️⬆️

**Work backward through the call chain:**
- What function called the immediate cause?
- Where did IT get the invalid value?
- Keep tracing upward through each caller

```
Example:
completeOrder() called handleCheckout(userData)
userData came from form submission
Form submission didn't validate user login state
```

### Step 4: Keep Tracing to Origin 🎯

**Follow invalid values to their source:**
- Continue until you reach the entry point
- Find where the invalid state was first created
- Identify the trigger that started the problematic flow

```
Example:
Root cause: onClick handler doesn't check if user is logged in
before starting checkout flow
```

## When Manual Tracing Fails

### Add Instrumentation

When call stack isn't clear, add logging **before problematic operations:**

```javascript
// ✅ Use console.error in tests (not logger - may not show)
console.error('DEBUG: About to process payment', {
  user: user,
  userId: user?.id,
  cwd: process.cwd(),
  env: process.env.NODE_ENV,
  stack: new Error().stack
});

processPayment(user, amount);
```

**Why console.error?**
- Always visible in test output
- Framework loggers may be suppressed
- Includes full context when error occurs

### Instrumentation Guidelines

**Add logging that captures:**
- Current directory paths
- Environment variables
- Full stack traces (not just error location)
- All relevant variable values (not just one)
- Timestamps (for timing-related issues)

```javascript
console.error('DEBUG: Checkout initiated', {
  timestamp: Date.now(),
  user: user,
  cart: cart,
  sessionId: req.sessionId,
  stack: new Error().stack
});
```

## Real-World Example

### Symptom
```
Error: User preferences undefined
  at savePreferences (preferences.js:89)
  at updateSettings (settings.js:45)
  at handleSubmit (form.js:123)
  at onSave (page.js:67)
```

### Tracing Process

**Step 1: Symptom**
- Location: preferences.js:89
- Problem: `user.preferences` is undefined

**Step 2: Immediate Cause**
- Function: savePreferences(user)
- Issue: `user` object is missing `preferences` property

**Step 3: Trace Backward**
- updateSettings() calls savePreferences(getCurrentUser())
- getCurrentUser() returns user from session
- Session doesn't include preferences data

**Step 4: Root Cause**
- Session creation doesn't fetch preferences from database
- Login endpoint only stores basic user info
- **Fix location**: Login endpoint, not preferences.js

### Before Fix (Wrong)
```javascript
// preferences.js:89 (symptom location)
function savePreferences(user) {
  // ❌ Symptom fix: Add default
  const prefs = user.preferences || {};
  // This masks the problem!
}
```

### After Fix (Correct)
```javascript
// auth.js:45 (root cause location)
async function login(credentials) {
  const user = await db.users.findOne(credentials);
  const preferences = await db.preferences.findOne({ userId: user.id });

  // ✅ Root cause fix: Include preferences in session
  req.session.user = { ...user, preferences };
}
```

## Validation at Multiple Layers

Once root cause is found, implement **defense-in-depth:**

**Layer 1: Entry Point**
```javascript
// Prevent invalid state from entering system
if (!user || !user.preferences) {
  throw new Error('User must be fully initialized');
}
```

**Layer 2: Business Logic**
```javascript
// Validate before critical operations
function savePreferences(user) {
  if (!user?.preferences) {
    throw new Error('User preferences not loaded');
  }
  // ...
}
```

**Layer 3: Data Layer**
```javascript
// Ensure data integrity
if (!preferences) {
  throw new Error('Preferences must exist for user');
}
```

## Common Mistakes

### Mistake #1: Fixing at Symptom Location

```
❌ "Error at line 45, so I'll add a null check at line 45"
✅ "Error at line 45, let me trace where null originated"
```

### Mistake #2: Stopping Too Early

```
❌ "Found that X is null, let me make X default to empty object"
✅ "Found that X is null, let me trace why X wasn't set properly"
```

### Mistake #3: Guessing Instead of Tracing

```
❌ "Probably a timing issue, let me add async/await"
✅ "Let me add instrumentation to see exact execution order"
```

## Integration with Other Skills

- Use **systematic-debugging** as the overall framework
- Apply **verification-before-completion** before claiming fix is complete
- Combine with **defense-in-depth** to add validation layers
- Use **test-driven-development** to create tests for root cause

## Success Criteria

You've traced the root cause correctly when:
- ✅ You can explain the full path from trigger to symptom
- ✅ Your fix is at the origin, not the symptom
- ✅ Fixing the root prevents the symptom entirely
- ✅ You've added validation to prevent similar issues
- ✅ The fix makes sense architecturally
- ✅ Tests prove the bug is fixed at its source
