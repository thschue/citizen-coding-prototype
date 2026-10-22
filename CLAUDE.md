# Citizen Coding Prototype

ADR operator and MCP server: turns human-written MADR decisions into audited, approved Kyverno / Cilium enforcement that coding agents and SOC analysts can read. The scope lives in epic #1.

## Technology
* Go, Kubebuilder / controller-runtime (operator), Go MCP server
* Kyverno and Cilium as enforcement targets, shipped as a Helm chart
* Tests: envtest for the operator, an in-process MCP client for the MCP server, kind/k3s for end-to-end

## Hygiene
* Every change comes with tests, and they pass. Failing or warning tests are not acceptable
* Quality gate: `make verify` (lint, vet, unit and envtest tests, build) exits 0. Until the scaffold (#2) adds it, run `go build ./... && go vet ./... && go test ./...`
* Fix problems at their root cause. A retry, a longer timeout, a skipped or loosened check, or an ignore entry is a workaround unless the cause is understood and written down
* Remove dead code immediately. Never downgrade versions to make tests pass
* The guardrails in epic #1 are requirements. Each one needs an automated test

## Workflow
* Pick the next work with the query recipe in `docs/agents/issue-tracker.md`. Don't infer next steps from issues or code alone
* Open a draft PR (`Closes #<n>`) before writing most of the code
* PR titles start with a Conventional Commits type (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `build:`, `ci:`, `chore:`, …)
* Keep documentation up to date when implementing
* Record blocking relationships between issues as native GitHub issue dependencies, not as prose

## Agent skills

### Issue tracker
Issues live as GitHub issues in `thschue/citizen-coding-prototype` and are managed with the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels
`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `epic`, `wontfix`, `priority`, `question`. See `docs/agents/triage-labels.md`.

### Domain docs
Single context: `CONTEXT.md` plus `docs/adr/` at the repo root (both created lazily). See `docs/agents/domain.md`.

### Loops
Three loops coordinate only through labels and marked PR comments, so any one of them can restart without the others noticing:

* **Issue worker** (`/issue-worker`, or `/loop 3m /issue-worker`): claims `ready-for-agent` issues, implements each one in an isolated worktree, and hands it off with `awaiting-security-review`. It never merges. See `docs/agents/issue-worker-loop.md`.
* **Security review** (`/security-review-loop`, or `/loop 5m /security-review-loop`): reviews hand-offs, rebases them, runs the gates and `/security-review`, then arms auto-merge, sends the work back (`changes-requested`), or escalates (`needs-info`). It is the only loop that merges, and only through auto-merge. See `docs/agents/security-review-loop.md`.
* **PM audit** (`/pm-audit`, or `/loop 15m /pm-audit`): keeps issues honest against `main`, triages, and keeps the worker fed. It writes no code and merges nothing. See `docs/agents/pm-audit-loop.md`.
