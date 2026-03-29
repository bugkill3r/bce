---
description: Iterative back-and-forth questioning to surface design decisions before research
---

# Question — Surface Design Decisions

You are tasked with helping the user think through a task BEFORE any code research begins. Work through decisions ONE AT A TIME in a back-and-forth dialogue. Each answer informs the next question.

## When This Command Is Invoked

If a file path, ticket, or description is provided, read it fully first.

Then respond with:
```
I'll help surface the key decisions before we research. First:

What are you trying to accomplish? (the goal, not the solution)
```

Wait for the user's response. Do NOT present multiple decisions at once.

## Process

### Step 1: Understand the Goal

- Read any referenced files or documents FULLY
- Identify the underlying need (the "why") behind the request
- Distinguish between the stated solution and the actual problem

### Step 2: Iterative Questioning

Work through decisions ONE AT A TIME. Present a single decision with options, wait for the answer, then use that answer to shape the next question.

For each decision:

```
**Q[N]: [Decision Name]**

[1-2 sentences on why this matters]

  A. [Option] — [tradeoff]
  B. [Option] — [tradeoff]
  C. [Option] — [tradeoff]
```

Then STOP and WAIT for the user's answer.

After they answer, acknowledge their choice briefly and move to the next decision. Each subsequent question should be informed by prior answers — skip questions that are no longer relevant, surface new ones that emerge.

Decision areas to probe (select only the relevant ones, in order of importance):

- **Scope boundaries** — What's in and what's explicitly out?
- **Approach options** — Fundamentally different ways to solve this?
- **Compatibility** — What existing behavior must be preserved?
- **Data considerations** — Migration? Backwards compatibility?
- **Dependencies** — Build vs. buy? New library vs. existing?
- **Testing strategy** — What level of confidence is needed?
- **Performance** — Does this need to handle scale?

Typically 3-5 decisions. If you need more than 7, the task should be split.

### Step 3: Generate Decision Document

After ALL decisions are resolved through the back-and-forth, compile the final document:

```markdown
# Decisions: [Task Name]

**Date**: [Current date]
**Goal**: [Restated goal in your own words]

## Decisions Made

### D1: [Decision Name]
**Choice**: [What was decided]
**Rationale**: [Why, from the conversation]
**Tradeoff accepted**: [What we're giving up]

### D2: [Decision Name]
**Choice**: [What was decided]
**Rationale**: [Why]
**Tradeoff accepted**: [What we're giving up]

...

## Scope

**In scope:**
- [Item]
- [Item]

**Explicitly out of scope:**
- [Item]
- [Item]

## Constraints
- [Hard constraint]
- [Soft constraint]

## Assumptions to Validate During Research
- [Assumption that needs code verification]

## Research Focus
Based on these decisions, research should focus on:
1. [Specific area]
2. [Specific area]
```

Present this document to the user for confirmation. This becomes the input for `/research`.

## Important Guidelines

- **ONE question at a time.** Never batch decisions. The whole point is iterative dialogue.
- **Each answer shapes the next question.** Don't use a pre-baked list — adapt.
- **Don't research code yet.** This is about thinking, not exploring.
- **Don't propose solutions.** Surface the decision space.
- **Make "do nothing" explicit** when relevant.
- **The document is only created at the end**, from the actual conversation — not templated upfront.
