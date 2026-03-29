# The QRSPI Framework

## Evolution: From RPI to QRSPI

The original Research-Plan-Implement (RPI) workflow — introduced by Dex Horthy at HumanLayer — proved effective but had failure modes in practice:

**RPI's failures** (from Dex's 6-month post-mortem, "Everything We Got Wrong About Research-Plan-Implement", Coding Agents 2026):
- The research step used a single sub-agent with a broad, open-ended prompt, consuming ~40% of context just for orientation
- The planning prompt contained **85+ instructions** — well over the reliable budget
- Critical interactive steps (e.g., "work back and forth with me") were buried and frequently skipped
- About half of users received finished plans with predetermined decisions instead of collaborative ones
- Teams pasted entire tickets into research rather than asking targeted questions, producing opinions instead of facts

**The instruction budget:** Frontier LLMs can reliably follow only ~150-200 instructions. When you combine system prompts, planning prompts, tool definitions, and MCP servers, models become "over budget" and silently drop requirements. This is why QRSPI splits one 85-instruction prompt into five prompts of <40 instructions each.

**Code review reversal:** Dex initially advocated reviewing plans instead of code. After 6 months, he revised: reading a 1,000-line plan still produces ~1,000 lines of code — review both. "Please read the code. It's the same amount of work."

The team's initial expansion was **QRDSPWIP** (Question, Research, Design, Structure, Plan, Worktree, Implement, Publish) — which was too long to remember. They simplified to **QRSPI**: Question, Research, Structure, Plan, Implement.

## The Five Phases

### Q — Question

**Purpose:** Surface implicit design decisions BEFORE investing time in research.

Most failed implementations trace back to misunderstood requirements, not bad code. This phase makes the decision space explicit.

**What happens:**
- Structured dialogue surfaces assumptions hiding in requirements
- Design decisions are presented as options with tradeoffs
- Scope boundaries are established
- Constraints are identified
- Research direction is focused

**Output:** A set of decisions made and constraints identified, plus a focused research brief.

**Key insight:** Without this step, research often explores the wrong things. Questioning ensures research time is well-spent.

### R — Research

**Purpose:** Understand how the codebase actually works. Document what exists — nothing more.

**What happens:**
- Parallel sub-agents explore different subsystems simultaneously
- Locator agents find WHERE files live
- Analyzer agents understand HOW code works
- Pattern finders identify existing conventions
- Sub-agents read 50+ files each, return compressed summaries
- Parent agent synthesizes findings into a research document

**Output:** A compressed research document (~200-500 tokens) capturing verified understanding of the relevant codebase.

**Key principle:** Research documents what IS, not what SHOULD BE. No suggestions, no critiques. Just a map.

**Context economics:**
- Input: 50+ files, thousands of lines (via sub-agents)
- Output: One markdown document
- Parent context cost: Near-zero (sub-agents did the heavy reading)

### S — Structure

**Purpose:** Create a phased breakdown distinguishing "where we're going" (design) from "how we get there" (plan details).

This phase was added because plans that jump straight from research to implementation details often fail. Structure separates design from execution.

**What happens:**
- Define current state and desired end state
- Break work into independently testable phases
- Establish what we're NOT doing (scope control)
- Identify risk areas and dependencies
- Resolve any open questions

**Output:** A structured outline with 3-5 phases, each with a clear goal, files touched, dependencies, and verification approach.

**Key principles:**
- Each phase is independently testable
- Each phase is independently revertable
- Earlier phases don't depend on later ones
- A phase should be completable in one focused session

### P — Plan

**Purpose:** Detail exact implementation steps with file references, code snippets, and success criteria.

A good plan is explicit enough for mechanical execution. Someone reading it should know exactly what will happen before any code is written.

**What happens:**
- Each phase gets detailed implementation steps
- Specific files and line numbers are referenced
- Code snippets show what will change
- Success criteria are split into automated (commands to run) and manual (behaviors to verify)
- No open questions allowed — everything resolved before finalizing

**Output:** A detailed plan document with file:line references, code snippets, and checkable success criteria.

**Key principles:**
- No open questions in the final plan
- Success criteria must be measurable
- "What we're NOT doing" section prevents scope creep
- Plans are reviewed by humans before implementation begins

### I — Implement

**Purpose:** Execute the plan mechanically, phase by phase, with verification.

**What happens:**
- Follow the plan's instructions phase by phase
- Run automated verification after each phase
- Pause for human review at phase boundaries
- Handle mismatches explicitly (stop, communicate, get direction)
- Keep context window small (research summary + plan + active code)

**Output:** Working code, verified against the plan's success criteria.

**Key principles:**
- Implementation should be almost mechanical — the thinking happened in prior phases
- Context stays small — the smart zone is preserved for actual coding
- Don't expand scope — flag things that should change but aren't in the plan
- Pause between phases — this is where you catch issues tests miss

## When to Use Which Phases

| Task Complexity | Phases Used |
|---|---|
| **Trivial** (typo, color change) | Direct chat — skip everything |
| **Simple** (single-file feature) | P → I |
| **Medium** (multi-file, single concern) | R → P → I |
| **Complex** (architectural, multi-service) | Q → R → S → P → I |
| **Extremely complex** (novel architecture) | Whiteboard → Q → R → S → P → I |

## Compaction Between Phases

The critical operational detail: **start a fresh context window between major phases.**

```
/question → [decisions documented] → FRESH WINDOW
/research → [research document] → FRESH WINDOW
/structure → [phase breakdown] → FRESH WINDOW
/plan → [detailed plan] → FRESH WINDOW
/implement → [working code]
```

Each phase produces a compressed artifact. The next phase starts with ONLY that artifact, not the entire history of exploration. This keeps every phase in the smart zone.

## Parallel Research Pattern

For larger features, parallelize the research phase:

```
┌─ Sub-agent 1: Auth subsystem research
├─ Sub-agent 2: API layer research
├─ Sub-agent 3: Database schema research
└─ Sub-agent 4: Test patterns research
        │
        ▼
   Parent synthesizes unified research document
        │
        ▼
   /structure with full picture
```

Sequential 10-minute exploration becomes 2 minutes.

## The Human Role at Each Phase

| Phase | Human Role | Time |
|---|---|---|
| Q — Question | Make design decisions | ~5-15 min |
| R — Research | Verify findings are correct | ~5-10 min |
| S — Structure | Approve phase breakdown | ~5 min |
| P — Plan | Review for correctness and completeness | ~10-15 min |
| I — Implement | Review at phase boundaries, manual verification | ~5 min/phase |

Total human time: 30-60 minutes for a complex feature. But those minutes are high-leverage — catching errors early where they're cheap to fix.

## After-Action Reports

Between phases, agents document:
- What succeeded and why
- What failed and why
- Lessons for subsequent phases

This creates iterative improvement across the session and prevents repeating mistakes.

## Ralph Loops

For autonomous workflows, HumanLayer uses "Ralph Loops" (named after Geoff Huntley's "Ralph Wiggum as a Software Engineer" pattern) — autonomous agent loops with **ruthless context resets**:

1. Agent picks up highest-priority ticket from the backlog
2. Runs a single QRSPI phase (research OR plan OR implement)
3. Produces a compressed artifact
4. Context resets completely
5. Next loop picks up where the last left off

This keeps each loop in the smart zone by design. The agent never accumulates stale context across phases.

## The Full 7-Step Origin

The internal pipeline before simplification was:

1. **Questioning** — Surface design decisions as explicit options
2. **Research** — Targeted, factual codebase investigation
3. **Design** — "Where we're going" — desired end state
4. **Structure** — "How we get there" — phased breakdown (~2 pages)
5. **Plan** — Precise implementation steps with verification
6. **Worktree** — Context isolation for implementation
7. **Implement** — Phase-by-phase execution

Two key human-review artifacts:
- **Design Discussion** (~200 lines): current state, desired end state, codebase patterns, resolved decisions, open questions
- **Structure Outline** (~2 pages): phase ordering and validation approaches

For the generic template, Design is folded into Structure (since most teams don't need a separate design document), and Worktree is an implementation detail rather than a workflow phase — yielding the 5-step QRSPI.
