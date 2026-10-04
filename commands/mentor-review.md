---
description: Review a PR or local diff in teaching mode — same severity bar as /code-review, but every finding explains why it matters here and links back to onboarding conventions. Use for a new contributor's first few PRs; once ramped up, switch to /code-review.
argument-hint: [pr-number | pr-url | blank for local review]
---

# Mentor Review

**Input**: $ARGUMENTS

Delegate to the `mentor-reviewer` agent, applying the `mentor-code-review` skill's methodology. This command reuses `/code-review`'s mode selection, validation, and publish mechanics — the only difference is the review and report phases below.

## Mode Selection

If `$ARGUMENTS` contains a PR number, PR URL, or `--pr` → **PR mode** (fetch via `gh pr view`/`gh pr diff`, same as `/code-review` Phase 1).
Otherwise → **Local mode** (`git diff --staged` / `git diff`, same as `/code-review` Phase 1).

## Phase 1 — Load the contributor's context

Look for, in order:
1. An Onboarding Guide or `CLAUDE.md` produced by `/onboard` / `onboarding-guide`
2. A `.tour` file covering the changed area
3. `.claude/onboarding/lessons-<contributor>.md` — a prior learning trail for this contributor

If none exist, proceed without them and say so in the report — don't block the review on missing onboarding artifacts.

## Phase 2 — Review

Apply the exact same checklist and severity model as `/code-review` (Correctness, Security, Pattern Compliance, Performance, Completeness, Maintainability — CRITICAL/HIGH/MEDIUM/LOW). Do not relax CRITICAL/HIGH for teaching's sake.

## Phase 3 — Report

For each finding:

```markdown
### [SEVERITY] <title> — <file>:<line>

**What**: <the issue>
**Why it matters here**: <project-specific reason from the loaded context,
  or "no documented convention — worth raising with the team" if none exists>
**Fix**: <concrete change>
```

Close with:

```markdown
## Patterns to Remember
- <1-3 compressed, generalizable takeaways — not a recap of every finding>
```

## Phase 4 — Update the learning trail

Append genuinely new entries from "Patterns to Remember" to `.claude/onboarding/lessons-<contributor>.md` (create it if absent). Skip entries already present there.

## Phase 5 — Graduation check

If the lessons file shows no new entries across several consecutive reviews, or the same mistake categories have stopped recurring, say so explicitly in the summary and suggest switching to `/code-review` for this contributor going forward.

## Phase 6 — Validate and publish

Reuse `/code-review`'s Phase 4 (validation commands) and Phase 7 (GitHub publish via `gh pr review`) unchanged — teaching mode only changes what the review body says, not how it's delivered or gated.

## Edge Cases

- **No `gh` CLI**: local-only review, same fallback as `/code-review`.
- **No onboarding artifacts found at all**: still run the review; note in the summary that running `/onboard` first would make future reviews more specific.
