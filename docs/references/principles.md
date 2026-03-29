# Core Context Engineering Principles

The fundamental principles that underpin everything in this template.

## 1. Context Is the Only Lever

LLMs are stateless. They have no memory between sessions. The only thing that affects output quality (without model training) is input quality.

> "The model is plenty smart; it just doesn't know what you know. The only way to get better output is to put better tokens in."

This means the difference between an agent that produces great code and one that produces "slop" isn't the model — it's what you put in the context window.

## 2. The Dumb Zone

LLM performance degrades significantly as context utilization increases. Empirically, this begins around **~40% of the context window** (roughly 67k tokens of a 168k window).

Causes:
- Large tool outputs (JSON, UUIDs, logs)
- Unfiltered file dumps
- Repeated correction loops
- Too many MCP tools loaded (20+ tools = 10-15% context just for tool definitions)
- Long chat histories full of noise

**More tokens ≠ better decisions.** Past the dumb zone threshold, every additional token makes the agent worse at everything.

Related research: "Lost in the Middle" paper found LLMs perform 70%+ worse on information buried in long contexts.

## 3. Trajectory Matters

LLMs learn patterns within a conversation. If the trajectory looks like:
1. Model makes mistake
2. Human corrects harshly
3. Model makes another mistake
4. Human corrects again

The most likely continuation is... another mistake. Bad trajectories reinforce failure modes.

This is why **restarting sessions** with compressed context is often more effective than continued correction.

## 4. Intentional Compaction

Deliberately compress context between phases. Don't drag an ever-growing conversation forward.

The process:
1. Finish a phase of work (research, planning, etc.)
2. Summarize the accumulated understanding into a markdown artifact
3. Review and validate the summary as a human
4. Start a fresh context window seeded with just that artifact

**What to compact:** Verified findings, decisions made, key file references, constraints
**What to discard:** Raw logs, tool traces, full file contents, verbose error explanations

This is what operating systems do with memory — page out inactive context to make room for the working set.

## 5. Sub-Agents Are Architecture, Not Roles

Sub-agents exist for **context control**, not org-chart cosplay.

**Wrong:** "Frontend agent", "Backend agent", "QA agent"
**Right:** "Locator agent (find files)", "Analyzer agent (trace code flow)", "Pattern finder (find examples)"

The mechanics:
1. Parent agent starts with clean context
2. Spawns sub-agent with narrow task (e.g., "find all files related to auth")
3. Sub-agent reads 50+ files, fills its context to 80%
4. Returns compressed summary to parent
5. Parent context remains at ~6%, now has the knowledge it needs

## 6. Documentation Lies, Code Is Truth

> "Between actual code, function names, comments, and documentation, documentation has the highest density of stale information."

Documentation becomes outdated the moment new functionality ships. On-demand compressed context — derived from current code through a research phase — beats static docs every time.

The research document says the same things a doc would say, but it's derived from current code, not aspirational markdown from six months ago.

Exception: CLAUDE.md files earn their value with **tribal knowledge** that lives nowhere in the repo — "we avoid ORMs because of X", "the auth service uses a specific adapter pattern because of Y", "don't touch billing without running integration suite against staging."

## 7. The Leverage Hierarchy

```
CLAUDE.md                    ← Set once, affects every task
  └── Persistent memory      ← Refines behavior over weeks
      └── Hooks              ← Automate context decisions
          └── Prompts        ← Shape each session
              └── Research   ← Compress truth from code
                  └── Plans  ← Compress intent into steps
                      └── Implementation  ← Mechanical execution
```

**Error amplification:**
- A bad line of code → 1 bad line
- A bad line in a plan → 100 bad lines of code
- A misunderstanding in research → entire task goes wrong

Focus human effort at the top. Spending hours reviewing implementation diffs doesn't help if you skipped the 10 minutes reviewing the plan.

## 8. Don't Outsource the Thinking

AI amplifies whatever thinking you've done — or the lack of it.

Every workflow requires:
- Reading research and verifying correctness
- Reviewing plans and catching mistakes before they become code
- Validating implementation matches the plan

> "There is no perfect prompt. There is no silver bullet. Thinking cannot be outsourced."

Watch out for tools that generate markdown files to make you feel productive. If you're not reading them, you're not getting value.

## 9. Match Effort to Complexity

Not every task needs the full QRSPI workflow:

| Task | Approach |
|---|---|
| Trivial (typo, button color) | Direct chat |
| Simple (single-file feature) | Light plan → implement |
| Medium (multi-file, single concern) | Research → Plan → Implement |
| Complex (architectural, multi-service) | Full QRSPI |
| Extremely complex | Whiteboard first, then QRSPI |

The ceiling of solvable problems goes up with more context engineering. But simple tasks don't need it.

## 10. Plans Are Compressed Intent

Plans aren't bureaucracy — they're the mechanism for:
- **Mental alignment** with your team (you can read plans, you can't read thousands of lines of AI-generated code)
- **Scope control** (the "what we're NOT doing" section prevents scope creep)
- **Reviewability** (plans with code snippets and test details offer a better review experience than raw PRs)
- **Context compression** (research might be 500 tokens, plan maybe 1000 — leaves most of the window in the smart zone for implementation)

## Context Priority Hierarchy

When optimizing context, address problems in this order (worst to least bad):

1. **Incorrect information** — actively harmful, produces wrong code
2. **Missing information** — agent has to guess, may hallucinate
3. **Excessive noise** — pushes into dumb zone, degrades all decisions

Optimization targets:
- **Correctness** — is the information accurate?
- **Completeness** — does the agent have what it needs?
- **Size** — is the context as small as possible?
- **Trajectory** — is the conversation guiding toward success?
