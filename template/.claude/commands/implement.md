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
- Check for any existing checkmarks (completed work)
- Read ALL files mentioned in the plan
- Read files FULLY — never partially
- Create a mental model of how the pieces fit together

If resuming, trust that completed phases are done. Pick up from the first unchecked item.

### Step 2: Implement Phase by Phase

For each phase:

1. **Implement the changes** as specified in the plan
2. **Follow the plan's intent** while adapting to what you actually find in the code
3. **Run automated success criteria** after completing the phase
4. **Fix any issues** before proceeding
5. **Check off completed items** in the plan file using Edit

### Step 3: Pause for Human Verification

After completing all automated verification for a phase:

```
Phase [N] Complete — Ready for Manual Verification

Automated verification passed:
- [x] Tests pass: `npm test` ✓
- [x] Lint clean: `npm run lint` ✓
- [x] Build succeeds: `npm run build` ✓

Please perform the manual verification steps from the plan:
- [ ] [Manual check 1]
- [ ] [Manual check 2]

Let me know when manual testing is complete so I can proceed to Phase [N+1].
```

Do NOT check off manual verification items until confirmed by the user.

If instructed to execute multiple phases consecutively, skip the pause until the last phase.

### Step 4: Handle Mismatches

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
- Research summary: ~200-500 tokens (compressed from prior phases)
- Plan: ~500-1000 tokens
- Active code: whatever the current phase needs

This leaves most of the context window in the "smart zone" for actual coding.

If context grows large during implementation:
1. Summarize progress into markdown
2. Note what's done and what's next
3. Start fresh with just the summary and remaining plan

## Sub-Agent Usage

Use sub-agents sparingly during implementation — mainly for:
- Targeted debugging when something isn't working
- Exploring unfamiliar code not covered by research
- Running verification across multiple files

## Important Notes

- **Don't skip verification.** Run the checks even if you're confident.
- **Don't expand scope.** If you notice something that should change but isn't in the plan, flag it — don't fix it.
- **Don't rush phases.** Each phase should be solid before moving on.
- **Maintain forward momentum.** You're implementing a solution, not just checking boxes.
