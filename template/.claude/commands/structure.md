---
description: Create a phased breakdown — HOW we get to the design's end state
---

# Structure Outline — Phase the Work

You are tasked with taking the Design document and producing a phased breakdown. Design defined WHERE we're going. Structure defines HOW we get there — the ordering, the phases, the validation approach. ~2 pages.

## When This Command Is Invoked

If a design document is provided, read it fully and begin.

Otherwise respond with:
```
I'll help structure this work into phases. Please provide:
1. The design document from /design
2. Any constraints on ordering or dependencies

I'll create a phased breakdown for your review.
```

Then wait for the user's input.

## Process

### Step 1: Absorb the Design

- Read the design document fully
- Understand the current state and desired end state
- Note resolved decisions and constraints
- Identify the delta — what needs to change to get from current to desired

### Step 2: Identify Natural Phases

Break the work into phases that are:

- **Independently testable** — each phase produces a verifiable result
- **Incrementally valuable** — earlier phases don't depend on later ones
- **Appropriately scoped** — completable in one focused session
- **Logically ordered** — dependencies flow forward, not backward
- **Independently revertable** — if Phase 3 fails, Phases 1-2 still stand

Common patterns:
- **Database changes** → Schema/migration → Store methods → Business logic → API → Client
- **New features** → Data model → Backend logic → API endpoints → UI
- **Refactoring** → Document current behavior → Incremental changes → Migration → Cleanup

### Step 3: Write the Structure Outline

~2 pages covering:

```markdown
# Structure Outline: [Feature/Task Name]

**Design ref**: [path or link to design document]

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

## Validation Approach
- [How we validate each phase before moving on]
- [Automated checks]
- [Manual checks]

## Risk Areas
- [Where things might get complicated]
- [Dependencies that could surprise us]

## Phase Dependencies
[Any non-obvious ordering constraints]
```

### Step 4: Get Feedback

Present the structure and ask:
- Are the phases properly scoped?
- Is the ordering right?
- Should anything be split or merged?

Do NOT proceed to planning until the user approves.

## Important Guidelines

- **This is HOW, not WHAT.** The design already defined the end state. Don't re-debate it here.
- **3-5 phases max.** If you need more, the task should be split.
- **~2 pages max.** Keep it scannable.
- **Each phase is a checkpoint.** Human reviews after each.
- **No implementation details.** File-level changes and code snippets go in `/plan`.

## Output

The structure outline feeds into `/plan`, which adds implementation details (specific file changes, code snippets, success criteria) to each phase.
