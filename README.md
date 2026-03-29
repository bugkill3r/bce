# BCE - Better Context Engineering

A generic, team-ready template for applying context engineering principles to any codebase. Drop it into your repo and start shipping better work with AI coding agents.

Based on the QRSPI workflow (Question, Research, Structure, Plan, Implement) — the evolution of Research-Plan-Implement that emerged from HumanLayer's production practices, Dex Horthy's talks, and real-world experience on mature codebases.

## Why This Exists

AI coding agents produce great work on greenfield projects and terrible work on mature codebases. The difference isn't the model — it's the context. Context engineering is the systematic management of information you give AI systems. Better tokens in, better tokens out.

Key insight: LLMs degrade past ~40% context window utilization (the "dumb zone"). Everything in this template is designed to keep agents in the "smart zone" through deliberate context management.

## What's Inside

```
bce/
├── template/                    # Drop into your repo
│   ├── CLAUDE.md                # Customize for your project
│   ├── .claude/
│   │   ├── commands/            # QRSPI workflow phases
│   │   │   ├── question.md      # Q — Surface design decisions
│   │   │   ├── research.md      # R — Map the codebase
│   │   │   ├── structure.md     # S — Phase the work
│   │   │   ├── plan.md          # P — Detail exact steps
│   │   │   ├── implement.md     # I — Execute the plan
│   │   │   └── iterate-plan.md  # Revise existing plans
│   │   └── agents/              # Sub-agent definitions
│   │       ├── codebase-locator.md
│   │       ├── codebase-analyzer.md
│   │       └── pattern-finder.md
│   ├── .cursorrules             # For Cursor users (mirrors CLAUDE.md)
│   └── .context.md.example      # Template for directory-level context
├── docs/
│   └── references/
│       ├── sources.md           # All source material & links
│       ├── principles.md        # Core context engineering principles
│       ├── qrspi-framework.md   # The full QRSPI framework explained
│       └── leverage-hierarchy.md # Where to focus human effort
├── examples/
│   ├── hooks.json               # Claude Code hook examples
│   ├── context-hierarchy.md     # Progressive disclosure example
│   └── complexity-heuristic.md  # When to use which workflow level
└── README.md                    # This file
```

## Quick Start

### 1. Copy the template into your repo

```bash
cp -r template/.claude your-repo/.claude
cp template/CLAUDE.md your-repo/CLAUDE.md
```

### 2. Customize CLAUDE.md

Edit `CLAUDE.md` with your project's:
- Tech stack and build commands
- Conventions and non-negotiables
- Testing requirements
- Tribal knowledge that lives nowhere in the code

### 3. Add directory-level context (optional but recommended)

```
your-repo/
├── CLAUDE.md                    # Root conventions
├── src/
│   ├── .context.md              # Package-level architecture
│   ├── auth/
│   │   └── .context.md          # Auth domain specifics
│   └── api/
│       └── .context.md          # API patterns
```

### 4. Use the QRSPI workflow

Match effort to complexity:

| Task Complexity | Approach |
|---|---|
| Trivial (typo, button color) | Direct chat |
| Simple (single-file feature) | `/plan` → implement |
| Medium (multi-file, single concern) | `/research` → `/plan` → `/implement` |
| Complex (architectural, multi-service) | `/question` → `/research` → `/structure` → `/plan` → `/implement` |
| Extremely complex | Whiteboard first, then full QRSPI |

## The Leverage Hierarchy

Focus human effort at the top — errors compound downstream.

```
CLAUDE.md                    ← set once, affects every task
  └── Persistent memory      ← refines behavior over weeks
      └── Hooks              ← automate context decisions
          └── Prompts        ← shape each session
              └── Research   ← compress truth from code
                  └── Plans  ← compress intent into steps
                      └── Implementation  ← mechanical execution
```

A bad line of code is one bad line. A bad line in a plan becomes 100 bad lines. A misunderstanding in research sends the entire task wrong.

## Core Principles

1. **Context is the only lever.** Models are smart enough. They just don't know what you know.
2. **Less context = better decisions.** Stay under 40% window utilization.
3. **Compact between phases.** Finish research → summarize → fresh window for planning.
4. **Sub-agents are for context control, not org-chart cosplay.** Fork clean windows for exploration, return compressed summaries.
5. **Don't outsource the thinking.** AI amplifies whatever thinking you've done — or the lack of it.
6. **Review research and plans, not just code.** That's where leverage lives.
7. **Documentation lies. Code is truth.** Research from live code, not stale markdown.

## Sources & Further Reading

See [docs/references/sources.md](docs/references/sources.md) for the complete list.

Key sources:
- [Context is all you need](https://mishras.xyz/posts/context-is-all-you-need) (Parts [1](https://mishras.xyz/posts/context-is-all-you-need) & [2](https://mishras.xyz/posts/context-is-all-you-need-part-2))
- [No Vibes Allowed](https://www.youtube.com/watch?v=rmvDxxNubIg) — Dex Horthy, AI Engineer (the original RPI talk)
- [Everything We Got Wrong About RPI](https://www.youtube.com/watch?v=YwZR6tc7qYg) — Dex Horthy, Coding Agents 2026 (the QRSPI evolution)
- [HumanLayer .claude/commands](https://github.com/humanlayer/humanlayer/tree/main/.claude/commands) — production prompts
- [ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md) — companion guide
- [A Survey of Context Engineering for LLMs](https://github.com/Meirtz/Awesome-Context-Engineering) (Mei et al., 2025)

Design note: each command has <40 instructions. LLMs reliably follow ~150-200 instructions; exceeding this causes silent drops. HumanLayer's original 85-instruction planning prompt was a key failure point.
