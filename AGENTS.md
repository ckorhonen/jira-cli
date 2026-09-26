# Jira CLI

## Map and setup

`cmd/jira/` is the entry point; `internal/cmd/` implements commands, `internal/config/` reads local settings, and `internal/view/` renders output. `pkg/jira/` and `pkg/jql/` own Jira API/query contracts; `pkg/tui/` and related packages support interaction. `api/` contains API resources.

Use Go 1.24.1 (the `go.mod` toolchain and CI version). `make deps` vendors dependencies; `make build` runs that prerequisite and builds all packages. `go run ./cmd/jira --help` is a CLI startup check. Actual configured Jira commands can read or mutate the user's account; use fixtures for automated verification and keep tokens/configuration out of output.

## Verification

CI runs `make deps`, `make lint`, and `make test`. The test target clears the test cache and runs all packages with the race detector and CGO enabled, so a C toolchain is required. While iterating, run `go test` for the affected package. Lint uses golangci-lint 1.64.7; `make lint` attempts a network installer if it is absent, so establish the tool prerequisite first.

For command changes, verify parsing, rendered output, error/exit behavior, and the request shape without a production account. `make jira.server` starts the Docker Compose Jira service; it is an integration environment, not a unit check. `make distclean` removes shared Go caches and isn't routine cleanup. Preserve existing command conventions and avoid unrelated vendored or dependency changes.

## Finishing work

Follow the nearby implementation and keep changes within the requested scope. Carry authorized changes through the relevant checks, fixing failures caused by the change. For a bug, reproduce the affected behavior and add a focused regression check when useful. Make routine reversible choices without another approval; ask only when missing information materially changes correctness, scope, or authorization, and name the exact source of any blocking rule. Report what changed, checks actually run, and concrete unverified behavior; repeat checks when new edits or evidence warrant it.
