---
description: Surface design decisions and constraints before research begins
---

# Question — Surface Design Decisions

You are tasked with helping the user think through a task BEFORE any code research begins. Your job is to surface implicit assumptions, identify design decisions that need to be made, and clarify constraints.

## Why This Step Exists

Most failed implementations trace back to misunderstood requirements, not bad code. This step makes design decisions explicit before investing time in research and planning.

## When This Command Is Invoked

Respond with:
```
I'll help surface the key decisions for this task before we dive into research.

Please describe:
1. What you're trying to accomplish (the goal, not the solution)
2. Any constraints you already know about
3. Who/what is affected by this change

I'll identify the design decisions we need to make upfront.
```

Then wait for the user's input.

## Process

### Step 1: Understand the Goal

- Read any referenced files, tickets, or documents FULLY
- Identify the underlying need (the "why") behind the request
- Distinguish between the stated solution and the actual problem

### Step 2: Surface Implicit Decisions

For every non-trivial task, there are decisions hiding in the requirements. Find them:

- **Scope boundaries** — What's in and what's explicitly out?
- **Approach options** — Are there fundamentally different ways to solve this?
- **Compatibility constraints** — What existing behavior must be preserved?
- **Performance implications** — Does this need to handle scale?
- **Data considerations** — Migration needed? Backwards compatibility?
- **Dependency decisions** — Build vs. buy? New library vs. existing?
- **Testing strategy** — What level of confidence is needed?

### Step 3: Present as Options, Not Opinions

Structure your output as decisions to be made, each with clear options:

```
## Task: [Restated goal in your own words]

### Decision 1: [Decision Name]
**Context:** [Why this decision matters]
**Options:**
  A. [Option] — [tradeoff]
  B. [Option] — [tradeoff]
  C. [Option] — [tradeoff]
**My lean:** [If you have enough context to suggest one, briefly say why]

### Decision 2: [Decision Name]
...

### Constraints Identified
- [Hard constraint from requirements]
- [Soft constraint from context]

### Assumptions to Validate During Research
- [Assumption that needs code verification]
- [Assumption about existing behavior]

### Suggested Research Focus
Based on these decisions, research should focus on:
1. [Specific area to investigate]
2. [Specific area to investigate]
```

### Step 4: Get Alignment

Wait for the user to make decisions on each point before proceeding. Document their choices.

## Important Guidelines

- **Don't research code yet.** This step is about thinking, not exploring.
- **Don't propose solutions.** Surface the decision space.
- **Don't skip this for "obvious" tasks.** The obvious ones are where implicit assumptions bite hardest.
- **Keep it concise.** 3-7 decisions max. If there are more, the task should be split.
- **Make the "do nothing" option explicit** when relevant — sometimes the best decision is to not do the thing.

## Output

The output of this phase feeds directly into `/research`. Decisions made here constrain what research needs to explore and what it can skip.
