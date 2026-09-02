# golang-template

Template repository for new Go applications, providing a standard project
layout, tooling, and CI/CD configuration to build from.

## Description

This repository is not an application itself — it's a starting point.
Copy it (or use it as a GitHub template) when creating a new Go service or
CLI, then replace the placeholder code under `src/` with real application
logic.

## Setup Instructions

1. Ensure [Go](https://go.dev/dl/) 1.24 or later is installed.
2. Clone the repository:
   ```sh
   git clone https://github.com/kevindowdy/golang-template.git
   cd golang-template
   ```
3. Download dependencies:
   ```sh
   go mod download
   ```

## Run Instructions

Run the application directly:

```sh
go run ./src/cmd/app
```

Build a binary:

```sh
go build -o bin/app ./src/cmd/app
./bin/app
```

Run tests:

```sh
go test ./...
```

## Project Structure

```
src/                  Application source code
  cmd/app/            Binary entrypoint (main package)
  internal/           Private packages not importable by other modules
    database/         Data access: connections, queries, repositories
    service/          Business logic, independent of API vs. script
    request/          Input/output boundary: HTTP handlers or CLI args
    utilities/        Small shared helpers with no business logic
tests/                Integration/end-to-end tests
.github/              CI/CD workflows and issue/PR templates
```

`request` calls into `service`, which calls into `database` and
`utilities`. This split holds whether the binary is an API (`request`
holds HTTP handlers) or a script (`request` holds CLI flag parsing) —
each project instantiated from this template is single-purpose, so keep
only the layers you actually need.

Unit tests live alongside the package they test (Go convention); `tests/`
holds broader integration tests.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Licensed under the terms in [LICENSE](LICENSE).
