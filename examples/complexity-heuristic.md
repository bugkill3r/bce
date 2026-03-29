# Complexity Heuristic — When to Use What

Match context engineering effort to task complexity. Over-engineering simple tasks wastes time. Under-engineering complex tasks produces slop.

## The Heuristic

| Complexity | Signals | Workflow | Time Overhead |
|---|---|---|---|
| **Trivial** | Single file, obvious change, no dependencies | Direct chat | 0 min |
| **Simple** | Single concern, 1-3 files, clear pattern to follow | `/plan` → `/implement` | ~10 min |
| **Medium** | Multi-file, single subsystem, some ambiguity | `/research` → `/plan` → `/implement` | ~30 min |
| **Complex** | Multi-subsystem, architectural decisions needed | `/question` → `/research` → `/structure` → `/plan` → `/implement` | ~45-60 min |
| **Extremely Complex** | Novel architecture, major refactor, unknown territory | Whiteboard first, then full QRSPI | 60+ min |

## How to Assess Complexity

Ask yourself:

1. **How many files will this touch?**
   - 1-3 files → Simple
   - 4-10 files → Medium
   - 10+ files → Complex

2. **Are there design decisions to make?**
   - No, pattern is obvious → Simple
   - One or two choices → Medium
   - Multiple tradeoffs → Complex (needs /question)

3. **How well do you understand the affected code?**
   - Wrote it yourself → Drop one level
   - Read it recently → Stay same level
   - Never seen it → Bump up one level

4. **Is this greenfield or brownfield?**
   - New code, no dependencies → Drop one level
   - Extending existing patterns → Stay same level
   - Modifying core behavior → Bump up one level

5. **What's the blast radius if it goes wrong?**
   - Easily reverted → Drop one level
   - Needs migration/rollback plan → Bump up one level

## Examples

| Task | Assessment | Workflow |
|---|---|---|
| Fix a typo in an error message | Trivial — 1 file, obvious | Direct chat |
| Add a new field to an API response | Simple — clear pattern, 2-3 files | `/plan` → `/implement` |
| Add pagination to a list endpoint | Medium — touches handler, service, store, tests | `/research` → `/plan` → `/implement` |
| Add OAuth2 provider integration | Complex — design decisions, multi-subsystem, security implications | Full QRSPI |
| Extract microservice from monolith | Extremely complex — architectural, high blast radius | Whiteboard → QRSPI |
| Change button color | Trivial | "Make the submit button blue" |
| Add dark mode | Medium — touches many files but follows a pattern | `/research` → `/plan` → `/implement` |

## Common Mistakes

**Over-engineering:** Running full QRSPI for a simple bug fix. The overhead isn't worth it — just fix the bug.

**Under-engineering:** Chatting directly with the agent about a multi-service architectural change. This produces slop that senior engineers have to clean up.

**Skipping research for unfamiliar code:** "I'll just look at it as I go" — this fills the context window with exploration noise and puts you in the dumb zone.

**Skipping the plan for medium tasks:** These are the tasks that seem simple but aren't. A 5-minute plan saves 30 minutes of rework.

## Calibration

It takes reps to calibrate. You'll go too big sometimes and too small other times. That's fine. The cost of over-engineering is wasted time. The cost of under-engineering is slop and rework.

When in doubt, err on the side of one level more structure — a plan you didn't need costs 10 minutes, a rewrite costs hours.
