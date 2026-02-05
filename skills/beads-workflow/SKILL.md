---
name: beads-workflow
description: MUST invoke BEFORE any implementation work. Use when user requests features, bug fixes, or changes. Use before writing ANY code. Use when wrapping up sessions. Triggers: "implement", "add", "fix", "build", "create", "update", "refactor", "let's wrap up", "done for today", session start, session end.
---

# Beads Workflow

This skill contains all workflow rules for using beads in this project.

## Core Principle

**Track work in beads BEFORE implementing.** Every non-trivial task gets a bead. Claim it, do it, close it.

## The Iron Rule

```
NO CODE WITHOUT A BEAD FIRST
```

When user requests work ("implement X", "add Y", "fix Z"):

1. **STOP** - Do not write any code yet
2. **CREATE** - `bd create "Title" --type feature|task|bug`
3. **CLAIM** - `bd update <id> --status in_progress`
4. **THEN CODE** - Only now may you implement

**No exceptions:**
- Not for "quick fixes"
- Not for "simple changes"
- Not for "I'll create the bead after"
- Not for "let me just explore first"

If you wrote code without a bead, you violated this rule. Create the bead now.

**Why this matters:** Beads provide audit trails, prevent duplicate work, enable handoffs, and ensure nothing is lost across sessions.

## 1. Session Startup

Orient yourself with current work state:

```bash
bd ready              # Unblocked work ready to claim
bd list --status open # All open issues
bd stale --days 7     # Neglected issues
```

Review: What's ready? What's in progress? Any blockers?

## 2. Planning with Beads

### Create Fine-Grained Issues

Smaller issues = better decisions, cheaper sessions.

```bash
bd create "Implement auth endpoint" --type feature
bd create "Add auth tests" --type task
bd create "Fix login crash" --type bug -p 0
```

### Issue Types

| Type | Use For |
|------|---------|
| `bug` | Defects, errors, crashes |
| `feature` | New functionality |
| `task` | General work items |
| `epic` | Large initiatives (parent of multiple issues) |
| `chore` | Maintenance, docs, cleanup |

### Priority Levels

| Flag | Meaning | Example |
|------|---------|---------|
| `-p 0` | Critical | Production down, security |
| `-p 1` | High | Blocking other work |
| `-p 2` | Medium | Normal work (default) |
| `-p 3` | Low | Nice to have |
| `-p 4` | Backlog | Future consideration |

### Issue Granularity

- **Too big:** "Implement user authentication system"
- **Right size:** "Add JWT token validation middleware"
- **Right size:** "Create login API endpoint"

### Issue Dependencies

```bash
bd create "Fix DB connection" --blocks AES-42
bd create "Add validation" --parent AES-40
bd create "Fix flaky test" --discovered-from AES-42
```

### User-Requested Work During Sessions

When user requests new work during active session:

1. **Pause implementation** - Don't code immediately
2. **Create bead(s)** - One per discrete piece
3. **Link appropriately** - `--parent` or `--discovered-from`
4. **Confirm with user** - Show created beads
5. **Mark in_progress** - Then implement

| Scenario | Flag |
|----------|------|
| Enhancement to in-progress work | `--parent <current-id>` |
| Related but independent | `--discovered-from <current-id>` |
| Extends closed issue | `--discovered-from <closed-id>` |
| Completely new work | (no flag) |

**Exception:** Skip bead for typos, minor tweaks, scope clarifications.

### Labels

```bash
bd create "Add dark mode" --type feature -l "frontend,ui"
bd label add AES-42 "urgent"
bd list --label "frontend"
```

### Issue Naming Convention

- `Implement [component/feature]`
- `Add [functionality] to [area]`
- `Fix [bug description]`
- `Update [docs/config] for [change]`

### Filtering Issues

```bash
bd list --status open --type bug
bd list --title-contains "auth"
bd list --no-assignee
bd list --label "frontend" --priority-max 1
```

## 3. Branching Strategy

### Branch Naming

```
feature/<work-id>-<short-description>
fix/<work-id>-<short-description>
```

### Starting Work Session

```bash
WORK_ID="work-$(date +%Y%m%d-%H%M)"
git checkout -b feature/${WORK_ID}-<description>
```

## 4. Execution Process

### Issue Statuses

| Status | Meaning |
|--------|---------|
| `open` | Ready to start (default) |
| `in_progress` | Currently being worked |
| `blocked` | Waiting on dependency |
| `deferred` | Postponed |
| `closed` | Completed |

### Claim and Execute

```bash
bd update <id> --status in_progress
# ... perform the work ...
# ... run code quality review (see below) ...
bd close <id>
```

### Code Quality Review (REQUIRED for JS/TS)

**MUST run before closing any bead that touched JS/TS files** (`.js`, `.ts`, `.jsx`, `.tsx`, `.mjs`, `.cjs`).

Dispatch the clean-code-reviewer subagent:
```
Tool: Task
Parameters:
  subagent_type: "clean-code-reviewer"
  prompt: "Review recent changes for Clean Code principles. Focus on files modified for <bead-id>: <description>"
```

**Wait for review results:**
- Issues found → fix them, re-run reviewer until approved
- Approved → proceed to close

**Skip ONLY for:** docs-only (`.md`), pure config (`.json`/`.yaml` with no logic), non-JS/TS files.

**If unsure whether to skip:** Run the review. It's cheap insurance.

### Handling Blocked Work

```bash
bd update AES-42 --status blocked
bd comment AES-42 "Waiting on API team for endpoint spec"
bd update AES-42 --status open  # When unblocked
```

### Reopening Issues

```bash
bd reopen AES-42 --reason "Bug reappeared after deploy"
```

### Work Discovery

Capture discovered issues immediately:

```bash
bd create "Fix broken auth tests" --discovered-from AES-42 --type bug
bd create "Refactor duplicated logic" --discovered-from AES-42 --type task
```

### On Failure

1. Do NOT close the issue
2. Add context: `bd comment <id> "Error: [description]"`
3. Create follow-up issues if needed
4. Seek user guidance

## 5. Self-Optimization

After significant work, analyze:

- Did user correct your approach?
- Discover undocumented conventions?
- Command need extra flags?

Capture as beads:
```bash
bd create "Update [file] to document [pattern]" --type chore
```

## 6. Code Commit Workflow

Use `/conventional-commits` skill for commit format.

```bash
git add .
git commit -m "feat: [description]"
bd sync
git push
```

Update `changelog.md` after completing feature sets.

## 7. Multi-Agent Coordination

### Actor Tracking

```bash
bd update AES-42 --status in_progress --actor "claude-session-1"
bd close AES-42 --actor "claude-session-1"
```

### Assignee Management

```bash
bd update AES-42 --status in_progress --assignee "agent-1"
bd list --assignee "agent-1"
bd ready --assignee ""  # Unclaimed work
```

## 8. Testing Workflow

```bash
cd synthetic && ./run-local.sh preprod
```

Install k6 if needed: `brew install k6`

## 9. Session Completion

**Complete ALL steps before ending:**

1. File issues for remaining/discovered work
2. Run quality gates (tests, linters) if code changed
3. Close completed issues: `bd close <id>`
4. Sync beads: `bd sync --from-main`
5. Commit and push:
   ```bash
   git add . && git commit -m "..."
   git push -u origin <branch-name>
   ```
6. If work continues: leave branch open, document progress
7. Hand off context for next session

## Red Flags - STOP

- Starting to code without claiming a bead
- User requests new work and you dive straight in
- Session ending without closing completed beads
- Discovering issues and not capturing them
- Skipping `bd sync --from-main` on ephemeral branches
- **Skipping code quality review** for JS/TS work before closing

**All mean: Pause. Create/update beads first (or run review before closing).**
