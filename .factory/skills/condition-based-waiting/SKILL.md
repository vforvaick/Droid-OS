---
name: condition-based-waiting
description: Use when tests have arbitrary delays or race conditions. Replaces sleep/setTimeout with active polling until conditions are met, improving reliability and speed.
---

# Condition-Based Waiting

## Core Principle

**"Wait for the actual condition you care about, not a guess about how long it takes."**

This technique replaces arbitrary delays in tests with active condition polling, dramatically improving test reliability and speed.

## When to Use This Skill

**Apply condition-based waiting when:**
- Tests contain arbitrary delays (`setTimeout`, `sleep`, `Thread.sleep`)
- Tests fail inconsistently under load or in CI environments
- Waiting for asynchronous operations to complete
- Race conditions cause flaky pass/fail behavior

**Avoid this approach when:**
- Testing actual timing behavior (like debounce intervals)
- Documented timeouts are intentional and justified

## The Problem with Arbitrary Delays

```javascript
// ❌ BAD: Guessing how long things take
await sleep(1000); // Will this be enough? Too much?
expect(element).toBeVisible();

// Problems:
// - Too short: flaky failures in CI
// - Too long: unnecessarily slow tests
// - Load-dependent: works locally, fails in CI
```

## The Condition-Based Solution

### Core Pattern: Poll Until Condition Met

```javascript
// ✅ GOOD: Wait for actual condition
await waitFor(() => element.isVisible(), {
  timeout: 5000,
  interval: 10
});
```

## Implementation Pattern

### Three-Step Methodology

**1. Define the Condition**

Specify what state change you're waiting for:
- An event type to be emitted
- A state value to change
- A count threshold to be reached
- A file to exist
- An element to become visible

**2. Implement Polling Loop**

Create a loop that repeatedly checks the condition:
```javascript
async function waitForCondition(checkFn, timeout = 5000) {
  const startTime = Date.now();

  while (Date.now() - startTime < timeout) {
    if (await checkFn()) {
      return true; // Condition met!
    }
    await sleep(10); // Poll every 10ms
  }

  throw new Error('Condition not met within timeout');
}
```

**3. Set Timeout Limits**

Always include maximum wait duration:
- Prevents indefinite loops
- Provides clear error messages
- Fails fast when conditions never materialize

## Real-World Examples

### Example 1: Event-Based Waiting

```javascript
// ❌ BAD: Arbitrary delay
await sleep(500);
expect(events).toHaveLength(3);

// ✅ GOOD: Wait for event count
await waitFor(() => events.length === 3, {
  timeout: 5000,
  message: 'Expected 3 events to be emitted'
});
```

### Example 2: State Change Waiting

```javascript
// ❌ BAD: Guessing state transition time
await sleep(2000);
expect(component.state).toBe('loaded');

// ✅ GOOD: Poll for state
await waitFor(() => component.state === 'loaded', {
  timeout: 5000,
  message: 'Component never reached loaded state'
});
```

### Example 3: File System Waiting

```javascript
// ❌ BAD: Hope file is written
await sleep(1000);
const content = fs.readFileSync('output.txt');

// ✅ GOOD: Poll for file existence
await waitFor(() => fs.existsSync('output.txt'), {
  timeout: 5000,
  message: 'output.txt was never created'
});
const content = fs.readFileSync('output.txt');
```

## Configuration Guidelines

### Polling Interval: 10ms (Recommended)

- Fast enough to catch changes quickly
- Not so fast it wastes CPU cycles
- Balances responsiveness with efficiency

### Timeout Values

- **Unit tests**: 5000ms (5 seconds)
- **Integration tests**: 10000ms (10 seconds)
- **E2E tests**: 30000ms (30 seconds)
- Adjust based on actual operation speed

### Clear Error Messages

```javascript
await waitFor(() => condition, {
  timeout: 5000,
  message: 'User login never completed - check auth service logs'
});
```

## Real-World Impact

**Before condition-based waiting:**
- Test pass rate: 60% in CI
- Average test time: 45 seconds
- Developer frustration: High

**After condition-based waiting:**
- Test pass rate: 100% in CI
- Average test time: 27 seconds (40% faster!)
- Developer frustration: Low

## Common Patterns

### Pattern 1: Element Visibility

```javascript
await waitFor(() => element.isVisible());
```

### Pattern 2: Array Length

```javascript
await waitFor(() => items.length > 0);
```

### Pattern 3: Property Value

```javascript
await waitFor(() => user.status === 'active');
```

### Pattern 4: Async Operation Complete

```javascript
await waitFor(() => !operation.isPending());
```

## Integration with Other Skills

- Use with **test-driven-development** for reliable TDD cycles
- Combine with **testing-anti-patterns** to avoid over-mocking async behavior
- Apply **systematic-debugging** when conditions never materialize

## Success Criteria

You're using condition-based waiting correctly when:
- ✅ No arbitrary `sleep()` or `setTimeout()` in tests
- ✅ Tests pass consistently in all environments
- ✅ Test failures have clear timeout messages
- ✅ Test suite runs faster than with arbitrary delays
- ✅ Conditions specify exactly what you're waiting for
