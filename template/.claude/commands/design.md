---
description: Define the desired end state — WHERE we're going, not how we get there
---

# Design — Define the End State

You are tasked with taking research findings and design decisions and producing a Design Discussion document. This defines WHERE we're going — the desired end state — separate from HOW we get there (that's `/structure`).

## When This Command Is Invoked

If a research document or decisions document is provided, read them fully and begin.

Otherwise respond with:
```
I'll help define the design for this work. Please provide:
1. The research document from /research (or paste key findings)
2. Design decisions from /question (if applicable)

I'll draft a design discussion for your review.
```

Then wait for the user's input.

## Process

### Step 1: Absorb Context

- Read research and decision documents fully
- Understand what exists today
- Understand what needs to change and why

### Step 2: Draft Design Discussion

Produce a document (~200 lines) covering:

```markdown
# Design: [Feature/Task Name]

**Date**: [Current date]

## Current State
[What exists today — summarize from research. Include key file:line references.]

## Desired End State
[What should exist after this work is complete. Be specific and concrete.]
[How do we verify we got there — what does "done" look like?]

## Codebase Patterns to Follow
[Existing patterns from research that this work should follow]
- [Pattern]: found in `file:line` — [how it applies here]
- [Convention]: [how we'll follow it]

## Design Decisions (Resolved)
[From /question phase — document what was decided and why]

### D1: [Decision]
**Choice**: [What]
**Why**: [Rationale]

### D2: [Decision]
...

## Open Questions
[Anything still unresolved that must be answered before structuring]

## What We're NOT Doing
- [Explicitly out of scope]
- [Related work to defer]

## Risks & Unknowns
- [Things that could go wrong]
- [Assumptions we're making]
```

### Step 3: Review with User

Present the design and ask:
- Does the end state match your intent?
- Are the resolved decisions captured correctly?
- Any open questions we need to resolve now?

Do NOT proceed until open questions are resolved. Research further or ask the user.

### Step 4: Finalize

Once approved, the design document feeds into `/structure` which will define the phased breakdown of HOW to get to this end state.

## Important Guidelines

- **This is WHAT, not HOW.** Don't include implementation steps, phase ordering, or file-change lists.
- **Ground everything in research.** Every claim about current state should reference actual code.
- **Resolve open questions here.** Don't carry them into structure or planning.
- **~200 lines max.** This is a discussion document, not a specification.
