---
description: Iterate on an existing plan based on feedback or changed requirements
---

# Iterate Plan

You are tasked with updating an existing implementation plan based on user feedback. Be skeptical, thorough, and ensure changes are grounded in actual codebase reality.

## When This Command Is Invoked

Parse the input to identify:
- Plan content or file path
- Requested changes/feedback

If no plan provided, ask for one. If plan provided but no feedback, ask what to change. If both provided, proceed immediately.

## Process

### Step 1: Read and Understand

- Read the existing plan COMPLETELY
- Understand the current structure, phases, and scope
- Understand the requested changes
- Determine if changes require new codebase research

### Step 2: Research If Needed

Only spawn research sub-agents if changes require new technical understanding:
- Use **codebase-locator** to find relevant files
- Use **codebase-analyzer** to understand implementation details
- Use **pattern-finder** to find conventions

Wait for ALL sub-agents to complete before proceeding.

### Step 3: Confirm Understanding

Before making changes:
```
Based on your feedback, I understand you want to:
- [Change 1]
- [Change 2]

My research found:
- [Relevant discovery]

I plan to update the plan by:
1. [Specific modification]
2. [Another modification]

Does this align with your intent?
```

Get confirmation before proceeding.

### Step 4: Make Surgical Edits

- Use precise edits, not wholesale rewrites
- Preserve good content that doesn't need changing
- Maintain the existing structure unless explicitly changing it
- Keep all file:line references accurate
- Update success criteria if needed
- Maintain automated vs manual verification distinction

Ensure consistency:
- New phases follow the existing pattern
- "What We're NOT Doing" is updated if scope changes
- "Implementation Approach" is updated if approach changes

### Step 5: Present Changes

```
I've updated the plan. Changes made:
- [Change 1]
- [Change 2]

Would you like any further adjustments?
```

## Guidelines

- **Be surgical** — precise edits, not rewrites
- **Be skeptical** — question changes that seem problematic
- **No open questions** — research or ask before updating
- **Verify feasibility** — don't accept changes without checking code
