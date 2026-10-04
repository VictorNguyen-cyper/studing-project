---
description: Onboard a new contributor to this repo — architecture overview, a guided CodeTour, and a first-PR review-readiness checklist. Pass a focus area to scope the tour; for the follow-up review use /mentor-review.
argument-hint: [focus-area | blank for full repo]
---

# Onboard

**Input**: $ARGUMENTS

Delegate to the `onboarding-guide` agent. If no agent/Task invocation is available in the current context, run the equivalent steps directly:

## Phase 1 — Scope

If `$ARGUMENTS` names a folder, package, or subsystem, scope everything below to it. Otherwise scope to the whole repo but keep the tour to the 5-8 most load-bearing files (entry points, core domain logic, most-frequently-changed files per `git log --format= --name-only | sort | uniq -c | sort -rn | head -20`).

## Phase 2 — Architecture reconnaissance

Run the `codebase-onboarding` skill's reconnaissance and architecture-mapping phases, scoped per Phase 1. Produce its Onboarding Guide output, and its Starter CLAUDE.md output if `CLAUDE.md` is missing or stale (enhance, don't replace, an existing one).

## Phase 3 — Guided tour

Run the `code-tour` skill to produce one `.tour` file in `.tours/`, persona = "new contributor," depth = ramp-up. Anchor it on the files identified in Phase 1.

## Phase 4 — First PR Readiness Checklist

Inspect what actually governs review in this repo (`CLAUDE.md`, `.claude/rules/*`, `rules/**`, the categories in `code-reviewer`/`code-review`, detected lint/test/build commands, CONTRIBUTING.md). Produce:

```markdown
# First PR Readiness Checklist — [Project Name]

- [ ] Ran: `[detected lint command]`
- [ ] Ran: `[detected test command]`
- [ ] Ran: `[detected typecheck/build command]`
- [ ] Commit message follows: `[detected convention]`
- [ ] No secrets/credentials in the diff
- [ ] New code matches conventions in the Onboarding Guide above
- [ ] [any repo-specific blocking condition detected]
```

## Phase 5 — Handoff

Report to the user:

```
Onboarding complete for: <scope>

Artifacts:
  Onboarding Guide: printed above
  CLAUDE.md: <written | updated | already current | not applicable>
  Tour: .tours/<file>.tour
  Checklist: printed above

Next step: when your first change is ready, run /mentor-review instead of
/code-review — it explains findings against the conventions above rather
than just listing them.
```

## Edge Cases

- **Empty repo / no commits**: skip git-convention detection, note it, proceed with the rest.
- **Monorepo**: if `$ARGUMENTS` doesn't disambiguate a package, ask which package/app before running Phase 2-4 rather than guessing.
- **Re-running `/onboard`**: treat as a refresh — diff against the existing `CLAUDE.md`/tour and call out what changed since last time, rather than regenerating from scratch silently.
