# Progressive Context Disclosure — Example

Instead of dumping everything into one massive system prompt (which pushes you into the dumb zone immediately), use hierarchical context files that are pulled based on where the agent is working.

## Directory Structure

```
your-repo/
├── CLAUDE.md                         # L1: Project-wide conventions
│                                     #     Tech stack, non-negotiables, build commands
│                                     #     Loaded automatically every session
│
├── src/
│   ├── .context.md                   # L2: Package-level architecture
│   │                                 #     How src/ is organized, key patterns
│   │
│   ├── auth/
│   │   ├── .context.md               # L3: Domain-specific context
│   │   │                             #     Auth-specific conventions, security notes
│   │   │                             #     "All auth endpoints must validate JWT"
│   │   │                             #     "Rate limiting is handled by middleware, not handlers"
│   │   ├── handler.go
│   │   └── middleware.go
│   │
│   ├── billing/
│   │   ├── .context.md               # L3: "Don't touch without running integration suite"
│   │   │                             #     "Uses legacy Stripe API v2, migration planned Q3"
│   │   ├── processor.go
│   │   └── webhook.go
│   │
│   └── api/
│       ├── .context.md               # L3: API patterns, middleware chain, error format
│       └── routes.go
│
└── docs/
    └── architecture.md               # Reference only — linked from .context.md, not inlined
```

## How It Works

When the agent is working on `src/auth/handler.go`, it pulls:
1. **CLAUDE.md** — project conventions (always loaded)
2. **src/.context.md** — package architecture (if agent navigates there)
3. **src/auth/.context.md** — auth domain specifics (most relevant)

It does NOT pull `src/billing/.context.md` or `src/api/.context.md` — those would be noise.

## What Goes in Each Level

### CLAUDE.md (L1) — ~300-500 tokens
- Tech stack
- Build/test/lint commands
- Non-negotiable conventions
- Things you'd tell a new hire on day one

### Package .context.md (L2) — ~100-200 tokens
- How the package is organized
- Key architectural patterns used here
- Cross-cutting concerns

### Domain .context.md (L3) — ~100-200 tokens
- Domain-specific rules and conventions
- Integration notes (external services, APIs)
- Danger zones ("don't modify X without Y")
- Non-obvious constraints

## What NOT to Put in Context Files

- Anything derivable from reading the code
- Git history or changelog information
- Full API documentation (link to it instead)
- Things that change frequently (they'll go stale)

## Automating with Hooks

You can wire hooks to auto-inject context when the agent reads files in specific directories:

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Read",
      "hooks": [{
        "type": "command",
        "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -q 'src/auth'; then cat src/auth/.context.md 2>/dev/null; fi"
      }]
    }]
  }
}
```

This converts the passive context hierarchy from Part 1 into active, automatic context injection. But start without hooks — add them only when you see a repeated pattern of the agent missing domain context.
