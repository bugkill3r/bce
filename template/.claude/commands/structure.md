---
description: Create a phased breakdown of the work — distinguish design from implementation
---

# Structure — Phase the Work

You are tasked with taking research findings and design decisions and producing a phased breakdown of the work. This step bridges the gap between "what we know" (research) and "what we'll do" (plan).

## Why This Step Exists

Plans that jump straight from research to implementation details often fail because they conflate WHERE we're going (design) with HOW we get there (steps). Structure separates these concerns.

## When This Command Is Invoked

If a research document or decisions are provided, read them fully and begin immediately.

Otherwise respond with:
```
I'll help structure this work into phases. Please provide:
1. The research document from /research (or paste key findings)
2. Design decisions from /question (if applicable)
3. Any constraints on ordering or dependencies

I'll create a phased breakdown for your review before we detail the plan.
```

Then wait for the user's input.

## Process

### Step 1: Absorb Context

- Read the research document fully
- Review any design decisions made during /question
- Understand the current state and desired end state
- Identify the delta — what needs to change

### Step 2: Identify Natural Phases

Break the work into phases that are:

- **Independently testable** — each phase produces a verifiable result
- **Incrementally valuable** — earlier phases don't depend on later ones
- **Appropriately scoped** — a phase should be completable in one focused session
- **Logically ordered** — dependencies flow forward, not backward

Common phase patterns:
- **Database changes** → Schema/migration → Store methods → Business logic → API → Client
- **New features** → Data model → Backend logic → API endpoints → UI
- **Refactoring** → Document current behavior → Incremental changes → Migration → Cleanup

### Step 3: Define the Structure

Present the structure for review:

```markdown
# Structure: [Feature/Task Name]

## Current State
[1-2 sentences on what exists now, from research]

## Desired End State
[1-2 sentences on what should exist after implementation]
[How to verify we got there]

## What We're NOT Doing
- [Explicitly list out-of-scope items]
- [Things that might seem related but aren't part of this work]
- [Future enhancements to defer]

## Phases

### Phase 1: [Name] — [What It Accomplishes]
**Goal:** [One sentence]
**Touches:** [List of files/areas]
**Depends on:** Nothing (first phase)
**Verifiable by:** [How we know it's done — commands, behaviors]

### Phase 2: [Name] — [What It Accomplishes]
**Goal:** [One sentence]
**Touches:** [List of files/areas]
**Depends on:** Phase 1
**Verifiable by:** [How we know it's done]

### Phase 3: [Name] — [What It Accomplishes]
...

## Risk Areas
- [Where things might get complicated]
- [Dependencies that could surprise us]
- [Assumptions that could be wrong]

## Open Questions
- [Anything that needs resolution before planning]
```

### Step 4: Get Feedback

Present the structure and ask:
- Are the phases properly scoped?
- Is the ordering right?
- Should anything be split or merged?
- Are the "not doing" items correct?

Do NOT proceed to detailed planning until the user approves the structure.

### Step 5: Resolve Open Questions

If open questions exist:
- Research them with sub-agents if they're code questions
- Ask the user if they're design questions
- Do NOT carry open questions into the plan

## Important Guidelines

- **Keep it high-level.** This is architecture, not implementation details.
- **3-5 phases max.** If you need more, the task should be split into separate efforts.
- **Each phase is a checkpoint.** The human reviews after each phase.
- **The "not doing" section matters most.** It prevents scope creep during implementation.
- **Phases should be independently revertable.** If Phase 3 fails, Phases 1-2 should still be valuable.

## Output

The structure document feeds directly into `/plan`, which will add implementation details (specific file changes, code snippets, success criteria) to each phase.
