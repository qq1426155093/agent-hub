# Repository Guidelines

## Project Structure & Module Organization

This repository is currently a design-stage Go project containing `README.md`; there is no `go.mod`, implementation, test suite, or asset directory yet. Read the README before changing architecture or protocols.

The planned executable entry points are `cmd/agent-manager/`, `cmd/agent-daemon/`, and `cmd/agent-dispatch/`. Keep entry points thin; place implementation and shared protocol types under `internal/` as packages emerge. Place Go tests beside the code they exercise.

## Architecture Boundaries

- **Manager:** owns service registration, instance leases, Agent definitions, capability descriptions, configuration, and routing publication.
- **Daemon:** checks local service readiness and maintains registration, heartbeats, and deregistration.
- **Dispatch:** subscribes to complete routing views, authenticates clients, manages conversations, and forwards calls directly to service instances.

Preserve the mapping `agent-id → agent-service-name → available instance`. Capability descriptions are metadata; changing them does not change service behavior. Dispatch routes from its local snapshot, subject to freshness and lease limits.

## Build, Test, and Development Commands

No build scripts or runnable programs exist yet. After module initialization and implementation, use these proposed commands from the repository root:

- `go build ./...`: compile all packages.
- `go test ./...`: run the test suite.
- `go test -race ./...`: check concurrent execution for data races.
- `go vet ./...`: run standard static checks.
- `go run ./cmd/agent-manager`: run Manager; use the corresponding paths for Daemon and Dispatch once implemented.

Document required configuration and dependencies when adding runnable components.

## Coding Style & Naming Conventions

Format Go code with `gofmt`, using its tab indentation. Use lowercase package names, exported `PascalCase` identifiers, and unexported `camelCase` identifiers. Keep protocol identifiers consistent with the README. Maintain the existing Chinese README terminology and clearly distinguish implemented behavior from proposals.

## Testing Guidelines

Use Go's standard `testing` package, `*_test.go` files, and `TestXxx` functions. No coverage percentage is established. Prioritize lease expiry, stale-lease rejection, subscription reconnects, snapshot freshness, routing, cross-Dispatch deduplication, and timeout behavior. Test observable outcomes and report validation limits explicitly.

## Commit & Pull Request Guidelines

This checkout has no Git history, so no existing commit convention can be verified. Prefer scoped Conventional Commits, such as `docs: clarify routing subscriptions`. Keep changes focused. PRs should explain the problem, behavior changes, validation, and protocol or configuration impacts; link relevant issues. Update the README when changing architectural decisions.
