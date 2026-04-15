---
name: plan-mode
description: >
  Structured, token-efficient planning and solution design for any topic or problem.
  Trigger this skill whenever the user says "plan mode", "plan this", "design a solution",
  "think through", "blueprint", "architect a solution", "help me plan", "create a plan for",
  "break this down", or anything asking for a structured design or planning output — even if
  the phrasing is casual like "how would you approach X?" or "walk me through building Y".
  Produces a compressed, layered plan using a strict output format that maximises information
  density and minimises token waste. Always prefer this skill over open-ended prose planning.
---

# Plan Mode

A skill for producing **dense, structured, token-efficient plans** for any topic, problem, or system.

---

## Core Philosophy

Plans must be **maximally dense** — every token earns its place.

- No filler prose, no restating the question
- Use symbols over words where meaning is clear
- Nest structure via indentation, not repeated headers
- Layer from macro → micro (strategy → tactics → steps)
- Flag unknowns and decisions explicitly, don't hide them in prose

---

## Output Format (strict)

Emit plans in this exact skeleton. Omit any section that doesn't apply.

```
## PLAN: <title> [<scope-tag>]

### GOAL
<1-2 lines: what success looks like>

### CONSTRAINTS
- <hard constraint>
- <assumption>

### PHASES
[Phase labels: optional, use only if sequence matters]

#### 1. <Phase Name>
  OBJ: <what this phase achieves>
  → <step or decision>
  → <step or decision>
    ↳ <sub-step if needed>
  ⚠ <risk or open question>

#### 2. <Phase Name>
  ...

### DECISIONS
| # | Decision | Options | Rec |
|---|----------|---------|-----|
| 1 | <choice> | A / B   | A   |

### OPEN Qs
- <unresolved question that blocks progress>

### METRICS
- <how you'll know it's working>
```

---

## Symbol Legend (use consistently)

| Symbol | Meaning |
|--------|---------|
| `→` | Step / action |
| `↳` | Sub-step / detail |
| `⚠` | Risk / watch-out |
| `✓` | Done / verified |
| `?` | Unknown / needs input |
| `[P1]` `[P2]` | Priority 1, 2… |
| `[~Xh]` | Estimated time |
| `[dep: Y]` | Depends on Y |

---

## Compression Rules

1. **No preamble.** Start with `## PLAN:` immediately.
2. **Verb-first steps.** Write "Define schema" not "You will need to define the schema".
3. **Merge trivial steps.** Combine steps that always co-occur.
4. **Inline small decisions.** If a decision is obvious, make it inline and note rationale in parens.
5. **One-word phase labels** where possible: Setup, Build, Test, Ship, Monitor.
6. **Collapse linear chains.** Use `A → B → C` on one line if no branching.
7. **DECISIONS table only** for genuinely contested choices with ≥2 viable options.
8. **METRICS section only** if the plan has measurable outcomes.

---

## Calibration by Scope

| Scope tag | Phases | Steps per phase | Decisions |
|-----------|--------|----------------|-----------|
| `[quick]` | 1–2    | 2–4            | 0–1       |
| `[med]`   | 2–4    | 3–6            | 1–3       |
| `[deep]`  | 4–8    | 4–10           | 2–5       |

Infer scope from topic complexity. When unsure, start `[med]` and note "expand with `plan deep`".

---

## Interaction Patterns

- If the user says **"plan deep"** → expand to `[deep]` scope, add sub-steps and full DECISIONS table
- If the user says **"plan quick"** → collapse to `[quick]`, remove DECISIONS/METRICS
- If the user says **"drill into <phase>"** → emit only that phase at full depth
- If the user gives feedback → revise in-place, don't restate unchanged sections
- After emitting a plan, offer: `→ drill into a phase | → plan deep | → export as checklist`

---

## Example (abbreviated)

```
## PLAN: Launch a B2B SaaS MVP [med]

### GOAL
Ship a working product to 5 paying beta customers within 8 weeks.

### CONSTRAINTS
- Solo founder, part-time (~20h/wk)
- Budget: $0 infra until revenue

### PHASES

#### 1. Validate
  OBJ: Confirm willingness-to-pay before building
  → 10 discovery calls → distil top 3 pain points
  → Build "concierge MVP" (manual fulfilment, no code)
  → Close 2 verbal commitments [~1wk]
  ⚠ Pivot trigger: <2 commitments after 15 calls

#### 2. Build
  OBJ: Minimum shippable product
  → Define data model (use Supabase, free tier) → scaffold Next.js app
  → Implement core loop only [dep: Validate]
  → Auth → billing (Stripe) → core feature [~3wk]

#### 3. Ship
  OBJ: Get to first paid invoice
  → Onboard beta customers (white-glove)
  → Instrument basic analytics (PostHog free)
  → Weekly check-in cadence [~2wk]

### DECISIONS
| # | Decision | Options | Rec |
|---|----------|---------|-----|
| 1 | Auth provider | Clerk / Supabase Auth | Clerk (faster DX) |
| 2 | Pricing | Flat $99/mo / Usage | Flat (simpler) |

### METRICS
- Activation: user completes core flow within 10 min
- Retention: 3 of 5 beta users active at week 4
```