---
name: claude-code-token-optimization
description: >
  Reduce Claude Code token usage and API costs. Use this skill whenever the user wants
  to cut their Claude Code bill, reduce token consumption, optimize context usage, stay
  within usage limits, speed up responses, or audit why their sessions are expensive.
  Triggers on: "too expensive", "burning tokens", "hit my limit", "reduce cost", "optimize
  usage", "context too large", "running out of tokens", "Claude Code cost", "token usage",
  "usage limits", and any related cost or efficiency concern with Claude Code. Apply
  proactively whenever helping a user set up or improve a Claude Code project — even if
  they haven't asked about costs yet.
---

# Claude Code Token Optimization Skill

Help users cut Claude Code costs by auditing their setup and applying targeted fixes. Work
through the checklist below from highest to lowest impact.

---

## Step 1: Diagnose Current Usage

Before recommending changes, ask or check:

1. **Are they on API or subscription?**
   - API (pay-per-token): every optimization directly reduces their bill
   - Subscription (Pro/Max): optimizations help them stay within usage windows

2. **Where is the pain?**
   - Hitting rate/usage limits mid-session?
   - Surprised by monthly API costs?
   - Slow responses from bloated context?
   - Running automated pipelines that are expensive?

3. **Do they have a CLAUDE.md?** Ask to see it or check its size.

4. **Run `/cost` in their session** to get a baseline: `Session tokens: X input, Y output`

---

## Step 2: Apply Fixes — Highest Impact First

### 🔴 Critical (50–70% savings potential)

#### 1. Add a `.claudeignore` file
Prevents Claude from reading build artifacts, generated files, and dependencies during
codebase exploration. Create at project root:

```
node_modules/
dist/
build/
.next/
*.lock
package-lock.json
yarn.lock
*.min.js
*.min.css
target/
__pycache__/
.git/
coverage/
*.generated.*
vendor/
```

Adjust for the project's language/framework. Every file Claude never reads saves tokens on
every exploration command.

#### 2. Trim CLAUDE.md ruthlessly
Every token in CLAUDE.md is paid for at the start of **every session**. Audit it for:
- Documentation Claude can find by reading source files → remove it
- Project history or changelog → remove it
- Verbose explanations of obvious things → shorten
- Anything not needed in 80%+ of sessions → move to a referenced file

**Target: under 200 lines.** For larger projects, split into domain-specific files (e.g.,
`CLAUDE.frontend.md`, `CLAUDE.api.md`) and instruct Claude to read only the relevant one.

#### 3. Disable unused MCP servers
Each enabled MCP server adds tool definitions to the system prompt — consumed every turn.
Check with `/mcp` and disable what's not in active use:
```
/mcp disable <server-name>
```

---

### 🟡 High Impact (20–40% savings)

#### 4. Use `/clear` between unrelated tasks
Claude re-processes entire conversation history on every message. Stale context from a
previous task wastes tokens on every subsequent turn.

**Workflow:**
```
/rename fix-auth-bug          # save session if you might resume it
/clear                        # fresh context for new task
```

#### 5. Use `/compact` for long sessions
When a session gets long (debugging, multi-file refactors), compact before continuing:
```
/compact Focus on the current implementation plan and key decisions made
```
Claude summarizes the conversation, preserving what matters and dropping the rest.

**Auto-compact triggers at 95% context capacity** — manual compaction at ~50-60% is better
because you control what gets preserved.

#### 6. Write specific, scoped prompts
Vague prompts force Claude to explore broadly. Specific prompts focus reads.

| Instead of | Use |
|---|---|
| `fix the bug` | `fix the null check in src/auth/login.ts line 47` |
| `make this better` | `optimize readability in api/handlers.js — extract constants, add JSDoc` |
| `update error handling` | `update error handling in auth.js, user.js, and api.js — batch these` |

**Batch related changes** into one prompt to maximize context efficiency.

#### 7. Use the right model for the task
Switch models via `/model`:

| Task type | Recommended model |
|---|---|
| Architecture decisions, complex refactors | `opus` |
| Standard coding, debugging, reviews | `sonnet` (default, 80% of tasks) |
| Simple tasks: linting, renaming, quick transforms | `haiku` |

Rule of thumb: **start every session on Sonnet, promote to Opus only when needed.**

---

### 🟢 Moderate Impact (10–20% savings)

#### 8. Truncate verbose command output
Shell command output enters the context window in full. Mitigate:
- Ask Claude to run `git log --oneline -10` instead of `git log`
- For test runs: `npm test 2>&1 | tail -50` instead of full output
- Use `--quiet` or `--silent` flags where available
- Add a PreToolUse hook to automatically truncate long outputs:

```json
// In .claude/settings.json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "echo 'Truncating output to 200 lines' && head -200"
      }]
    }]
  }
}
```

#### 9. Suppress non-essential background calls
Set this environment variable to disable background model calls for tips/suggestions:
```bash
export DISABLE_NON_ESSENTIAL_MODEL_CALLS=1
```
Add to your shell profile (`.bashrc` / `.zshrc`) to apply permanently.

#### 10. Use Plan Mode for expensive operations
Press **Shift+Tab twice** before any large operation. Claude outlines the plan before
writing code — you catch misunderstandings before they generate tokens of rework.

#### 11. Set automation safeguards
For CI/CD or agent loops, cap runaway sessions:
```json
// In headless/automation configs
{
  "max_turns": 20,
  "timeout_minutes": 10
}
```

---

## Step 3: Monitor After Changes

After applying fixes, establish a measurement habit:
- Run `/cost` at the end of sessions and note patterns
- Compare session costs for similar task types before/after
- Use `ccusage` (installable npm tool) for aggregate daily/monthly views:
  ```bash
  npm install -g ccusage
  ccusage daily
  ccusage monthly
  ccusage blocks --live   # real-time 5-hour window view
  ```

---

## Optimization by User Type

### API user (pay-per-token)
Priority order: `.claudeignore` → trim CLAUDE.md → model selection → `/clear` discipline → disable MCPs

### Subscription user hitting limits
Priority order: `/clear` discipline → `/compact` usage → trim CLAUDE.md → `.claudeignore` → Plan Mode

### Automation / CI pipelines
Priority order: set `max_turns` + `timeout_minutes` → use Haiku for routine tasks → batch prompts → truncate command output → disable MCPs not needed for automation

---

## Quick Reference Card

```
Daily habits:
  /clear          — fresh context for new tasks
  /compact        — summarize long sessions (do at ~50% context)
  /cost           — check token usage
  /model haiku    — switch to cheap model for simple tasks
  /model sonnet   — switch back to default

One-time setup:
  .claudeignore   — block build artifacts / deps from reads
  CLAUDE.md trim  — keep under 200 lines
  /mcp disable X  — turn off unused integrations
  
Environment:
  DISABLE_NON_ESSENTIAL_MODEL_CALLS=1  — suppress background calls
```

---

## Expected Savings by Fix

| Fix | Typical savings |
|---|---|
| `.claudeignore` (large project) | 30–50% input tokens |
| CLAUDE.md trim | 10–20% per session |
| `/clear` between tasks | 20–40% over a day |
| Model downgrade (Opus → Sonnet) | ~80% cost per token |
| Disable idle MCPs | 5–15% per turn |
| Specific prompts | 10–30% (less rework) |

A developer with good habits typically spends $5–15/day on API pricing. Without good habits,
the same work runs $20–40/day. These fixes close that gap.
