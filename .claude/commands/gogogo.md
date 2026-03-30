---
description: Start a new Claude Code session - load context, check git status, and prepare for work
---

## Session Startup Checklist

Please perform the following startup tasks to begin this session:

### 1. Environment Setup
- Check if your dev server is running (customize the port for your project)
- If not running, start it in background (e.g., `npm run dev`, `python manage.py runserver`, etc.)
- Confirm the server starts successfully

> **Note:** Customize the dev server command and port for your specific project stack.

### 2. Git Status Check
- Run `git status` to show current branch and any uncommitted changes
- Run `git log -5 --oneline` to show recent commits
- **If there are uncommitted changes:** Ask the user what to do:
  - Commit them now (with a message)
  - Stash them for later
  - Continue without committing
- If there are remote changes (behind origin), pull them with `git pull`

### 3. Load Project Context
Read and internalize the full project context:

**Core Context Files (read but don't summarize verbosely):**
- `.claude/claude.md` - Master context and conflict resolution rules
- `.claude/prd.md` - Product requirements and user stories
- `.claude/infra.md` - Infrastructure and coding conventions

**Status Files (summarize for user):**
- `.claude/changelog.md` - Summarize the most recent entries (what was completed recently)

**Beads Issue Tracking:**
- Run `bd list` to see all tracked issues
- Run `bd ready` to identify available work
- Note any issues with status "in_progress" that may need continuation

### 4. Environment Check
- Verify environment files exist (e.g., `.env.local`, `.env`)
- Note if any environment variables appear to be missing based on example files

### 5. Present Options and STOP

After gathering context, present a summary and **STOP to wait for user input**:

"Session ready! Here's what I found:
- **Recent work:** [Summary from changelog]
- **In progress:** [Any in-progress issues from `bd list`]
- **Ready to start:** [Available issues from `bd ready`]

What would you like to work on?"

**CRITICAL: STOP HERE.** Do NOT automatically:
- Continue on in-progress work
- Start the next ready issue
- Suggest a specific task to begin
- Take any action beyond presenting this summary

Wait for the user to explicitly tell you what they want to work on.

### 6. Route to Next Step (ONLY after user selects)

**Only proceed with this step AFTER the user explicitly tells you what to work on.**

When the user selects a bead:

**If bead is a FEATURE (needs planning):**

Check if bead already has child beads:
```bash
bd list --parent <selected-id>
```

- **No children:** Say "This feature needs a plan. Run `/beads-plan <bead-id>` to break it into implementation steps."
- **Has children:** Say "This feature has a plan. Run `/beads-execute <bead-id>` to start implementation."

**If bead is a TASK or BUG (already atomic):**

Proceed directly:
```bash
bd update <bead-id> --status in_progress
```

Then implement following `beads-workflow` skill.

### 7. Session Workflow Reminder

**Beads-Native Planning:**
- Features need plans: `/beads-plan <bead-id>` before coding
- Execute plans: `/beads-execute <bead-id>` to implement
- Plans persist across sessions in beads

**Iron Rules:**
- NO CODE WITHOUT A BEAD FIRST
- NO FEATURE CODE WITHOUT A PLAN FIRST
- Create beads for discovered work: `bd create "..." --discovered-from <current-id>`
- Close completed work: `bd close <id>`
