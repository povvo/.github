# Governance

## Maintainer

This project is maintained by [@povvo](https://github.com/povvo). Final decisions are made by the maintainer.

## Branch Strategy

- `main`: production-ready code only; direct pushes are restricted
- `feat/<description>`: new features
- `fix/<description>`: bug fixes
- `chore/<description>`: maintenance, dependencies, and tooling
- `docs/<description>`: documentation only
- `refactor/<description>`: code restructuring without behaviour change

## Merge Strategy

- Feature and fix branches are squash-merged into `main`
- Commit messages follow Conventional Commits: `type(scope): description`
- Merge commits are not used on `main`
- Branches are deleted after merge

## Versioning

Projects that publish versions use [Semantic Versioning](https://semver.org/):

| Change | Bump |
|---|---|
| Breaking change | MAJOR |
| New feature | MINOR |
| Bug fix | PATCH |

## Releases

- Releases are tagged from `main` only
- Breaking changes are stated explicitly in release notes
- Project-specific release records remain in the project repository

## Two-Stage Review

Every task branch goes through two review passes before merge. They check different dimensions.

| Reviewer | Question | Focus |
|---|---|---|
| Spec reviewer | Did it do what was asked? | Requirement compliance, acceptance criteria, scope creep |
| Code reviewer | Is it well written? | Correctness, security, maintainability, edge cases |

Neither review substitutes for the other. Both verdicts are required before a merge is offered.

### 400 LOC Threshold

Review efficacy degrades as a change grows. Before review, estimate the changed lines excluding deleted lines and lock files.

| LOC | Action |
|---|---|
| 0–200 | Proceed |
| 200–400 | Proceed, with increased review attention |
| 400–600 | Flag for a human split decision |
| 600+ | Split before review |

## Conflict Resolution

When reviewers disagree, classify the disagreement before trying to settle it.

### Type I: Stylistic

Examples include naming, indentation, and import ordering. Defer to the project style guide. If it is silent, the author's preference prevails. Mark these as non-blocking.

### Type II: Empirical

Examples include performance claims, vulnerabilities, and correctness defects. Evidence decides the issue. The reviewer asserting a problem must provide a benchmark, complexity analysis, reproduction, or concrete exploit path.

### Type III: Architectural

Examples include design patterns, library selection, and module boundaries. Discuss the trade-off, record the decision in the pull request, and escalate unresolved cases to the maintainer.

## Verdict Matrix

| Spec | Code | Action |
|---|---|---|
| PASS | PASS | Accept |
| PASS | CONCERNS, minor | Accept with notes |
| PASS | CONCERNS, important | Human decision: accept caveats, fix, or escalate |
| PASS | CONCERNS, critical | Must fix |
| FAIL | PASS | Revise the implementation or update the specification |
| FAIL | CONCERNS | Revise the implementation |
| FAIL | FAIL | Replan or retry the task |

## Confidence Scoring

Code review should report only findings it can support.

| Confidence | Treatment |
|---|---|
| 0–25 | Do not report |
| 26–50 | Note internally |
| 51–79 | Report as minor |
| 80–89 | Report as important |
| 90–100 | Report as critical |

## Escalation

Escalate to the maintainer when:

- Reviewers directly contradict each other
- A failure is unclear or not actionable
- Two fix attempts have failed review
- The specification may itself be wrong

Accept with caveats when:

- The specification did not require the missing behaviour and the limitation is documented
- Minor style issues do not affect correctness
- A performance trade-off is measured and recorded
- Technical debt has a specific follow-up task

## Anti-Patterns

- Treating a passing specification review as permission to ignore code concerns
- Treating one reviewer as a substitute for the other
- Resolving conflicts automatically when evidence or judgement is still required
- Sending very large diffs through review and calling the resulting uncertainty thoroughness
