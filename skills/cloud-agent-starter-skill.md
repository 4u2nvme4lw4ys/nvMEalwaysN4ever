# Cloud Agent Starter Skill: Run + Test Quickstart

Use this skill when you need to get productive fast in this repository from a fresh Cloud agent session.

## 1) Fast reality check (run first)

This repo is currently minimal (`README.md`, `LICENSE`, `.gitignore`) and has no runnable app/service code yet.

### Practical implications right now
- **Login:** Not required (no auth flow exists in-repo yet).
- **Start app:** Not available yet (no `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, or compose files).
- **Feature flags:** None defined yet in tracked code.

### Commands to confirm current state
- `ls -la`
- `rg -n "package.json|pyproject.toml|go.mod|Cargo.toml|docker-compose" .`
- `git status --short`

---

## 2) Codebase area workflows

Organize your work by area and run only the checks that prove your changes in that area.

### Area A: Repository root/docs (`README.md`, `LICENSE`, `.gitignore`)

Use when changing project docs, policy notes, or root metadata.

**Setup**
- No service startup needed.
- No login needed.

**Concrete testing workflow**
1. Validate files are present and readable:
   - `ls -la`
2. Validate intended text updates:
   - `rg -n "<keyword-you-edited>" README.md`
3. Validate clean change scope:
   - `git diff -- README.md LICENSE .gitignore`

**Success signal**
- Diff matches intended text edits only.

### Area B: App/service runtime areas (when introduced)

Expected future paths: `apps/`, `services/`, `backend/`, `frontend/`, `packages/`.

**Setup discovery**
1. Find run manifests:
   - `rg -n "\"scripts\"|pytest|uvicorn|flask|django|go test|cargo test" --glob "*{package.json,pyproject.toml,go.mod,Cargo.toml}"`
2. Read local runbook files:
   - `rg -n "run|start|dev|test|login|auth|feature flag|env" --glob "README*.md"`

**Login/auth guidance**
- Prefer local/dev auth paths first (seed user, test account, mocked auth).
- If real credentials are required, use environment variables only (never hardcode secrets).
- Document in PR notes exactly how login was performed (or why it was mocked).

**Start-app workflow template**
1. Install deps with the project package manager in that area.
2. Start only required services first (DB/cache/api), then app.
3. Keep startup logs visible and verify readiness endpoint or startup banner.

**Feature-flag workflow template**
1. Identify flag source (`env`, config file, DB, or remote provider shim).
2. Prefer local override (`export FLAG_X=true`) or test fixture.
3. State the exact flag values used in your test evidence.

**Concrete testing workflow**
1. Reproduce baseline behavior before change.
2. Run targeted automated tests closest to edited code.
3. Run one end-to-end/manual happy path for the changed feature.
4. Re-run with opposite flag state when flags are involved.

**Success signal**
- Targeted tests pass and the changed flow is verified with explicit config/flag state.

### Area C: CI/workflow configs (when introduced, e.g. `.github/workflows/`)

Use when touching CI scripts, job matrices, or check definitions.

**Setup**
- No app login usually required.
- Validate syntax and command references locally where possible.

**Concrete testing workflow**
1. Inspect changed workflow command paths:
   - `rg -n "run:|uses:|working-directory|matrix" .github/workflows`
2. Dry-run equivalent local commands from repo root.
3. Confirm referenced files/scripts exist.

**Success signal**
- Every CI step maps to a locally valid command/path.

---

## 3) Cloud-agent execution defaults

- Prefer targeted checks over full-suite runs.
- Keep long-running services in tmux sessions.
- Capture exact commands + outputs used to validate changes.
- If a step is not possible in Cloud, record the blocker and the nearest alternative evidence.

---

## 4) Updating this skill with new runbook knowledge

When you discover a new reliable setup/testing trick, update this file in the same PR (or immediate follow-up PR).

Use this mini-template:
1. **Context:** Which codebase area it applies to.
2. **Trigger:** When to use the trick.
3. **Steps:** Copy-pasteable commands in order.
4. **Evidence:** What output proves success.
5. **Failure mode:** Most common pitfall + quick fix.

Keep additions short, concrete, and command-first.
