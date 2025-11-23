---
name: testing-anti-patterns
description: Use when writing or modifying tests to avoid three critical failures - validating mock behavior instead of real code, polluting production with test-only methods, and uninformed mocking.
---

# Testing Anti-Patterns

## Core Principle

**"Test what the code does, not what the mocks do."**

This skill prevents three critical testing failures that lead to false confidence and brittle test suites.

## When to Use This Skill

Apply this skill when:
- Writing new tests
- Modifying existing tests
- Adding mocks to test suites
- Tempted to add methods only called by tests
- Debugging tests that mysteriously pass or fail
- Refactoring existing test code
- Reviewing pull requests with test changes

## The Three Iron Laws

### Law #1: Never Verify Mock Behavior

**❌ WRONG: Testing that mocks were called**

```javascript
test('user service calls database', () => {
  const mockDb = jest.fn();
  userService.getUser(mockDb, 123);

  expect(mockDb).toHaveBeenCalledWith('SELECT * FROM users WHERE id = ?', [123]);
  // This only tests that we called the mock!
});
```

**✅ RIGHT: Testing real behavior**

```javascript
test('user service returns user data', async () => {
  const mockDb = {
    query: jest.fn().mockResolvedValue([{ id: 123, name: 'Alice' }])
  };

  const user = await userService.getUser(mockDb, 123);

  expect(user).toEqual({ id: 123, name: 'Alice' });
  // This tests actual behavior - what getUser returns!
});
```

### Law #2: Never Add Test-Only Methods to Production Code

**❌ WRONG: Exposing internals for tests**

```javascript
// production code
class UserService {
  async createUser(data) {
    const validated = this._validateUser(data);
    return this._saveToDb(validated);
  }

  // ❌ This only exists for tests
  _validateUserForTesting(data) {
    return this._validateUser(data);
  }
}

// test
test('validation works', () => {
  expect(service._validateUserForTesting(invalidData)).toThrow();
});
```

**✅ RIGHT: Testing through public interface**

```javascript
// production code
class UserService {
  async createUser(data) {
    const validated = this._validateUser(data);
    return this._saveToDb(validated);
  }
  // No test-only methods!
}

// test
test('invalid user data throws error', async () => {
  await expect(service.createUser(invalidData)).rejects.toThrow();
  // Test through the real public API!
});
```

### Law #3: Never Mock Without Understanding

**❌ WRONG: Mocking blindly**

```javascript
test('payment processing', async () => {
  // What does processPayment actually do? Who knows!
  const mockStripe = {
    processPayment: jest.fn().mockResolvedValue({ success: true })
  };

  const result = await checkout.completeOrder(mockStripe, order);
  expect(result.paid).toBe(true);
});
```

**✅ RIGHT: Understanding before mocking**

```javascript
test('payment processing', async () => {
  // I know processPayment returns { id, status, amount, receipt_url }
  // because I read the Stripe docs and existing code
  const mockStripe = {
    processPayment: jest.fn().mockResolvedValue({
      id: 'pi_123',
      status: 'succeeded',
      amount: 4999,
      receipt_url: 'https://...'
    })
  };

  const result = await checkout.completeOrder(mockStripe, order);
  expect(result.paid).toBe(true);
  expect(result.receiptUrl).toBe('https://...');
  // Mock returns realistic data that matches real API!
});
```

## The TDD Foundation

**Strict Test-Driven Development prevents these anti-patterns naturally:**

1. **Write failing test first** (RED)
2. **Watch it fail against real code** (not mocks)
3. **Implement minimally** (GREEN)
4. **Refactor** (REFACTOR)

When you write tests after code, you're tempted to:
- Mock everything "just to be safe"
- Test implementation details
- Add test-only hooks to reach private methods

**TDD forces you to test behavior, not implementation.**

## Red Flags to Watch For

### 🚩 Red Flag #1: Test IDs Contain "Mock"

```javascript
// 🚩 Warning!
test('mock returns correct value', () => {
  // You're testing the mock, not your code
});
```

### 🚩 Red Flag #2: Methods Only in Test Files

```javascript
// production.js
class Service {
  doWork() { ... }
  _internalHelper() { ... }
  getInternalStateForTesting() { ... } // 🚩 Only called in tests!
}
```

### 🚩 Red Flag #3: Mock Setup > 50% of Test

```javascript
test('feature works', () => {
  // 30 lines of mock setup
  const mockA = { ... };
  const mockB = { ... };
  const mockC = { ... };

  // 2 lines of actual test
  const result = doThing(mockA, mockB, mockC);
  expect(result).toBe(true);

  // 🚩 More mock than test!
});
```

### 🚩 Red Flag #4: Tests Pass With Mocks Removed

```javascript
test('processes data', () => {
  const mockProcessor = jest.fn();
  service.process(mockProcessor);
  expect(mockProcessor).toHaveBeenCalled();
});

// 🚩 If you remove the expect(), does the test still compile?
// 🚩 If yes, you're not testing real behavior!
```

### 🚩 Red Flag #5: Mocking "Just to Be Safe"

```javascript
// 🚩 Why are we mocking this?
const mockLogger = jest.fn();
const mockConfig = jest.fn();
const mockMetrics = jest.fn();

// Do these even affect the behavior we're testing?
```

## Corrective Actions

### When You Catch Yourself Violating Law #1

1. Identify what **real behavior** you want to verify
2. Rewrite test to assert on that behavior
3. Keep mocks only for dependencies you must isolate
4. Verify behavior, not mock interactions

### When You Catch Yourself Violating Law #2

1. Ask: "Would this method exist without tests?"
2. If no, remove it from production code
3. Test through the real public interface instead
4. If private logic is complex, it might deserve its own class

### When You Catch Yourself Violating Law #3

1. Stop writing the test
2. Read documentation for what you're mocking
3. Look at real usage examples
4. Understand data structures and error cases
5. Then create realistic mocks

## Common Rationalizations (And Why They're Wrong)

### ❌ "But I need to test this private method!"

**Response:** No, you need to test the **behavior** that private method contributes to. Test through the public interface.

### ❌ "But checking mock calls is faster than integration testing!"

**Response:** Fast tests that don't verify real behavior are worthless. They give false confidence.

### ❌ "But I can't call the real API in tests!"

**Response:** That's fine - mock the API. But understand what the real API does first, and make your mock realistic.

### ❌ "But our code is legacy and hard to test!"

**Response:** Use TDD for new code. Gradually refactor legacy code to be more testable. Don't compound the problem.

## Integration with Other Skills

- **REQUIRED:** Use **test-driven-development** to prevent these anti-patterns
- Combine with **condition-based-waiting** for realistic async test scenarios
- Apply **systematic-debugging** when tests fail mysteriously

## Success Criteria

You're avoiding anti-patterns correctly when:
- ✅ Tests verify actual behavior, not mock calls
- ✅ No methods exist only for testing purposes
- ✅ You can explain what every mock represents
- ✅ Tests fail when behavior breaks (not when internals change)
- ✅ Mock data matches real API responses
- ✅ You write tests before code (TDD)
