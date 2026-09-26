# Multi-reviewer tagging on SunCulture PRs

## Problem

Requesting several reviewers often left only one (or none) tagged. Wrong-case
handles (`baraka-mulumia`, `kelvinnjoseph`) make `gh pr edit --add-reviewer`
succeed while GitHub skips the user. Team requests (`@SunCulture/the-unit`) are
ignored on the org’s Free plan. Most product repos also lacked multi-owner
CODEOWNERS, so nothing nudged agents toward multiple people.

## Solution

- Standard: `docs/standards/ci-cd-pipeline.md` (Pull request reviewers) + root
  `AGENTS.md` + Cursor rule `.cursor/rules/multi-pr-reviewers.mdc`.
- Helper: `Devops/scripts/request-pr-reviewers/request-pr-reviewers.sh` resolves
  logins via the Users API, requests all of them, and verifies the PR’s
  `requested_reviewers` list.
- CODEOWNERS catch-all expanded on `sundeskv2`, `status`, and `amt-core` to
  `@dentonya @KelvinNJoseph @Baraka-Mulumia @Eleazar-Harold`.

## Prevention

- Agents must verify `gh pr view <n> --json reviewRequests` after tagging.
- Prefer the helper script over raw `gh pr edit` with unverified casing.
- Do not request `@SunCulture/the-unit` until the org plan supports team reviews.
