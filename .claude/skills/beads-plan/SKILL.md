---
name: beads-plan
description: Use when starting work on a feature bead from PRD, before writing any code. Use when bead needs breakdown into implementation steps.
---

# Beads Plan

## Overview

Break a feature bead into implementation sub-tasks. The plan lives in beads, not markdown files.

**Announce at start:** "I'm using beads-plan to break this feature into implementation steps."

## The Iron Rule

```
NO CODE WITHOUT A PLAN FIRST
```

When picking up a feature bead:

1. **STOP** - Do not implement yet
2. **EXPLORE** - Understand the codebase
3. **DESIGN** - Determine approach and steps
4. **BREAK DOWN** - Create child beads
5. **DOCUMENT** - Add notes to parent bead
6. **THEN STOP** - User reviews plan before execution

## The Process

### Step 1: Load Feature Context

```bash
bd show <bead-id>          # Read the feature bead
```

Also read relevant PRD sections and context files to understand requirements.

### Step 2: Explore Codebase

Investigate to understand:
- What files need to change?
- What patterns exist to follow?
- What dependencies are involved?
- What tests need writing?

Use Task tool with Explore agent for thorough investigation.

### Step 3: Design Approach

Determine:
- Implementation sequence (what depends on what)
- Key files to modify/create
- Testing strategy
- Any risks or blockers

### Step 4: Create Child Beads

Break into atomic, testable steps. Each child should be completable in one focused session.

```bash
# Create children with --parent
bd create "Add X to file Y" --type task --parent <parent-id>
bd create "Write tests for X" --type task --parent <parent-id>
bd create "Update config for X" --type task --parent <parent-id>

# Set dependencies if order matters
bd dep add <later-bead> <earlier-bead>
```

**Granularity guide:**
- Too big: "Implement TTFI measurement"
- Right size: "Add synthPerfDump polling loop"
- Right size: "Extract TTFI from dump response"
- Right size: "Send TTFI to Grafana metrics"

### Step 5: Document Approach

Add implementation notes to parent bead:

```bash
bd update <parent-id> --notes "$(cat <<'EOF'
## Approach
- Brief description of implementation strategy

## Key Files
- path/to/file.js - what changes here
- path/to/other.js - what changes here

## Risks/Notes
- Any gotchas or considerations
EOF
)"
```

### Step 6: Present Plan and STOP

Show the user:
- List of child beads created
- Dependency order
- Parent bead notes (the approach)

**CRITICAL: STOP HERE.**

Say: "Plan created. Child beads are ready for execution. Review and run `/beads-execute <parent-id>` when ready."

Do NOT:
- Start implementing
- Mark any beads in_progress
- Write any code

## Red Flags - STOP

- Jumping to code without breaking down the feature
- Creating only 1-2 child beads for a complex feature
- Skipping codebase exploration
- Not adding notes to parent bead
- Starting execution without user approval

## Session Boundary

This skill is designed to END a session. The plan persists in beads.

Next session can pick up with `/beads-execute <parent-id>`.
