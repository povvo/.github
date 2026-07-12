# Contributing

Contributions are welcome. This document describes the process expected of every contributor, whether human or agent.

## Code Quality Standard

Submit code you have reviewed, understand fully, and can defend line by line.

A contribution is acceptable when you understand what it does, it is not a naive or default implementation, and you can explain the decisions behind it.

## Branch Naming

Task branches:

```
task-<N>-<slug>
```

Human branches:

```
feat/<description>
fix/<description>
chore/<description>
docs/<description>
refactor/<description>
perf/<description>
test/<description>
style/<description>
```

Never push directly to `main`, `master`, or `release`. Force pushes to protected branches are forbidden.

## Commit Messages

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Use for |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Code change, no new feature or fix |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `chore` | Maintenance, dependencies, tooling |

### Subject line rules

- Imperative mood: `add feature`, not `added feature`
- Lowercase after the type
- No trailing full stop
- Maximum 50 characters
- State what changed, not how

### Body

Explain **why**, not what. Include a body when the reason is non-obvious, there are trade-offs, or the change fixes a specific non-obvious issue.

### Breaking changes

Append `!` to the type and include a `BREAKING CHANGE:` block in the footer:

```
feat(api)!: require authentication on all endpoints

BREAKING CHANGE: All endpoints now require authentication.
Previously /health was public.

Migration: add Authorization header to all requests.
See MIGRATION.md for full steps.
```

## Verification Evidence

Passing tests is not sufficient evidence that an implementation works.

Before opening a pull request:

- Observe actual output: CLI output, API responses, rendered UI, or screenshots
- Put the commands run and outputs observed in the pull request
- Confirm the verified commit SHA and file manifest match the submitted branch

## Review Gates

Every pull request requires two review passes before a human merge decision:

1. **Spec compliance** verifies the implementation matches what was agreed. It must return PASS.
2. **Code quality** checks correctness, security surface, quality, and maintainability. It must return PASS or CONCERNS explicitly acknowledged by a human.

No pull request merges without human approval. Auto-merge is disabled.

## What Gets Merged

- Code that solves a real problem clearly
- Implementations that are correct, not merely functional
- Pull requests with observable evidence that the implementation works
- Changes that respect existing architecture and conventions
- Full compliance with this contributing document

## What Does Not Get Merged

Half-finished work, implementations the submitter cannot explain, and pull requests without verification evidence.

Maintainer: [@povvo](https://github.com/povvo)
