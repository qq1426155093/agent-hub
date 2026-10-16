# Repository Guidelines

## Project Structure & Module Organization

This is a design-stage Go project. `README.md` introduces the project; `docs/README.md` indexes detailed documentation. Read `docs/architecture.md` for responsibilities, `docs/roadmap.md` for scope, and `docs/protocol-design.md` for deferred protocol topics. There is no `go.mod`, implementation, test suite, or asset directory yet.

Planned entry points are `cmd/agent-manager/`, `cmd/agent-daemon/`, and `cmd/agent-dispatch/`. Keep entry points thin; place implementation, adapters, and shared types under `internal/`. Keep tests beside their packages.

## Architecture Boundaries

- **Manager:** owns registration, leases, Agent definitions, capability descriptions, configuration, and routing publication.
- **Daemon:** is an Agent service instance that wraps existing agents such as Codex and Claude, registers itself, and manages native sessions, execution resources, and backend adapters.
- **Dispatch:** subscribes to routing views, authenticates clients, maintains platform sessions and instance bindings, and forwards calls and events using the standard protocol.

Third-party services implement the protocol directly without Daemon. Preserve `agent-id → agent-service-name → instance`. Bind existing sessions to their execution instance; do not assume chat history restores native state or workspaces. Distinguish descriptions from actual protocol capabilities. Protocol details remain deferred; do not treat proposed fields, endpoints, or transports as settled.

## Build, Test, and Development Commands

No runnable programs exist yet. After module initialization and implementation, run from the repository root:

- `go build ./...`: compile all packages.
- `go test ./...`: run the test suite.
- `go test -race ./...`: check concurrent execution for data races.
- `go vet ./...`: run standard static checks.
- `go run ./cmd/agent-manager`: run Manager; substitute the other entry points as needed.

Document required configuration and dependencies when adding runnable components.

## Coding Style & Naming Conventions

Use `gofmt` and its tab indentation, lowercase package names, exported `PascalCase`, and unexported `camelCase`. Keep documentation in Chinese with relative links. Keep the README concise; maintain details under `docs/` and update its index. Distinguish decisions, proposals, and implemented behavior.

## Testing Guidelines

Use Go's `testing` package, `*_test.go` files, and `TestXxx` functions. No coverage threshold exists. Cover leases, subscriptions, routing freshness, adapter execution, third-party compatibility, session affinity, cross-Dispatch deduplication, and unknown outcomes. Report validation limits explicitly.

## Commit & Pull Request Guidelines

Follow the existing Conventional Commit style, for example `docs: clarify routing subscriptions`. Keep changes focused. PRs should describe behavior, validation, and protocol or configuration impacts; link relevant issues. Update architectural documentation when responsibilities change.
