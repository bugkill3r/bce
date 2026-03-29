---
description: Create a pull request with context from the QRSPI process
---

# Pull Request — Ship with Context

Create a pull request that includes context from the process — not just a wall of green diffs, but the decisions, design, and verification that produced them.

## When This Command Is Invoked

If on a feature branch with committed changes, proceed directly.

Otherwise respond with:
```
I'll help create a PR. Please confirm:
1. All changes are committed
2. You're on the feature branch (not main)
3. The plan's success criteria have passed

I'll draft the PR with process context attached.
```

## Process

### Step 1: Gather Context

- Read the plan document (if available)
- Read the design document (if available)
- Run `git log main..HEAD --oneline` to see all commits
- Run `git diff main...HEAD --stat` to see files changed

### Step 2: Verify Before Opening

Run the plan's automated success criteria one final time:
```
[test command]
[lint command]
[build command]
```

If anything fails, stop and fix before creating the PR.

### Step 3: Push and Create PR

```bash
git push -u origin [branch-name]
```

Create the PR with this structure:

```markdown
## Summary
[1-3 sentences: what this PR does and why]

## Design Decisions
[Key decisions from /question and /design — the WHY behind the changes]
- [Decision 1]: [choice and rationale]
- [Decision 2]: [choice and rationale]

## Changes by Phase
[From the plan — what changed in each phase]

### Phase 1: [Name]
- [Change summary]
- [Files touched]

### Phase 2: [Name]
- [Change summary]
- [Files touched]

## What's NOT Included
[From the design's out-of-scope section — set expectations]

## Testing
[What was verified — both automated and manual]

### Automated
- [x] [Test command] — passing
- [x] [Lint command] — passing
- [x] [Build command] — passing

### Manual
- [x] [Manual check performed]
- [x] [Manual check performed]

## Review Guide
[Where to focus review attention — the highest-risk changes]
```

### Step 4: Present to User

```
PR created: [URL]

The PR includes:
- Design decisions and rationale
- Phase-by-phase change summary
- Verification results
- Review focus areas

Ready for team review.
```

## Important Guidelines

- **Attach process context.** The reviewer should understand WHY, not just WHAT.
- **Include out-of-scope.** Prevents "why didn't you also do X?" comments.
- **Verify before opening.** Never open a PR with failing checks.
- **Guide the reviewer.** Point them to the highest-risk changes first.
- **Never force push or push to main.** Always confirm with the user.
