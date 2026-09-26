# CI/CD Pipeline Standard

## Branch flow

`feature/<slug>` → PR into `develop` → (periodic, reviewed PR) `develop` → `main`.
No direct commits to `develop` or `main` — every change lands through a PR, even small
ones. A hosting provider's auto-deploy-on-push (e.g. Amplify, Vercel) is a deploy
mechanism, not a substitute for the PR/CI gate; it must trigger from `main` after a
reviewed merge, not from arbitrary pushes.

## Required checks before merge

A PR is mergeable only when CI is green on:

1. **Lint** — the repo's configured linter (eslint/ruff/golangci-lint/etc.), not a task
   that exists in the build graph but has no leaf implementation.
2. **Type check** — `tsc`/`mypy`/equivalent, strict mode where the language supports it.
3. **Test + coverage** — per `test-coverage.md`, gating on changed-file coverage.
4. **Build** — the actual production build/bundle step, not just `dev` working locally.

If any of these don't exist yet in a repo, standing them up is itself the first PR —
don't let "there's no CI gate yet" become the reason nothing gets gated.

## What CI is not

CI is not a substitute for the code review in `code-review.md` or the security review in
`security-review.md`. Green CI means the change doesn't break anything mechanically
checkable; it says nothing about whether the change is the right one.

## Ownership

Whoever adds a new app/package to a monorepo is responsible for wiring its
lint/test/build tasks into the shared pipeline (e.g. `turbo.json`) in the same PR — a
new package with no CI coverage is a gap from day one, not something to backfill later.

## Pull request reviewers (multi)

SunCulture PRs may — and usually should — request **more than one** reviewer.

- Prefer several individual logins (e.g. `dentonya`, `KelvinNJoseph`, `Baraka-Mulumia`).
- Resolve casing before requesting: `gh api users/<handle> -q .login`, or use
  `Devops/scripts/request-pr-reviewers/request-pr-reviewers.sh`.
- Never rely on wrong-case handles (`baraka-mulumia`, `kelvinnjoseph`) with
  `gh pr edit --add-reviewer` — GitHub accepts the call but may skip the user.
- Org plan is Free: `@SunCulture/the-unit` **team** review requests are ignored.
  Expand CODEOWNERS with multiple `@user` entries (or upgrade the org plan for
  team CODEOWNERS / required team reviews).
- After requesting, verify: `gh pr view <n> --json reviewRequests`.
