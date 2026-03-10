---
name: go-style
description: Prescriptive standards for Go code style, project structure, and development practices emphasizing simplicity and idiomatic Go
license: CC-BY-4.0
compatibility: claude-dev,opencode
metadata:
  version: "1.0"
  category: "engineering"
  type: "advisory"
---

# Go Style and Conventions

**Status**: Advisory  
**Target Audience**: Duncan de Boer organization projects  
**Go Version**: 1.20 or later  

## Overview

This advisory establishes prescriptive standards for Go code style, project structure, and development practices. The goal is consistency, maintainability, and alignment with the Go community's best practices.

**Philosophy**: Favor simplicity, clarity, and idiomatic Go. Tooling (gofmt, golangci-lint) enforces style; this document covers higher-level conventions.

---

## Code Style

### Formatting

- **Mandatory**: Use `gofmt` for all code. Non-negotiable.
- **Linting**: Enable `golangci-lint` with strict rules; configuration should include `gofmt`, `vet`, `errcheck`, `staticcheck`, and `misspell`.
- **Line Length**: Soft limit 100 characters. Break long lines for readability.

### Naming Conventions

- **Packages**: Lowercase, single word preferred (e.g., `config`, `storage`). Avoid plural or underscores.
- **Exported Functions/Types**: `PascalCase` (e.g., `UserService`, `FetchUser`).
- **Unexported Functions/Variables**: `camelCase` (e.g., `userService`, `fetchUser`).
- **Error Variables**: Use `err`, `w`, `r` for file operations; contextual names for domain-specific errors.
- **Constants**: `SCREAMING_SNAKE_CASE` for package-level constants; `CamelCase` within functions.

### Code Organization

- Group related declarations together.
- Place `const` and `var` declarations before function definitions.

---

## Project Structure

```text
projectname/
├── cmd/
│   ├── app1/
│   │   └── main.go
│   └── app2/
│       └── main.go
├── internal/
│   ├── config/
│   ├── storage/
│   └── service/
├── pkg/
│   ├── models/
│   └── utils/
├── test/
├── go.mod
├── go.sum
└── README.md
```

- **cmd/**: Executables only. One package per executable.
- **internal/**: Private packages not intended for external use.
- **pkg/**: Public, reusable libraries.
- **Avoid**: Deep nesting (max 3 levels). Keep packages focused and cohesive.

---

## Comments and Documentation

- **Exported Symbols**: Document all public functions, types, and methods with comments starting with the symbol name.

  ```go
  // UserService handles user operations.
  type UserService struct { ... }
  ```

- **Unexported Code**: Comment non-obvious algorithms or complex logic.
- **Package Documentation**: Include a package-level comment in `doc.go` or the primary package file.
- **Example Code**: Include usage examples in package documentation for public APIs.

---

## Testing

- **Pattern**: Use table-driven tests for comprehensive coverage.

  ```go
  tests := []struct {
    name  string
    input string
    want  int
  }{
    {"case1", "input1", 1},
  }
  for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) { ... })
  }
  ```

- **Coverage**: Minimum 80% for libraries; 70% for applications.
- **Naming**: Test files: `*_test.go`. Tests: `Test<FunctionName>` or `Test<Type>_<Method>`.
- **Benchmarks**: Include benchmarks for performance-critical functions.

---

## Error Handling

- **Wrap Errors**: Use `fmt.Errorf("%w", err)` to add context while preserving the error chain.

  ```go
  if err != nil {
    return fmt.Errorf("failed to fetch user: %w", err)
  }
  ```

- **Avoid Generics**: Never return a bare `error`; specify the error type or wrap with context.
- **Sentinel Errors**: Use sparingly. Prefer error wrapping and type assertions for custom errors.
- **No Silent Failures**: Always log or return errors; never ignore them.

---

## Dependencies

- **Minimize**: Keep external dependencies to a minimum. Evaluate each dependency carefully.
- **Vendor**: Use `go mod vendor` for reproducible builds, or rely on `go.mod` lock files in CI/CD.
- **Security**: Regularly run `go mod tidy` and check for CVE advisories. Review dependency updates monthly.
- **Import Organization**: Group imports (stdlib, then third-party, then internal), separated by blank lines.

---

## Commit Conventions

- **Message Format**: Imperative mood, clear subject line. Include issue/design references when applicable.

  ```text
  Implement user authentication service

  - Add JWT token validation
  - Implement session management
  - Fixes #123
  ```

- **Co-authored-by**: When working with Copilot or in pair programming:

  ```text
  Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
  ```

- **Atomic Commits**: One logical change per commit. Easy to review and revert if needed.

---

## Performance and Optimization

- **Measure Before Optimizing**: Use `pprof` and benchmarks to identify bottlenecks.
- **Memory Allocation**: Minimize allocations in hot paths. Pre-allocate slices when size is known.
- **Concurrency**: Use goroutines and channels idiomatically. Avoid data races (test with `-race`).
- **Documentation**: Comment non-obvious performance decisions.

---

## Trigger Language

Use these phrases when requesting code review or standards compliance:

- "Review this for Go style compliance"
- "Check Go project structure"
- "Validate Go testing standards"
- "Ensure error handling best practices"
