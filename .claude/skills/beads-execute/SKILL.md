---
name: beads-execute
description: Use when executing a planned feature bead that has child tasks. Use after /beads-plan has created the implementation breakdown.
---

# Beads Execute

## Overview

Execute child beads of a planned feature, one at a time, with review between batches.

**Announce at start:** "I'm using beads-execute to implement the planned tasks."

## Prerequisites

Before using this skill:
- Parent bead should have child beads (created by `/beads-plan`)
- Parent bead should have notes describing approach
- No code should have been written yet

If prerequisites not met, suggest running `/beads-plan <bead-id>` first.

## The Process

### Step 1: Load Plan Context

```bash
bd show <parent-id>        # Read parent with notes
bd list --parent <parent-id>  # See all child beads
```

Review:
- Approach notes from parent bead
- Child bead order and dependencies
- Any blocked children

### Step 2: Claim Parent

```bash
bd update <parent-id> --status in_progress
```

### Step 3: Execute First Batch

**Default: First 3 unblocked child beads**

For each child:

1. **Claim it:**
   ```bash
   bd update <child-id> --status in_progress
   ```

2. **Implement:** Follow the task description

3. **Test:** Verify the implementation works

4. **Close it:**
   ```bash
   bd close <child-id>
   ```

### Step 4: Report and Pause

After batch complete:

```
Batch complete:
- [x] <child-id>: <description>
- [x] <child-id>: <description>
- [x] <child-id>: <description>

Remaining:
- [ ] <child-id>: <description>
- [ ] <child-id>: <description>

Ready for review. Continue with next batch?
```

**WAIT for user feedback before continuing.**

### Step 5: Continue or Adjust

Based on feedback:
- Fix issues if needed
- Execute next batch
- Repeat until all children complete

### Step 6: Close Parent

When all children complete:

```bash
bd close <parent-id>
```

Report: "Feature complete. All tasks finished."

## Handling Blockers

If blocked mid-task:

1. **Don't force it** - stop and report
2. **Add context:**
   ```bash
   bd comment <child-id> "Blocked: <description>"
   bd update <child-id> --status blocked
   ```
3. **Ask for guidance**

## Work Discovery

If you discover new work during implementation:

```bash
bd create "Fix unexpected issue" --discovered-from <current-id> --type bug
```

Don't get sidetracked - capture and continue.

## Session Boundary

If session needs to end mid-execution:

1. Close completed children
2. Leave current child as in_progress with comment about state
3. Leave parent as in_progress
4. Sync beads: `bd sync --from-main`
5. Commit work: `git add . && git commit -m "wip: partial implementation"`

Next session picks up with same command: `/beads-execute <parent-id>`

## Red Flags - STOP

- Implementing without checking plan first
- Skipping beads or doing them out of dependency order
- Not closing completed children
- Continuing past batch without user check-in
- Force-completing blocked work

## Integration with Other Skills

After all work complete, consider:
- `bd sync --from-main` to sync beads
- `/wrapup` for full session close
