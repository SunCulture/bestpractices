# Compound Engineering Workflow

Every task follows: **Plan → Design Review → Execute → Test (98%+) → Multi-agent Review → Document → PR (develop/dev)**

## Standards (loaded every session)

All standards live at `docs/standards/` — read them before starting any task:

| Standard | File | When |
|----------|------|------|
| Architecture review | `docs/standards/architecture-review.md` | Before writing code — write a plan first |
| Code review | `docs/standards/code-review.md` | Self-review during execution; review agents use this |
| Test coverage | `docs/standards/test-coverage.md` | Every change must meet 98% line coverage |
| Security review | `docs/standards/security-review.md` | Any auth/crypto/PII/payment change |
| CI/CD pipeline | `docs/standards/ci-cd-pipeline.md` | PR lifecycle and merge gates |
| Documentation | `docs/standards/documentation.md` | After every task — compound step |

## Before any work

1. Write a plan at `docs/plans/<slug>.md` per the architecture-review standard
2. Run past cartwright (design review) before touching code

## During execution

- Use ponytail mode (minimal, lazy-correct code)
- Follow fullstack-eng conventions (SOLID, UUIDv7, structured logging, etc.)
- Every non-trivial change gets a test. Target 98%+ line coverage on changed files

## After any work

1. **Compound**: write `docs/solutions/<slug>.md` per the documentation standard — problem, solution, prevention
2. **Review**: run whetstone (code review), locksmith (security), touchstone (coverage check)
3. **Document**: loresmith links the solution to README.md
4. **PR**: always against develop/dev branch

## Before any PR

1. Ensure the PR description captures all changes made from the commits in the branch
2. Ensure the PR description does not have any signatures
3. Ensure all commits do not have any signatures
4. Request **multiple** reviewers when humans should review — never stop at one by default.
   Resolve GitHub login casing first (`gh api users/<handle> -q .login`) or use
   `Devops/scripts/request-pr-reviewers/request-pr-reviewers.sh`. Wrong-case handles
   fail silently. Team `@SunCulture/the-unit` review requests do not work on the
   current Free org plan — use individual logins. Verify with
   `gh pr view <n> --json reviewRequests`.

## Error recurrence

When the same mistake happens twice, add a rule in this file or a `docs/standards/` doc to prevent it. Third occurrence is a system failure, not a human one.

## Project-specific conventions

(Add stack details, naming conventions, preferences below)
