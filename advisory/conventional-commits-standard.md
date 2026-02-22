# Conventional Commits Standard

## Purpose

This document establishes the standard for commit messages across all repositories in this organization. Conventional Commits provide a structured, machine-readable format for commit messages that enables automated changelog generation, semantic versioning, and clearer project history.

## Standard Reference

This organization follows the Conventional Commits specification as documented at https://sawin.com.np/blog/conventional-commits and the broader Conventional Commits standard (https://www.conventionalcommits.org/).

## Required Format

All commits must follow this structure:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

The type field must be one of:

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation changes
- **style**: Changes that do not affect code meaning (formatting, whitespace, etc.)
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: Code change that improves performance
- **test**: Changes to tests
- **chore**: Changes to build system, dependencies, or other non-code items

### Scope

The scope is optional and provides context for what part of the codebase is affected. Use kebab-case for multi-word scopes.

Examples: `auth`, `user-model`, `api-routes`

### Subject

- Use imperative mood ("add feature" not "adds feature" or "added feature")
- Do not capitalize the first letter
- No period at the end
- Maximum 50 characters
- Be concise and descriptive

### Body

The body is optional. When present:

- Explain *what* and *why*, not how
- Separate from subject with a blank line
- Wrap at 72 characters
- Use bullet points for multiple changes

### Footer

The footer is optional. Common footers:

- `Closes #123` - Link to issue
- `Refs #456` - Reference related issue
- `BREAKING CHANGE: description` - Indicate breaking changes
- `Co-authored-by: Name <email>` - Credit multiple authors (do not use automated generated co-author footers)

## Examples

### Simple fix without scope

```
fix: prevent race condition in auth flow
```

### Feature with scope and body

```
feat(api): add rate limiting to endpoints

Implement token-bucket algorithm to prevent API abuse. Adds new
RateLimiter middleware and configuration options.

Closes #234
```

### Breaking change

```
feat(api)!: restructure authentication response

BREAKING CHANGE: auth endpoint now returns JWT in response body
instead of Authorization header for improved security.
```

## Enforcement

- All commits must follow this standard
- Pull request reviews will check commit message format
- Commit messages are permanent records; clarity and consistency matter
- Tools may be used to validate commits automatically (pre-commit hooks)

## Benefits

- Automated changelog generation from commit history
- Clear project history for future maintainers
- Machine-readable structure enables tooling
- Consistency across projects and team members
- Easier debugging through precise commit descriptions
