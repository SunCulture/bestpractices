# Multi-reviewer tagging on SunCulture PRs

## Problem

Requesting several reviewers often left only one (or none) tagged. Wrong-case
handles (`baraka-mulumia`, `kelvinnjoseph`) make `gh pr edit --add-reviewer`
succeed while GitHub skips the user. Team requests (`@SunCulture/the-unit`) are
ignored on the org’s Free plan. Batch `requested_reviewers` POSTs return 422 when
any named user is not a collaborator on that repo and can drop the whole set.
Most product repos also lacked multi-owner CODEOWNERS.

## Solution

- Standard: `docs/standards/ci-cd-pipeline.md` (Pull request reviewers) +
  `AGENTS.md` + Cursor rule `.cursor/rules/multi-pr-reviewers.mdc`.
- Helper: `scripts/request-pr-reviewers/request-pr-reviewers.sh` resolves logins
  via the Users API, requests **one login at a time**, skips the PR author, and
  verifies the PR’s `requested_reviewers` list.
- CODEOWNERS catch-all on `sundeskv2`, `status`, `amt-core`, and `Devops` lists
  `@dentonya @KelvinNJoseph @Baraka-Mulumia @Eleazar-Harold` (GitHub only
  auto-requests users who already have collaborator access on that repo).

## Prevention

- Agents must verify `gh pr view <n> --json reviewRequests` after tagging.
- Prefer the helper script over raw `gh pr edit` with unverified casing.
- Do not request `@SunCulture/the-unit` until the org plan supports team reviews.
- If a login fails with “not a collaborator”, add them to the repo (or pick
  another collaborator) — the helper will still attach everyone who is valid.
