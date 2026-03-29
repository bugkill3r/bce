# The Leverage Hierarchy

Where to focus human effort for maximum impact.

## The Hierarchy

```
Layer 1: CLAUDE.md / .cursorrules          ← Highest leverage
Layer 2: Persistent memory / auto memory
Layer 3: Hooks
Layer 4: Prompts / slash commands
Layer 5: Research
Layer 6: Plans
Layer 7: Implementation                    ← Lowest leverage
```

## Why This Order Matters

### Layer 1: CLAUDE.md

**Set once, affects every task.**

A well-written CLAUDE.md shapes every conversation the agent has with your codebase. It's the equivalent of telling a new hire your non-negotiables on day one.

**Worth investing in:** Tribal knowledge that lives nowhere in the code — architecture decisions, conventions that aren't obvious, things you'd repeat to every new team member.

**Not worth investing in:** Things the agent can derive from code (import patterns, file structure) or git history (recent changes).

The ETH Zurich study (Gloaguen et al., 2026) found that CLAUDE.md files only add value when they contain information agents can't find elsewhere. LLM-generated files actually reduced success by 3%.

### Layer 2: Persistent Memory

**Refines behavior over weeks.**

Auto memory captures corrections and preferences across sessions: "don't add attribution lines to commits", "use absolute imports", "run integration tests before pushing."

CLAUDE.md handles stable conventions. Memory handles emergent patterns from actual collaboration.

**Limitation:** Memory drift — corrections from one codebase become wrong assumptions in another. No expiry mechanism. Agents carry forward preferences from three projects ago.

### Layer 3: Hooks

**Automate context decisions previously made manually.**

Examples:
- Path-based context injection (opening files in `pkg/auth/` auto-loads security conventions)
- Pre-commit reminders (run tests first)
- Convention validation at tool-use time

**Risk:** Hooks injecting excessive context push agents into the dumb zone. Start with one or two solving real problems.

### Layer 4: Prompts / Slash Commands

**Shape each session.**

The QRSPI commands (`/question`, `/research`, `/structure`, `/plan`, `/implement`) are prompts that enforce a workflow. They ensure consistent, reviewable output regardless of who runs them.

Good prompts encode the process. They don't need to be clever — they need to be systematic.

### Layer 5: Research

**Compress truth from live code.**

Research produces the seed artifact for everything downstream. Bad research → bad plan → bad code. This is where correctness matters most.

A misunderstanding at the research level sends the entire task in the wrong direction. That's hundreds of bad lines of code.

### Layer 6: Plans

**Compress intent into actionable steps.**

Plans are how teams maintain mental alignment at scale. You can read plans all week. You can't read thousands of lines of AI-generated code.

A bad line in a plan becomes 100 bad lines of code. But plans are cheaper to review than code — they're shorter, more structured, and explicitly state what will happen.

### Layer 7: Implementation

**Mechanical execution.**

By the time you reach implementation, three layers of convention, four layers of process, and two compressed artifacts are already working. Implementation should be almost mechanical.

Reviewing implementation diffs is lowest-leverage. Not useless — but if you skipped the 10 minutes reviewing the plan, those hours reviewing code won't catch architectural errors.

## Error Amplification

```
Bad CLAUDE.md entry      → Wrong convention in every task
Bad memory entry         → Repeated mistakes across sessions
Bad hook                 → Incorrect context injected silently
Bad prompt               → Poorly structured workflow
Bad research finding     → Thousands of bad lines
Bad plan line            → Hundreds of bad lines
Bad implementation line  → One bad line
```

## Practical Advice

1. **Spend 2 hours on CLAUDE.md.** It pays off every session.
2. **Review auto memory monthly.** Clean stale entries.
3. **Start with zero hooks.** Add only when you see a repeated manual pattern.
4. **Use the template commands as-is first.** Customize after you understand the workflow.
5. **Always review research and plans.** This is where your time has the most impact.
6. **Don't review every line of AI code.** Review the plan that produced it.
