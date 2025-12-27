
# GitHub Copilot Instructions – `immich-go` (Go)

## 1. Project Context

You are contributing to **`immich-go`**, an open-source **Go CLI tool** used to bulk upload, migrate, and archive personal media into an Immich server.

This project:

* Handles large volumes of personal data
* Is often used in automated or headless environments
* Must remain predictable, safe, and backward compatible

**Never assume user intent.**
If a request is ambiguous, **ask for clarification before writing code**.

---

## 2. Go Programming Style (Mandatory)

Follow idiomatic Go strictly:

* Compatible with `gofmt`, `go vet`, and `golangci-lint`
* Prefer simplicity and explicitness
* Avoid unnecessary abstractions
* One function, one responsibility
* No dead or commented-out code

### Naming

* Clear, explicit English names
* No generic suffixes (`Util`, `Helper`, `Common`)
* Interfaces named after behavior (`Uploader`, `Source`, `Archiver`)
* Short variable names only in tight scopes

### Errors

* Always handle errors explicitly
* Use contextual wrapping:

  ```go
  return fmt.Errorf("failed to parse capture date for %s: %w", path, err)
  ```
* `panic` only for unrecoverable programmer errors

### Context & Concurrency

* Long-running or I/O-bound operations must accept `context.Context`
* Ensure cancellation, no goroutine leaks, deterministic shutdown

---

## 3. Testing (Non-Negotiable)

### Rules

* Every new feature must include tests
* Every change must preserve or improve test coverage
* If tests are missing or insufficient, write them

### Expectations

* Prefer unit tests using `testing`
* Deterministic, fast, isolated
* No real network or filesystem unless abstracted

### immich-go Focus Areas

* CLI flags and config parsing
* Folder → album mapping logic
* Timezone and EXIF date handling
* Dry-run behavior (must never mutate state)
* Partial failures and error propagation

Copilot must **actively identify missing test cases**.

---

## 4. Documentation (Required)

### New Code

All exported code must include GoDoc explaining:

* Purpose
* Behavior
* Non-obvious constraints or side effects

### Legacy Code

When touching existing code:

* Update or add documentation if missing or inaccurate
* Align comments with actual behavior

---

## 5. Dependencies: Standard Library First

* Always prefer Go standard library
* External dependencies require explicit justification
* No convenience or overlapping dependencies

---

## 6. Large Changes: Plan First

For any non-trivial change, Copilot **must propose a plan before coding**.

### Required Plan

1. Functional goal
2. Assumptions and open questions
3. Step-by-step implementation plan
4. Impact analysis:

   * CLI compatibility
   * workflows
   * tests
   * documentation

No code until the plan is acknowledged.

---

## 7. Scope Control, Clarification, and Decomposition

* Challenge unclear or risky requests
* Ask clarifying questions early
* Never silently assume requirements
* Break ambitious goals into:

  * small
  * reviewable
  * reversible steps

Each step must have:

* a clear objective
* validation criteria

---

## 8. Legacy Code Refactoring (Strictly Controlled)

Refactoring in `immich-go` is **intentional, incremental, and planned**.

### Refactoring Goals

* Reduce technical debt
* Improve readability and testability
* Align legacy code with current standards
* Eliminate hidden coupling and side effects

### Refactoring Rules

* No behavior change without explicit agreement
* No mixed feature + refactor work
* Backward compatibility must be preserved

### Mandatory Refactoring Plan

1. Current state analysis
2. Target state
3. Step-by-step refactor sequence
4. Safety measures (tests first)

### Execution

* Prefer multiple small PRs
* Each step must compile and pass tests
* Minimal diff noise

### Challenging Over-Ambitious Refactors

Copilot must push back and propose safer, incremental alternatives.

---

## 9. Mergeability and Branch Hygiene

Changes must be:

* Easy to review
* Easy to merge into `main` / `develop`
* Designed to minimize long-lived divergence

Rules:

* Prefer incremental PRs
* Avoid large rebases
* Favor additive over destructive changes

---

## 10. Conventional Commits & Change Communication (Mandatory)

### 10.1 Commit Messages

All commits **must follow Conventional Commits**:

```
<type>(<scope>): <short, imperative summary>
```

#### Allowed Types

* `feat` – new user-facing feature
* `fix` – bug fix
* `refactor` – behavior-preserving refactor
* `test` – tests only
* `docs` – documentation only
* `chore` – tooling, CI, maintenance

#### Rules

* Keep commit messages **short and explicit**
* One logical change per commit
* Avoid vague messages (`update stuff`, `misc fixes`)
* Use imperative mood

#### Examples

```
feat(upload): support album path joiner flag
fix(exif): handle timezone-less timestamps correctly
refactor(cli): isolate flag parsing from execution
test(upload): cover dry-run behavior on failures
docs(cli): document new --folder-as-tags option
```

---

### 10.2 User-Facing Changes (Required)

If a commit introduces:

* a CLI flag change
* a new feature
* a behavior change
* a breaking or potentially surprising change

👉 **The commit message and PR description must explicitly mention it.**

Example:

```
feat(cli): add --date-range flag

User impact:
- Allows restricting imports to a date interval
- No change to default behavior
```

---

### 10.3 Pull Request Descriptions

PR descriptions must be:

* Lean
* Informative
* User-oriented

#### Recommended PR Structure

```
### Summary
Short explanation of what and why.

### User-facing changes
- New flag: --xyz
- Behavior change: ...

### Technical notes
- Tests added for ...
- Refactor limited to ...

### Compatibility
- Backward compatible / Breaking change (explain)
```

Avoid long prose. Favor **clear bullet points**.

---

## 11. Progress Tracking and Plan Updates

For multi-step work:

* Maintain a task list
* Track completed, pending, and blocked steps
* Update the plan when scope or constraints change

---

## 12. immich-go Core Principles

When in doubt, prioritize:

1. Data safety
2. Predictable behavior
3. Explicit user intent
4. Backward compatibility
5. Testability
6. Easy review and merge
7. Clarity over cleverness

---

## 13. Final Rule

If you must choose between:

* speed
* cleverness
* correctness
* mergeability

👉 **Always choose correctness, clarity, and easy mergeability.**

