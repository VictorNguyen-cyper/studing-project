---
name: mentor-code-review
description: Teaching-oriented code review methodology — turn standard review findings into mentorship by explaining why a finding matters in this specific repo, citing the onboarding guide/convention it comes from, and tracking recurring lessons across a new contributor's early PRs. Use when reviewing a newcomer's PR, pairing review feedback with onboarding material, or deciding when a contributor no longer needs teaching-mode review.
metadata:
  origin: ECC
---

# Mentor Code Review

A methodology for reviewing a new contributor's early PRs so that review feedback also completes their onboarding, instead of being a second, unrelated gate they have to pass. Pairs with `codebase-onboarding` and `code-tour` on the ramp-up side, and with `code-reviewer`/`/code-review` on the standard-review side.

## When to Use

- Reviewing one of a new contributor's first few PRs in a repo
- The user asks for a review "in teaching mode," "explain like I'm new here," or "review this for someone ramping up"
- Deciding whether a contributor has graduated out of needing teaching-mode review

## When NOT to Use

| Instead of mentor-code-review | Use |
| --- | --- |
| Reviewing a contributor who is already ramped up | `code-reviewer` / `/code-review` |
| The newcomer hasn't been onboarded yet | `onboarding-guide` / `codebase-onboarding` first |
| Multi-agent adversarial review for a risky change | `/review-pr` or `/orch-review` |
| The ask is architecture/design feedback, not a diff review | `architect` or `code-explorer` |

## How It Works

### 1. Anchor to onboarding material

Before reviewing, check for:
- An Onboarding Guide or `CLAUDE.md` produced during this contributor's onboarding
- A `.tour` file covering the area being changed
- A learning trail at `.claude/onboarding/lessons-<contributor>.md`, if one exists

This is what turns "why it matters" into something concrete instead of generic advice.

### 2. Review at full strength, explain at teaching depth

Run the same checklist and severity model as a standard review (Security, Quality, Best Practices, style — see `code-reviewer`). Do not relax CRITICAL/HIGH bars. For every finding, attach:

- **What** — the issue itself
- **Why it matters here** — the project-specific reason (a documented convention, a past incident, a framework-specific gotcha). If no project-specific reason exists, say so rather than inventing one.
- **Fix** — the concrete change

### 3. Distill, don't just list

Close with a short "Patterns to Remember" section — the compressed, generalizable lesson(s) from this review, not a recap of every finding. Two or three bullets, maximum.

### 4. Maintain the learning trail

Append new, genuinely new patterns to `.claude/onboarding/lessons-<contributor>.md`. Before adding an entry, check it isn't already there — a lesson repeated across five reviews means the teaching isn't landing, which is itself a signal (see Graduation below), not a reason to keep restating it louder.

### 5. Decide on graduation

A contributor has likely outgrown mentor mode when:
- Several consecutive reviews surface no new "Patterns to Remember" entries
- The same categories of mistake stop recurring in their diffs
- They've shipped a handful of PRs without a CRITICAL/HIGH finding

When any of these hold, say so explicitly and suggest moving to `/code-review` for future PRs. Teaching mode that never ends stops being useful feedback and starts being noise.

## Best Practices

1. **Never lower the bar to be kind** — teaching framing changes tone, not standards.
2. **Specificity over platitudes** — "we use Result types here because a 2024 incident traced to a swallowed exception" beats "error handling is important."
3. **One pass, one lesson set** — resist the urge to teach everything at once; a reviewee learns 2-3 things per PR, not fifteen.
4. **Make the trail visible** — the contributor should be able to read their own lessons file and see progress, not just scattered PR comments.

## Anti-Patterns to Avoid

- Softening or omitting CRITICAL/HIGH findings for a newcomer's comfort.
- Fabricating a "why it matters here" when the real answer is "no documented reason — ask the team."
- Restating an already-learned lesson instead of recognizing it's time to graduate the contributor.
- Teaching-mode reviews that never check whether the contributor still needs them.

## Examples

### Example 1: First PR from a new contributor
**Input**: Diff from a contributor onboarded earlier the same day via `onboarding-guide`.
**Action**: Load their Onboarding Guide + `.tour`, review at full strength, explain findings against detected conventions, write 2-3 "Patterns to Remember," start their lessons file.

### Example 2: Fifth PR, no new lessons surfacing
**Input**: Contributor's lessons file shows no additions in the last 3 reviews.
**Action**: Review as usual, but explicitly flag in the summary that they appear ramped up and suggest switching to `/code-review` going forward.
