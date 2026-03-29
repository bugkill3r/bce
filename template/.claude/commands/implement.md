---
description: Execute an approved plan phase by phase with verification at each step
---

# Implement — Execute the Plan

You are tasked with implementing an approved plan. The plan contains phases with specific changes and success criteria. Your job is to execute faithfully, verify your work, and pause for human review at phase boundaries.

## When This Command Is Invoked

If a plan path or plan content is provided, read it completely and begin.

Otherwise respond with:
```
I'll help implement an approved plan. Please provide:
1. The plan (paste it or provide a file path)
2. Which phase to start from (default: Phase 1)

I'll execute each phase, run verification, and pause for your review.
```

Then wait for the user's input.

## Process

### Step 1: Read and Understand

- Read the plan completely
- Check for any existing checkmarks `- [x]` (completed work)
- Read ALL files mentioned in the plan — read them FULLY, never use limit/offset
- Create a mental model of how the pieces fit together

If resuming from a partially completed plan:
- Trust that checked-off phases are done
- Pick up from the first unchecked item
- Only verify previous work if something seems off

### Step 2: Implement Phase by Phase

For each phase:

1. **Implement the changes** as specified in the plan
2. **Follow the plan's intent** while adapting to what you actually find in the code
3. **Run automated success criteria** after completing the phase
4. **Fix any issues** before proceeding
5. **Update the plan file** — check off completed items using Edit so progress is tracked

### Step 3: Pause for Human Verification

After completing all automated verification for a phase:

```
Phase [N] Complete — Ready for Manual Verification

Automated verification passed:
- [x] Tests pass: `[command]` ✓
- [x] Lint clean: `[command]` ✓
- [x] Build succeeds: `[command]` ✓

Please perform the manual verification steps from the plan:
- [ ] [Manual check 1]
- [ ] [Manual check 2]

Let me know when manual testing is complete so I can proceed to Phase [N+1].
```

**Do NOT check off manual verification items until confirmed by the user.** Only the human can verify manual items.

If instructed to execute multiple phases consecutively, skip the pause until the last phase.

### Step 4: After-Action Report

After completing each phase (before starting the next), briefly document:

```
### Phase [N] After-Action
- **What worked:** [What went as planned]
- **What didn't:** [Deviations, surprises, issues encountered]
- **Lesson for next phase:** [Anything the next phase should know]
```

This carries lessons forward and prevents repeating mistakes across phases.

### Step 5: Handle Mismatches

When the code doesn't match the plan:

```
Issue in Phase [N]:
Expected: [What the plan says]
Found: [Actual situation in the code]
Why this matters: [Explanation]

Options:
A. [Adaptation that preserves the plan's intent]
B. [Alternative approach]
C. [Skip and flag for plan revision]

How should I proceed?
```

STOP and communicate clearly. Don't silently deviate from the plan.

## Implementation Philosophy

- **Follow the plan's intent** while adapting to reality
- **Implement each phase fully** before moving to the next
- **Verify your work** makes sense in the broader codebase
- **The plan is your guide**, but your judgment matters too
- **When in doubt, ask** — don't guess

## Context Management

Implementation should have a SMALL context window:
- Plan: ~500-1000 tokens
- Active code: whatever the current phase needs

This leaves most of the context window in the "smart zone" for actual coding.

If context grows large during implementation:
1. Summarize progress into markdown (what's done, what's next, any issues)
2. Start fresh with just the summary and remaining plan
3. This is intentional compaction — same principle as between phases

## Important Notes

- **Read files FULLY** — never use limit/offset parameters
- **Update the plan file as you go** — check off items so progress is resumable
- **Don't check off manual items** — only the human can verify those
- **Don't skip verification** — run checks even if you're confident
- **Don't expand scope** — if you notice something outside the plan, flag it, don't fix it
- **Don't rush phases** — each phase should be solid before moving on
- **Write after-action reports** — lessons carry forward to the next phase
- **Don't silently deviate** — if reality doesn't match the plan, stop and communicate
