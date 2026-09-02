# CLAUDE.md

Context and conventions for AI assistants (and humans) working in this
repository.

## What this repo is

A template for new Go applications — a standard project layout plus
CI/CD, linting, and contributor scaffolding. Code under `src/` is a
placeholder demonstrating the layout, not a real application.

## Project structure

```
src/
  cmd/app/       main package — binary entrypoint
  internal/      private packages, not importable outside this module
    database/    data access: connections, queries, repositories
    service/     business logic, independent of API vs. script
    request/     input/output boundary: HTTP handlers or CLI args
    utilities/   small shared helpers with no business logic
tests/           integration/end-to-end tests
.github/         workflows, issue templates, PR template
```

- Unit tests live next to the code they test (`foo_test.go` beside
  `foo.go`), per Go convention.
- `tests/` is reserved for integration/end-to-end tests that exercise
  multiple packages or the built binary together.
- New packages go under `src/internal/` unless they are meant to be
  imported by other modules, in which case use `src/pkg/`.
- Each project instantiated from this template is single-purpose (one
  API or one script), so `src/cmd/` holds a single entrypoint. Keep
  logic layered — `request` (I/O boundary) calls `service` (business
  logic) calls `database`/`utilities` — and only add the layers a given
  project actually needs; don't stub out empty packages "for later".

## Commands

- Build: `go build ./...`
- Run: `go run ./src/cmd/app`
- Test: `go test ./...`
- Format: `gofmt -l .` (fix with `gofmt -w .`)
- Vet: `go vet ./...`

## Conventions

- Go version: see `go.mod` (currently 1.24).
- Format every change with `gofmt` before committing.
- Prefer the standard library; justify new dependencies in the PR
  description.
- Exported identifiers need Go doc comments; avoid comments that
  restate what the code already says.
- Follow [Keep a Changelog](https://keepachangelog.com/) conventions in
  `CHANGELOG.md` when a change is user-facing.

## CI/CD

- `.github/workflows/build.yaml` — build, vet, and test on every push
  and pull request.
- `.github/workflows/deploy.yaml` — build and publish a container image
  on release/tag.
