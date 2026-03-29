# Sources & References

All source material that informed the ACE template.

## Primary Sources

### Articles

**Context is all you need** (Part 1) — Saurabh Mishra, December 2025
https://mishras.xyz/posts/context-is-all-you-need

Introduces context engineering fundamentals: the dumb zone (~40% context utilization), progressive context disclosure via hierarchical `.context.md` files, on-demand compressed context vs stale documentation, the Research-Plan-Implement (RPI) workflow, sub-agents for context isolation (Locator, Analyzer, Pattern Finder), manual compaction between phases, and the leverage hierarchy (CLAUDE.md > prompts > research > plans > implementation). Includes complexity heuristic table for matching effort to task size.

**Context is all you need** (Part 2) — Saurabh Mishra, March 2026
https://mishras.xyz/posts/context-is-all-you-need-part-2

Extends Part 1 with tooling evolution: CLAUDE.md + auto memory for persistent context, the AGENTS.md movement (ETH Zurich study showing LLM-generated files reduce success by 3%), sub-agents with git worktrees for parallel isolation, hooks for automated context injection, manual compaction over auto-compaction, MCP as context source (keep tool sets small — 20+ tools eats 10-15% of context), multi-agent orchestration, and the revised leverage hierarchy adding persistent memory and hooks.

### Video Talks

**No Vibes Allowed: Solving Hard Problems in Complex Codebases** — Dex Horthy (HumanLayer), AI Engineer Conference, December 2025
https://www.youtube.com/watch?v=rmvDxxNubIg

The foundational talk on context engineering for coding agents. Covers: why AI produces "slop" in brownfield codebases (survey of 100k developers), context as the only control surface, the dumb zone phenomenon, trajectory effects (bad patterns reinforce failure), intentional compaction strategy, sub-agents for context control not role-playing, the full RPI workflow with case studies (300k LOC Rust fix, 35k LOC BAML feature), mental alignment through plans, the Parquet Java failure case (7 hours, domain expertise needed), and the seniority split.

Key frameworks introduced:
- Performance = (Correctness² × Completeness) / Size
- Context priority: Incorrect info > Missing info > Excessive noise
- "Do not outsource the thinking"

**Everything We Got Wrong About Research-Plan-Implement** — Dex Horthy, Coding Agents 2026 (Mountain View, CA)
https://www.youtube.com/watch?v=YwZR6tc7qYg
Channel: MLOps.community

A 6-month post-mortem of the "No Vibes Allowed" talk. Candidly shares what failed when RPI was scaled to real teams:
- Research prompt had 85+ instructions — over the model's reliable ~150-200 instruction budget
- Half of users got finished plans with predetermined decisions instead of collaborative ones
- Reversed position on "review plans not code" — "Please read the code. It's the same amount of work."
- Introduced QRSPI (from QRDSPWIP): Question, Research, Structure, Plan, Implement — each step <40 instructions
- Introduced Ralph Loops: autonomous loops with ruthless context resets
- After-Action Reports: agents document what succeeded/failed between phases
- The Autonomy Slider: teams need to decide where they sit on the human control ↔ agent autonomy spectrum

**Advanced Context Engineering for Coding Agents** — Dex Horthy & Vaibhav, ~1h27m
https://www.youtube.com/watch?v=rmvDxxNubIg (same as Video 1 — the longer deep-dive podcast version covers the same ground plus case studies)

Also covers: frequent intentional compaction framework, case study details (BAML Rust 300k LOC, 35k LOC feature), specs as source code (Sean Grove's framework), and workflow optimization with worktrees.

### GitHub Repositories

**HumanLayer .claude/commands** — Production prompt templates
https://github.com/humanlayer/humanlayer/tree/main/.claude/commands

Files:
- `research_codebase.md` — Comprehensive codebase research with parallel sub-agents, structured output format with YAML frontmatter
- `create_plan.md` — 5-step interactive plan creation (Context Gathering → Research → Structure → Detail → Review)
- `create_plan_nt.md` — Same as create_plan without the "thoughts" directory dependency
- `implement_plan.md` — Phase-by-phase implementation with automated + manual verification
- `iterate_plan.md` — Surgical updates to existing plans

**HumanLayer .claude/agents** — Sub-agent definitions
https://github.com/humanlayer/humanlayer/tree/main/.claude/agents

Files:
- `codebase-locator.md` — Find WHERE code lives (Grep, Glob, LS only; Sonnet model)
- `codebase-analyzer.md` — Understand HOW code works (Read, Grep, Glob, LS; Sonnet model)
- `codebase-pattern-finder.md` — Find existing patterns (Grep, Glob, Read, LS; Sonnet model)
- `thoughts-locator.md` — Find documents in thoughts directory
- `thoughts-analyzer.md` — Extract insights from documents
- `web-search-researcher.md` — External documentation research

All agents follow the "documentarian, not critic" principle — they describe what exists without suggesting changes.

**Advanced Context Engineering companion doc**
https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md

Comprehensive guide covering: the Stanford study findings, context priority hierarchy, naive vs intentional approaches, sub-agents as architecture for context windows, frequent intentional compaction framework, case studies with timelines, human leverage architecture, and the "specs as source" conceptualization.

### Academic & Research

**A Survey of Context Engineering for Large Language Models** — Mei et al., 2025
Academic survey covering 1,400+ papers on context engineering.
GitHub companion: https://github.com/Meirtz/Awesome-Context-Engineering

**Evaluating AGENTS.md** — Gloaguen et al., 2026 (ETH Zurich)
https://arxiv.org/abs/2602.11988
LLM-generated context files reduced task success by 3% while increasing inference costs by 20%. Human-written files showed 4% improvement only when containing information agents couldn't find elsewhere.

**METR: Measuring AI Impact on Developer Productivity** — 2025
https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
Experienced developers were 19% slower with AI tools on mature open-source codebases while believing they'd accelerated by 20%. 2026 follow-up revised to roughly breakeven.

## Secondary Sources

### Blog Posts & Articles

**The Necessary Evolution of RPI** — BetterQuestions.ai, 2026
https://betterquestions.ai/the-necessary-evolution-of-research-plan-implement-as-an-agentic-practice-in-2026/
Explains why RPI failed in practice and how it evolved to QRSPI. Key insight: the single mega-prompt research step consumed ~40% context for orientation alone.

**Context Engineering for Coding Agents** — Martin Fowler
https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html
Context as the primary bottleneck for agent effectiveness.

**Boris Cherny on Lenny's Podcast** — Claude Code lead
https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens
Ships 20-30 PRs daily running 5 parallel instances across worktrees. Review commands spawn sub-agents checking style, history, and bugs.

**Augmented Coding** — Kent Beck
https://tidyfirst.substack.com/p/augmented-coding-beyond-the-vibes
Distinction between "vibe coding" (caring only about behavior) and "augmented coding" (caring about code, complexity, tests, coverage).

**Agentic Engineering Patterns** — Simon Willison
https://simonwillison.net/guides/agentic-engineering-patterns/
Practical patterns including sandboxing, TDD with agents.

**Pragmatic Engineer AI Tooling Survey 2026**
https://newsletter.pragmaticengineer.com/p/ai-tooling-2026
95% weekly adoption, 75% use AI for half+ of work, 55% use agents regularly.

**Effective Context Engineering for AI Agents** — Anthropic
https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents

### Dev Community Summaries

**Advanced Context Engineering for Coding Agents** — Alex Metelli, DEV Community
https://dev.to/ametel01/advanced-context-engineering-for-coding-agents-11p7
Faithful technical distillation of the first Dex Horthy talk.

**No Vibes Allowed — Notes** — bagrounds.org
https://bagrounds.org/videos/no-vibes-allowed-solving-hard-problems-in-complex-codebases-dex-horthy-humanlayer

### Talks

**The New Code** — Sean Grove (OpenAI), AI Engineer Conference
https://www.youtube.com/watch?v=8rABwKRsec4
Treating code like assembly — prompts/specs become the source code, generated code is the compiled artifact. Key thesis: code is a "lossy projection" from specifications, just as decompiled binaries lack original variable names. Code represents only 10-20% of a programmer's professional value; the remaining 80-90% is requirements gathering, planning, collaboration, and verification. Uses OpenAI's Model Spec as case study — markdown files that are human-readable, version-controlled, and testable.

### Tools & Frameworks

**Claude Code documentation** — Anthropic
https://docs.anthropic.com/en/docs/claude-code
Official docs for hooks, memory, worktrees, sub-agents.

**HumanLayer** — https://www.humanlayer.dev/
Human-in-the-loop platform for AI agents. Building "Aentic IDE."

### Additional Sources (from agents' research)

**Dex Horthy on Ralph, RPI, and escaping the "Dumb Zone"** — Dev Interrupted Podcast
https://devinterrupted.substack.com/p/dex-horthy-on-ralph-rpi-and-escaping

**Ralph loops make agentic coding reliable with ruthless context resets** — LinearB
https://linearb.io/blog/dex-horthy-humanlayer-rpi-methodology-ralph-loop

**The Humans in the Loop Deep Dive: Making AI Agents Mainstream** — Dexter Horthy interview
https://thehumansintheloop.substack.com/p/making-agents-mainstream-for-dev-with-dexter-horthy

**WISC Framework** (Write, Isolate, Select, Compress) — LangChain
https://blog.langchain.com/context-engineering-for-agents/
Complementary framework addressing mechanical context management: Write (save outside context window), Isolate (sub-agents with separate windows), Select (pull only relevant context via search), Compress (summarize when too large).

**Context Engineering: A Framework for Enterprise AI Operations** — Shelly Palmer
https://shellypalmer.com/2025/06/context-engineering-a-framework-for-enterprise-ai-operations/

**Mei et al. Survey formal definition of context:**
C = A(c_instr, c_know, c_tools, c_mem, c_state, c_query) — context as structured assembly of system instructions, external knowledge, tool definitions, persistent memory, dynamic state, and user queries.
