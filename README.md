# Studing Project

Standalone export of the **AI Onboarding & Code-Review Assistant** feature built for the ECC (Claude Code plugin) repository.

## Contents

- `agents/onboarding-guide.md` — orchestrator agent for onboarding new contributors (delegates to `codebase-onboarding` + `code-tour`, generates a "First PR Readiness Checklist")
- `agents/mentor-reviewer.md` — mentor-style PR reviewer for newcomers (same rigor as `code-reviewer`, but explains *why it matters here*)
- `skills/mentor-code-review/SKILL.md` — methodology bridging onboarding and review
- `commands/onboard.md` — `/onboard` slash command
- `commands/mentor-review.md` — `/mentor-review` slash command
- `.tours/new-joiner-ecc-architecture.tour` — CodeTour walkthrough of the ECC architecture used to demo the onboarding flow

These files were extracted from a working copy of the ECC repo where they were built and tested (`/onboard` was run against the real repo, and the relevant validators — `validate-agents.js`, `validate-commands.js`, `validate-rules.js`, `check-unicode-safety.js`, `validate-no-personal-paths.js` — passed). They are provided here standalone, without the catalog/registry sync changes made in the original repo (README/AGENTS.md/plugin.json counts, etc.), since those are specific to that repo's generated files.
