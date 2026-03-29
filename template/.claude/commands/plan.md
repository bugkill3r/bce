---
description: Create a detailed implementation plan with exact steps, file references, and success criteria
---

# Plan — Detail Exact Steps

You are tasked with creating a detailed implementation plan that is explicit enough for mechanical execution. A good plan references specific files, line numbers, and includes code snippets showing what will change. Someone reading it should know exactly what will happen before any code is written.

## When This Command Is Invoked

If a structure document, design document, research document, or ticket is provided, read them fully and begin immediately.

Otherwise respond with:
```
I'll help create a detailed implementation plan. Please provide:
1. The structure outline from /structure (or design from /design, or research from /research)
2. Any design decisions or constraints
3. Links to related tickets or documents

I'll analyze this and work with you to create a comprehensive, actionable plan.
```

Then wait for the user's input.

## Process

### Step 1: Context Gathering

1. **Read ALL referenced files completely** — never use limit/offset, always read the full file
2. **Read files yourself in main context BEFORE spawning sub-agents** — you need this context to write good sub-agent prompts
3. **Spawn research sub-agents** if you need to understand code not covered by prior research:
   - Use **codebase-locator** to find relevant files — be specific about directories
   - Use **codebase-analyzer** to understand implementation details
   - Use **pattern-finder** to find conventions to follow
4. **Read files identified by sub-agents** into your main context
5. **Cross-reference** requirements against actual code

Present your understanding with file:line references. Only ask questions you cannot answer through code investigation.

### Step 2: Verify and Discover

If the user corrects any misunderstanding:
- **DO NOT just accept the correction** — spawn new sub-agents to verify against actual code
- Read the specific files/directories they mention
- Only proceed once you've verified the facts yourself

If sub-agent results seem incomplete or wrong:
- Spawn follow-up sub-agents to investigate
- Cross-check findings — don't trust blindly
- Be EXTREMELY specific about directories in follow-up prompts

Wait for ALL sub-agents to complete before proceeding.

### Step 3: Plan Structure

If no structure document exists, create an outline proposing phases. Get user feedback before writing details.

Reference common implementation patterns:
- **Database changes:** Schema/migration → Store methods → Business logic → API → Client
- **New features:** Data model → Backend logic → API endpoints → UI
- **Refactoring:** Document current behavior → Incremental changes → Migration → Cleanup

### Step 4: Write the Plan

Write the plan with this structure:

```markdown
# [Feature/Task Name] — Implementation Plan

**Date**: [Current date]
**Branch**: [Current git branch]
**Based on**: [Research/design/structure doc references]

## Overview
[1-2 sentences: what we're implementing and why]

## Current State
[What exists now, key constraints from research]

## Desired End State
[Specification of the end state and how to verify it]

### Key Discoveries
- [Finding with file:line reference]
- [Pattern to follow]
- [Constraint to work within]

## What We're NOT Doing
- [Out-of-scope item 1]
- [Out-of-scope item 2]

## Implementation Approach
[High-level strategy and reasoning for the chosen approach]

---

## Phase 1: [Descriptive Name]

### Overview
[What this phase accomplishes]

### Changes Required

#### 1. [Component/File Group]
**File**: `path/to/file.ext`
**Changes**: [Summary]

```[language]
// Specific code to add/modify
// Include enough context to be unambiguous
```

#### 2. [Next Component]
...

### Success Criteria

#### Automated Verification
- [ ] Tests pass: `[test command]`
- [ ] Lint clean: `[lint command]`
- [ ] Build succeeds: `[build command]`
- [ ] [Specific check]: `[command]`

#### Manual Verification
- [ ] [Behavior to verify manually]
- [ ] [Edge case to test]
- [ ] [UX check if applicable]

**After automated verification passes, pause for human manual verification before proceeding to Phase 2.**

---

## Phase 2: [Descriptive Name]
[Same structure as Phase 1...]

---

## Testing Strategy

### Unit Tests
- [What to test]
- [Key edge cases]

### Integration Tests
- [End-to-end scenarios]

### Manual Testing Steps
1. [Specific step]
2. [Another step]

## Rollback Plan
[How to revert if something goes wrong]

## References
- Research: [link or path to research document]
- Design: [link or path to design document]
- Related: [file:line references]
```

### Step 5: Review and Iterate

Present the plan and ask:
- Are the phases properly scoped?
- Are success criteria specific enough?
- Any missing edge cases or considerations?
- Technical details that need adjustment?

Iterate until the user is satisfied.

## Important Guidelines

- **Read files FULLY** — never use limit/offset parameters
- **Read referenced files yourself BEFORE spawning sub-agents**
- **Don't accept corrections blindly** — verify against code
- **Verify sub-agent results** — spawn follow-ups if something seems off
- **Be specific about directories** in sub-agent prompts
- **Question vague requirements** — identify issues early
- **Don't write the full plan in one shot** — get buy-in at each major step
- **Include specific file paths and line numbers** for all claims
- **No open questions** — if questions arise, STOP and resolve before continuing
- **Always separate automated vs manual success criteria**
- **Manual verification items are NOT checked off until the human confirms**

## Output

The plan feeds directly into `/implement`, which executes it phase by phase with verification at each step.
