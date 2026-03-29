---
description: Create a detailed implementation plan with exact steps, file references, and success criteria
---

# Plan — Detail Exact Steps

You are tasked with creating a detailed implementation plan that is explicit enough for mechanical execution. A good plan references specific files, line numbers, and includes code snippets showing what will change. Someone reading it should know exactly what will happen before any code is written.

## When This Command Is Invoked

If a structure document, research document, or ticket is provided, read them fully and begin immediately.

Otherwise respond with:
```
I'll help create a detailed implementation plan. Please provide:
1. The structure document from /structure (or research from /research)
2. Any design decisions or constraints
3. Links to related tickets or documents

I'll analyze this and work with you to create a comprehensive, actionable plan.
```

Then wait for the user's input.

## Process

### Step 1: Context Gathering

1. **Read ALL referenced files completely** — no partial reads
2. **Spawn research sub-agents** if you need to understand code not covered by prior research:
   - Use **codebase-locator** to find relevant files
   - Use **codebase-analyzer** to understand implementation details
   - Use **pattern-finder** to find conventions to follow
3. **Read files identified by sub-agents** into your main context
4. **Cross-reference** requirements against actual code

Present your understanding with file:line references. Only ask questions you cannot answer through code investigation.

### Step 2: Research & Discovery

If the user corrects any misunderstanding:
- DO NOT just accept the correction — spawn new sub-agents to verify
- Read the specific files/directories they mention
- Only proceed once you've verified the facts yourself

Spawn parallel sub-agents for concurrent investigation. Wait for ALL to complete before proceeding.

Present findings with design options if applicable.

### Step 3: Plan Structure

If no structure document exists, create an outline proposing phases. Get user feedback before writing details.

### Step 4: Write the Plan

Write the plan with this structure:

```markdown
# [Feature/Task Name] — Implementation Plan

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

**Pause here for manual verification before proceeding to Phase 2.**

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

### Be Skeptical
- Question vague requirements
- Identify potential issues early
- Don't assume — verify with code

### Be Interactive
- Don't write the full plan in one shot
- Get buy-in at each major step
- Allow course corrections

### Be Thorough
- Read all context files COMPLETELY
- Research actual code patterns via parallel sub-agents
- Include specific file paths and line numbers
- Write measurable success criteria

### Be Practical
- Focus on incremental, testable changes
- Consider migration and rollback
- Think about edge cases
- Include "what we're NOT doing"

### No Open Questions
- If you encounter open questions, STOP
- Research or ask for clarification immediately
- DO NOT write the plan with unresolved questions
- Every decision must be made before finalizing

## Success Criteria Format

Always separate into two categories:

**Automated Verification** — commands that can be run:
```
- [ ] Tests pass: `npm test`
- [ ] Lint clean: `npm run lint`
- [ ] Type check: `npm run typecheck`
- [ ] Build: `npm run build`
```

**Manual Verification** — requires human testing:
```
- [ ] Feature works correctly in UI
- [ ] Performance acceptable under load
- [ ] Error messages are user-friendly
- [ ] No regressions in related features
```

## Output

The plan feeds directly into `/implement`, which executes it phase by phase with verification at each step.
