---
name: autonomous-mode
description: >
  Self-directed execution: complete workflows, auto-solve problems, refactor code.
  No user input needed between steps. Triggers: "run autonomously", "auto-solve",
  "execute this end-to-end", "self-refactor", "just fix it", "autonomous workflow",
  "solve this without asking"
---

# Autonomous Mode

Execute complex tasks from start → finish without waiting for user input between steps.

## The Three Modes

**Workflow** — end-to-end task completion  
**Solver** — diagnose problems → auto-fix → validate  
**Refactor** — improve code structure while preserving behavior  

Mode is triggered automatically by context.

## Safety: Git Sandbox Mode

All changes execute on a **temporary git branch** (auto-created).

When ready to commit:
1. Autonomous execution completes
2. All changes staged on temp branch
3. **User reviews**: `git diff origin/main`
4. **User approves**: merge or `git commit` manually
5. Temp branch cleaned up

**Workaround for commits:**
```bash
# After autonomous execution, user runs:
git merge --no-ff <temp-branch> -m "your message"
# OR cherry-pick specific commits
# OR force-push if desired (your choice)
```

This is **actual git workflow**, not a hack. You stay in control.

## Intensity Levels

- **Light** — fix obvious bugs, run existing tests, small refactors
- **Medium** — refactor larger sections, add features, create new files
- **Strict** — can delete unused code, restructure modules, break APIs (⚠️ MAJOR WARNING required)

## Decision Framework

Before acting autonomously:

- **Will this break anything testable?** Run tests first. Don't commit broken code.
- **Is this idempotent?** Safe to retry if interrupted?
- **Can the user understand what I did?** Document choices via comments or commit messages.
- **Do I need approval?** Ask. Never assume.

## Guardrail Violations → Auto-Abort

Stop immediately if:
- Attempting `git push` / `git push --force`
- Deleting files without backup
- Modifying `.env`, `.secrets`, config files, credentials
- Executing destructive commands (`rm -rf`, etc.)
- Writing to databases without user permission
- Calling external APIs without explicit approval

When blocked: explain why, offer safe alternative.

## Workflow

1. **Intake**: Understand the task fully
2. **Plan**: Outline steps (show to user if complex)
3. **Execute**: Implement steps autonomously
4. **Validate**: Run tests, check for regressions
5. **Stage**: Create temp branch, stage changes
6. **Report**: Show what changed, why, next steps
7. **Wait**: User reviews git diff, decides to merge or cherry-pick

## What Autonomy Means

- You don't ask "should I do X?" in the middle of execution
- You do ask "can I do X safely?" before proceeding
- You document decisions so the user understands your reasoning
- You fail fast: if you hit a blocker, stop and explain

## What Autonomy Doesn't Mean

- Committing to main automatically ❌
- Pushing code without approval ❌
- Modifying configs or secrets ❌
- Executing commands that can't be undone ❌

---

**Autonomy is about removing friction, not removing control.**
