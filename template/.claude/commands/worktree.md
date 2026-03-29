---
description: Set up an isolated git worktree for implementation — context isolation boundary
---

# Worktree — Isolate for Implementation

Set up a git worktree to create a clean, isolated environment for implementation. This is the context isolation boundary between planning and coding.

## Why This Step Exists

Worktrees give the implementation phase its own filesystem copy. Changes happen in isolation — no conflicts with your main branch, no risk of polluting other work. This is also a natural context reset point.

## When This Command Is Invoked

If a plan document and branch name are provided, proceed directly.

Otherwise respond with:
```
I'll set up a worktree for implementation. Please provide:
1. The plan document (path or paste)
2. A branch name (or I'll generate one from the task)
3. Base branch to fork from (default: main)
```

Then wait for the user's input.

## Process

### Step 1: Create the Worktree

```bash
# Create a new branch and worktree
git worktree add ../[repo]-[branch-name] -b [branch-name] [base-branch]
```

### Step 2: Set Up the Environment

In the new worktree:
- Install dependencies if needed
- Verify the build works from a clean state
- Confirm tests pass before any changes

### Step 3: Prepare for Implementation

Copy or reference the plan document so it's accessible in the new worktree context.

Present:
```
Worktree ready at: ../[path]
Branch: [branch-name]
Base: [base-branch]

Build: [passing/failing]
Tests: [passing/failing]

Plan loaded. Ready to run /implement.
```

## When to Use Worktrees vs Not

| Scenario | Worktree? |
|---|---|
| Quick single-file fix | No — just work on current branch |
| Multi-phase implementation from a plan | Yes |
| Parallel features by different agents | Yes — each gets own worktree |
| Research or planning phases | No — read-only, no filesystem changes |
| Experimental approach you might discard | Yes — easy cleanup |

## Cleanup

After merging or abandoning:
```bash
git worktree remove ../[path]
git branch -d [branch-name]  # if merged
```

## Important Guidelines

- **This is a context reset point.** Start the implementation phase with a fresh context window — just the plan and the clean worktree.
- **Verify before coding.** Build and tests should pass in the worktree before you start.
- **One worktree per task.** Don't reuse worktrees across different plans.
