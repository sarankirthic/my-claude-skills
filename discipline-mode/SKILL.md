---
name: discipline-mode
description: >
  Unified debugging skill with 3 intensity levels (light/medium/strict). Trigger when: "debug", "what's wrong",
  "find bug", "discipline mode", "strict debug", "quick debug", "assert", "investigate", "troubleshoot",
  or any bug report. Skill auto-detects intensity based on bug complexity. Prevents token waste from blind guessing.
  Replaces manual debugging loops with systematic, precondition-gated flow.
---

# Discipline Mode: Systematic Debugging (3 Intensities)

## Purpose

Debug efficiently. Stop guessing before implementing. Three intensity levels:
- **Light:** Fast assertions. Basic preconditions. Overhead: minimal. Best for: simple bugs (typos, obvious errors).
- **Medium:** Flexible checkpoints with looping. Confidence gates. Overhead: moderate. Best for: mixed-complexity bugs.
- **Strict:** Mandatory phases, no skips. Exhaustive rigor. Overhead: high. Best for: hard bugs, production incidents.

All three prevent 30-70% token waste by blocking bad fixes.

---

## Pick Intensity by Trigger

**Light Mode Triggers:** "debug typo", "quick debug", "check this", simple error message visible
- Fast. Overhead: minimal. Best: obvious bugs.

**Medium Mode Triggers:** "debug", "what's wrong", "investigate", unclear cause, tried 1 approach
- Flexible. Overhead: moderate. Best: mixed complexity.

**Strict Mode Triggers:** "strict debug", "discipline mode", "hard bug", "prod incident", tried 3+ fixes
- Rigorous. Overhead: high. Best: complex bugs, race conditions.

---

## LIGHT MODE: Fast Assertions

**When:** Simple bugs, clear error messages, short investigation.

**How:** 4 assertion gates before any fix.

```
GATE 1: Error Signal Present?
  ✓ YES: Have error output / failing test
  ✗ NO: Ask for it

GATE 2: Hypothesis Grounded?
  ✓ SIGNAL: Based on actual error
  ✗ GUESS: Refine or ask for more info

GATE 3: Testable in <2 min?
  ✓ YES: Existing test or easy new one
  ✗ NO: Plan test first

GATE 4: Scope Isolated?
  ✓ CONFINED: Fix is 1 function/module
  ✗ RIPPLES: Verify broader impact

→ ALL PASS: Implement fix
→ ANY FAIL: Loop back, gather missing info
```

**Example (Typo):**
```
Gate 1 ✓ Error: "consoleLog is not defined"
Gate 2 ✓ Grounded: Obvious typo
Gate 3 ✓ Testable: Run code again
Gate 4 ✓ Confined: Single line change
→ FIX: Change consoleLog to console.log
```

**Overhead:** Minimal. Looping rare. 80 tokens total.

---

## MEDIUM MODE: Flexible Checkpoints

**When:** Bug cause unclear, multiple possible angles, need exploration.

**How:** Suggested checkpoints, looping allowed, confidence gates.

```
CHECKPOINT 1: Signal Gathering
  Collect: error, when, environment, recent changes
  Confidence check: (LOW/MEDIUM/HIGH)
    HIGH → Hypothesis
    MEDIUM → Gather more
    LOW → Ask user

CHECKPOINT 2: Hypothesize
  Propose: root cause theory based on signals
  Testable: yes/no
  Confidence check: (LOW/MEDIUM/HIGH)
    HIGH → Test
    MEDIUM/LOW → Refine or loop back to signals

CHECKPOINT 3: Test
  Run test / reproduce
  Hypothesis: proven/disproven/inconclusive
    PROVEN → Fix
    DISPROVEN → Loop to checkpoint 2 (new hypothesis)
    INCONCLUSIVE → Loop to checkpoint 1 (more signals)

CHECKPOINT 4: Implement & Verify
  Existing tests pass? New test passes?
    YES → Done
    NO → Loop to checkpoint 2
```

**Example (Slow API):**
```
Checkpoint 1: Memory climbs, latency worsens over hours. Confidence: MEDIUM
  → Gather more signals → Session leak found

Checkpoint 2: Sessions not cleaned on logout. Confidence: HIGH → Test

Checkpoint 3: Load test proves leak (100→10K sessions). Hypothesis PROVEN → Fix

Checkpoint 4: Add Session.cleanup(). Tests pass. Done.
```

**Overhead:** Moderate. 200-250 tokens. Multiple checkpoint visits normal (not failure).

---

## STRICT MODE: Mandatory Phases

**When:** Hard bugs, production incidents, previous fixes failed.

**How:** 4 mandatory sequential phases. No skipping.

```
PHASE 1: Signal Gathering (MANDATORY)
  Extract 3+ independent error signals
  → Error message + stack trace
  → When it happens (reproducible steps)
  → What changed (git diff, version changes)
  → Environment (OS, versions, config)
  → Scope (one user? system-wide?)
  Can't proceed until complete.

PHASE 2: Hypothesize (MANDATORY)
  One hypothesis: root cause theory
  Cite signals from Phase 1
  State what would prove/disprove it
  Can't proceed until testable.

PHASE 3: Test (MANDATORY)
  Write test or use existing
  Run against current code → FAIL
  Run against hypothesis fix → PASS
  Can't proceed until test proves hypothesis.

PHASE 4: Implement (MANDATORY)
  Code fix (confirmed by Phase 3)
  Verify all tests pass
  Confirm scope (what changed, what's covered)
  Done.
```

**Example (Race Condition):**
```
PHASE 1: Collect signals → Error (10% requests fail), Pattern (concurrent requests), 
Changed (caching added week ago)

PHASE 2: Hypothesis → Cache layer race condition on concurrent uncached keys

PHASE 3: Test → Load test confirms (10% fail → 0% after fix)

PHASE 4: Fix → Add mutex lock to cache layer. Tests pass.
```

**Overhead:** High. 300+ tokens. But prevents 3-5 failed attempts = 700+ tokens wasted. Net savings.

---

## Rules (All Intensities)

- **No blind guessing.** Light mode gates it. Medium/Strict expose it.
- **Show your work.** User sees checkpoints/gates/phases passed.
- **Looping OK.** Not failure. It's how you refine.
- **When in doubt, escalate.** Light stuck? → Medium. Medium stuck? → Strict.

---

## Session Behavior

Once discipline-mode is active:
- Auto-detect intensity based on bug description (or user specifies)
- Show gates/checkpoints/phases explicitly
- Stay in mode until bug fixed or user says "stop"
- Allow mid-session escalation ("switch to strict mode")

---

## Rules Summary

- No guessing. Assert / checkpoint / phase gates prevent it.
- Show work. User sees gates/checkpoints/phases.
- Looping OK. Expected, not failure.
- Escalate if stuck. Light → Medium → Strict.
- Never compress: error messages, test output, code readability, reasoning.
