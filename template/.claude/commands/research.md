---
description: Research the codebase to understand what exists — document, don't evaluate
---

# Research — Map the Codebase

You are tasked with conducting comprehensive research across the codebase. Your ONLY job is to document and explain the codebase AS IT EXISTS TODAY.

## Critical Constraints

- DO NOT suggest improvements or changes
- DO NOT critique the implementation
- DO NOT propose future enhancements
- DO NOT perform root cause analysis unless explicitly asked
- ONLY describe what exists, where it exists, how it works, and how components interact
- You are creating a technical map, not a review

## When This Command Is Invoked

If parameters are provided (file path, ticket, or topic), read them fully and begin research immediately.

Otherwise respond with:
```
I'm ready to research the codebase. Please provide:
1. Your research question or area of interest
2. Any specific files or areas you already know about
3. Decisions from a prior /question phase (if applicable)

I'll explore relevant components and return a compressed research document.
```

Then wait for the user's input.

## Process

### Step 1: Read Referenced Files First

- If the user mentions specific files, read them FULLY before spawning sub-agents
- This ensures you have complete context before decomposing the research

### Step 2: Decompose the Research

Break down the query into parallel research areas. Think about:
- Which subsystems are involved?
- What files likely contain relevant logic?
- Are there patterns or conventions to discover?
- What connections exist between components?

### Step 3: Spawn Parallel Sub-Agents

Use specialized agents to research different aspects concurrently:

**codebase-locator** — Find WHERE files and components live
- Input: Feature name, keyword, or concept
- Output: File paths grouped by purpose

**codebase-analyzer** — Understand HOW specific code works
- Input: Specific files or components to analyze
- Output: Flow analysis with file:line references

**pattern-finder** — Find existing patterns to follow
- Input: Pattern type or similar feature name
- Output: Code examples showing "how we do this here"

**Key principle:** Start with locator agents, then use analyzers on the most promising findings. Each sub-agent gets a fresh context window — they can read 50+ files without polluting your context.

### Step 4: Wait and Synthesize

IMPORTANT: Wait for ALL sub-agents to complete before proceeding.

- Compile results from all sub-agents
- Connect findings across different components
- Prioritize live code findings as primary source of truth
- Include specific file paths and line numbers

### Step 5: Generate Research Document

Write a research document (to a file or as output) with this structure:

```markdown
# Research: [Topic]

**Date**: [Current date]
**Scope**: [What was investigated]

## Summary
[2-3 sentences answering the research question by describing what exists]

## Detailed Findings

### [Component/Area 1]
- What it does: [description] (`file.ext:line`)
- How it connects: [connections to other components]
- Key details: [implementation specifics]

### [Component/Area 2]
...

## Code References
- `path/to/file.ext:123` — [What's there]
- `path/to/other.ext:45-67` — [What this block does]

## Patterns & Conventions Found
- [Pattern name]: [How it's used, with example file references]

## Assumptions to Validate
- [Anything uncertain that needs human confirmation]

## Context for Next Phase
[Compressed summary suitable for feeding into /structure or /plan]
```

### Step 6: Present and Iterate

- Present a concise summary to the user
- Include key file references for navigation
- Ask if they have follow-up questions
- For follow-ups, spawn additional sub-agents as needed

## Context Economics

The entire point of this phase is compression:
- **Input:** 50+ files, thousands of lines of code (via sub-agents)
- **Output:** One research document, ~200-500 tokens of compressed understanding
- **Parent context cost:** Near-zero (sub-agents did the heavy reading)

This research document becomes the seed for the next phase — plan or structure — in a FRESH context window.

## Important Notes

- Always use parallel sub-agents to maximize efficiency and minimize context usage
- Sub-agents are for context control, not role-playing
- Document cross-component connections
- Research documents should be self-contained
- The main agent synthesizes — sub-agents do deep file reading
- ALL sub-agents are documentarians, not critics
- Document what IS, not what SHOULD BE
