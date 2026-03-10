---
name: conventional-commits
description: Establishes structured, machine-readable commit message format for semantic versioning and automated changelog generation
license: CC-BY-4.0
compatibility: claude-dev,opencode
metadata:
  version: "1.0"
  category: "advisory"
  type: "standard"
---

# Conventional Commits Standard

## Purpose

This document establishes the standard for commit messages across all repositories in this organization. Conventional Commits provide a structured, machine-readable format for commit messages that enables automated changelog generation, semantic versioning, and clearer project history.

## Standard Reference

This organization follows the Conventional Commits specification as documented at
[sawin.com.np/blog/conventional-commits](https://sawin.com.np/blog/conventional-commits)
and the broader [Conventional Commits standard](https://www.conventionalcommits.org/).

## Required Format

All commits must follow this structure:

```text
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

The type field must be one of:

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semicolons, etc)
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing or correcting existing tests
- **chore**: Changes to the build process, dependency management, or other auxiliary tools

### Scope

The scope is optional and specifies what area of the codebase is affected. Wrap it in parentheses and use lowercase with hyphens:
Examples: `(auth)`, `(database)`, `(api)`, `(ci-cd)`

### Subject

The subject line must:

- Be in imperative mood ("add" not "added" or "adds")
- Not capitalize the first letter (unless it's a proper noun)
- Not end with a period (.)
- Be no more than 50 characters
- Clearly describe what the change does

### Body

The body is optional but recommended for non-trivial changes. It should:

- Explain what the change does and why
- Be wrapped at 72 characters
- Be separated from the subject by a blank line
- Be written in imperative mood

### Footer

The footer is optional and can include:

- **References to issue trackers**: `Closes #123` or `Refs #456`
- **Breaking changes**: `BREAKING CHANGE: description`
- **Co-authored-by**: `Co-authored-by: Name <email@example.com>`

## Examples

### Simple feature commit

```text
feat(auth): add password reset functionality
```

### Bug fix with body

```text
fix(api): handle null response in user endpoint

The endpoint was not properly handling cases where the backend
returned null. Added defensive null checks and return a 204
No Content response instead.

Closes #789
```

### Breaking change

```text
refactor(database)!: remove legacy user migration support

BREAKING CHANGE: The deprecated migrate-legacy-users command is no
longer available. Users must have migrated by version 2.0.

Closes #456
```

### Collaborative commit

```text
feat(payments): integrate stripe webhook handler

Implements asynchronous payment status updates via Stripe webhooks.
Includes idempotency handling and retry logic.

Co-authored-by: Jane Doe <jane@example.com>
Closes #234
```

## Tools and Automation

Conventional Commits enable several powerful automations:

- **Semantic Versioning**: Automatically determine version bumps (major.minor.patch) based on commit types
- **Changelog Generation**: Automatically generate changelogs from commit messages
- **CI/CD Integration**: Trigger different pipelines or deployments based on commit type
- **Filtering and Searching**: Easily find specific types of changes in history

## Common Mistakes

- **Mixed tenses**: Do not mix "add" and "added"; always use imperative mood
- **Capitalization**: Subject should not be capitalized (unless proper noun)
- **Periods**: Do not end the subject with a period
- **Too much detail in subject**: Move details to the body
- **Vague subjects**: "fix stuff" or "update code" are not descriptive
- **Incorrect scope format**: Use lowercase with hyphens: `(my-scope)` not `(MyScope)`

## Enforcement

This standard is enforced via:

- **commitlint**: Automated validation of commit message format
- **Pre-commit hooks**: Local validation before pushing
- **CI/CD checks**: Repository-level validation on pull requests

See your repository's commitlint configuration for the specific rules applied.
