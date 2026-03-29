# The Full Process: From RPI to 8 Steps

## Evolution: From RPI to the Full Process

The original Research-Plan-Implement (RPI) workflow — introduced by Dex Horthy at HumanLayer — proved effective but had failure modes in practice:

**RPI's failures** (from Dex's 6-month post-mortem, "Everything We Got Wrong About Research-Plan-Implement", Coding Agents 2026):
- The research step used a single sub-agent with a broad, open-ended prompt, consuming ~40% of context just for orientation
- The planning prompt contained **85+ instructions** — well over the reliable budget
- Critical interactive steps (e.g., "work back and forth with me") were buried and frequently skipped
- About half of users received finished plans with predetermined decisions instead of collaborative ones
- Teams pasted entire tickets into research rather than asking targeted questions, producing opinions instead of facts

**The instruction budget:** Frontier LLMs can reliably follow only ~150-200 instructions. When you combine system prompts, planning prompts, tool definitions, and MCP servers, models become "over budget" and silently drop requirements. This is why the process splits one 85-instruction prompt into eight prompts of <40 instructions each.

**Code review reversal:** Dex initially advocated reviewing plans instead of code. After 6 months, he revised: reading a 1,000-line plan still produces ~1,000 lines of code — review both. "Please read the code. It's the same amount of work."

The team's initial expansion was **QRDSPWIP** (Question, Research, Design, Structure, Plan, Worktree, Implement, Publish) — the acronym didn't work, so they called it **QRSPI** for short. But the actual process has all 8 steps.

## The Eight Steps

### 1. Questions

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

### 2. Research

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

### 3. Design

**Purpose:** Define WHERE we're going — the desired end state, separate from how we get there.

**What happens:**
- Summarize current state from research (with file:line references)
- Define the desired end state concretely
- Document resolved decisions from the Questioning phase
- Resolve any remaining open questions
- Establish what we're NOT doing (scope control)

**Output:** A Design Discussion document (~200 lines) containing: current state, desired end state, codebase patterns to follow, resolved decisions with rationale, and out-of-scope items.

**Key principle:** This is WHAT, not HOW. No implementation steps, no phase ordering, no file-change lists.

### 4. Structure Outline

**Purpose:** Define HOW we get to the design's end state — the phased breakdown.

**What happens:**
- Take the Design document as input
- Break the delta (current → desired) into independently testable phases
- Define validation approach for each phase
- Identify ordering constraints and dependencies

**Output:** A ~2-page structure outline with 3-5 phases, each with a goal, files touched, dependencies, and verification approach.

**Key principles:**
- Each phase is independently testable and revertable
- Earlier phases don't depend on later ones
- A phase should be completable in one focused session
- This is HOW, not WHAT — the Design already defined the end state

### 5. Plan

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

### 6. Worktree

**Purpose:** Create an isolated environment for implementation — the context isolation boundary.

**What happens:**
- Create a git worktree on a feature branch
- Install dependencies, verify build and tests pass clean
- Load the plan into the fresh context

**Output:** A clean worktree with passing build, ready for implementation.

**Key principle:** This is a context reset point. The implementation phase starts with a fresh context window — just the plan and the clean worktree. No accumulated exploration noise.

### 7. Implement

**Purpose:** Execute the plan mechanically, phase by phase, with verification.

**What happens:**
- Follow the plan's instructions phase by phase
- Run automated verification after each phase
- Pause for human review at phase boundaries
- Handle mismatches explicitly (stop, communicate, get direction)
- Keep context window small (plan + active code)

**Output:** Working code, verified against the plan's success criteria.

**Key principles:**
- Implementation should be almost mechanical — the thinking happened in prior steps
- Context stays small — the smart zone is preserved for actual coding
- Don't expand scope — flag things that should change but aren't in the plan
- Pause between phases — this is where you catch issues tests miss

### 8. Pull Request

**Purpose:** Ship with process context attached — not just a wall of green diffs.

**What happens:**
- Run final verification (automated success criteria)
- Push the branch
- Create PR with design decisions, phase-by-phase changes, verification results, and review focus areas
- Include out-of-scope items to set reviewer expectations

**Output:** A PR that takes the reviewer on a journey — WHY, not just WHAT.

**Key principle:** As AI output scales, reviewing thousands of lines is unsustainable. PRs that include the decisions, design, and verification from prior steps make review tractable.

## When to Use Which Steps

| Task Complexity | Steps Used |
|---|---|
| **Trivial** (typo, color change) | Direct chat — skip everything |
| **Simple** (single-file feature) | Plan → Implement |
| **Medium** (multi-file, single concern) | Research → Plan → Implement → PR |
| **Complex** (architectural, multi-service) | All 8 steps |
| **Extremely complex** (novel architecture) | Whiteboard → All 8 steps |

## Compaction Between Steps

The critical operational detail: **start a fresh context window between major steps.**

```
/question  → [decisions doc]       → FRESH WINDOW
/research  → [research doc]        → FRESH WINDOW
/design    → [design doc]          → FRESH WINDOW
/structure → [structure outline]   → FRESH WINDOW
/plan      → [detailed plan]       → FRESH WINDOW
/worktree  → [clean environment]   → FRESH WINDOW
/implement → [working code]        →
/pr        → [PR with context]
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

## The Human Role at Each Step

| Step | Human Role | Time |
|---|---|---|
| 1. Questions | Make design decisions (back-and-forth) | ~5-15 min |
| 2. Research | Verify findings are correct | ~5-10 min |
| 3. Design | Review end state and decisions doc | ~5-10 min |
| 4. Structure | Approve phase breakdown | ~5 min |
| 5. Plan | Review for correctness and completeness | ~10-15 min |
| 6. Worktree | Confirm branch/setup | ~1 min |
| 7. Implement | Review at phase boundaries, manual verification | ~5 min/phase |
| 8. PR | Review PR description, submit for team review | ~5 min |

Total human time: 40-75 minutes for a complex feature. But those minutes are high-leverage — catching errors early where they're cheap to fix.

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

## The Naming

The internal pipeline was originally called **QRDSPWIP** — too long. They shortened the acronym to **QRSPI** but kept all the steps. The actual process from the slide is 8 steps:

1. Questions
2. Research
3. Design
4. Structure Outline
5. Plan
6. Worktree
7. Implement
8. Pull Request

Two key human-review artifacts produced along the way:
- **Design Discussion** (~200 lines): current state, desired end state, codebase patterns, resolved decisions, open questions
- **Structure Outline** (~2 pages): phase ordering and validation approaches
