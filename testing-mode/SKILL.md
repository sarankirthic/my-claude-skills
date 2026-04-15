---
name: testing-mode
description: >
  Risk-driven testing. Ask what breaks, then test it. Trigger when: "test this", "write tests",
  "edge cases", "what breaks", "test strategy", or any request for tests. Prevents happy-path
  blindness. Focuses on failure modes first, test writing second.
---

# Testing Mode: Risk-Driven Testing

## Purpose

Write tests that catch bugs. Most tests verify happy path. Miss 80% of failures.

Testing-mode: Ask what breaks → test that → done.

---

## The 4 Steps

### Step 1: Ask What Breaks
Use interrogation questions (not brainstorm):

- What inputs would break this? (null, empty, huge, negative, wrong type)
- What state transitions fail? (invalid states, bad sequences)
- What external calls fail? (API down, timeout, permission denied)
- What happens under load? (concurrency, large data, memory spike)
- What edge math? (off-by-one, divide by zero, rounding)

List 3-5 answers.

### Step 2: Rank by Impact
Rank risks:
- **P1 (high):** Data loss, security, crashes
- **P2 (medium):** Wrong output, recoverable errors
- **P3 (low):** Edge cases, rare scenarios

Pick top 2-3. P1 always, then highest P2.

### Step 3: Write Focused Tests
1 test per risk. Verify it fails without fix, passes with fix.

Format: `test('handles [risk]', () => { ... })`

### Step 4: Run & Confirm
Tests catch the failure. Done.

---

## Example: Simple vs Complex

**Simple (Getter):**
```
function getUserById(id) { return users[id]; }

Risks: null id, missing user, wrong type
Tests: 3 focused tests covering each
```

**Complex (API Call):**
```
async function saveUser(user) {
  validate(user);
  const response = await api.post('/users', user);
  return response.data;
}

Risks:
- P1: Invalid data passes validation (data corruption)
- P1: API fails (network error, 500)
- P2: Timeout (slow network)
- P2: Permission denied (401)
- P3: Large payload (performance)

Pick P1 risks → Write 2 tests:
Test 1: Invalid user rejected by validate()
Test 2: API 500 error handled gracefully
```

Complex function. Just 2 critical tests. Not 20.

---

## Risk Categories Quick Reference

| Category | Ask | Example |
|----------|-----|---------|
| Input | What input breaks? | null, empty, huge, wrong type |
| State | What state is invalid? | transition not allowed |
| External | What external call fails? | API error, timeout |
| Concurrency | What race condition? | 2 calls simultaneous |
| Math | What edge math? | off-by-one, divide-by-zero |

---

## Test Quality Rules

✓ 1 test = 1 risk (not 5 risks)
✓ Name describes risk: `test('handles null id')`
✓ Fails without fix (prove it catches bug)
✓ Passes with fix (prove fix works)
✓ No redundancy (don't repeat same risk)

---

## Session Behavior

Once testing-mode active:
- Ask: "What breaks?" (use interrogation questions)
- List 3-5 risks
- Rank by impact
- Write 2-3 tests (P1 + top P2)
- Run tests
- Stay in mode until done or user says "stop"
